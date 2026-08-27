# 4. 모델 커스터마이징 (파인튜닝) · Model Customization (Fine-Tuning)

> 나만의 데이터·용도에 맞게 모델을 손보는 방법들. 공통 질문은 **"무엇을 학습하고 무엇을 얼리는가(freeze)"**.
> 출처 근거: [research/SOURCES.md](../research/SOURCES.md) 카테고리 4. 서식: [STYLE.md](../STYLE.md).

---

### Base model vs Instruct model · 베이스 모델과 인스트럭트 모델

> **한 줄 요약:** 사전학습만 끝난 모델과, 지시를 따르도록 추가로 다듬어진 모델. 파인튜닝을 시작할 때 무엇을 출발점으로 삼을지의 첫 갈림길이다.

**정의 (Definition)**
- KO: **베이스 모델**은 대규모 말뭉치로 다음 단어를 예측하도록 사전학습만 마친 상태의 모델. **인스트럭트 모델**은 그 위에 지도 파인튜닝·정렬 과정을 거쳐 지시 이행과 대화에 맞춰진 모델이다.
- EN: A *base* model has only been pretrained (next-token prediction on a large corpus); an *instruct* model has additionally been fine-tuned and aligned to follow instructions and behave as an assistant.

**비유 (쉽게):** **읽기를 아주 많이 한 사람과, 그 위에 접객 교육을 받은 사람**의 차이다. 전자는 아는 게 많아도 질문에 답하는 방식이 몸에 배지 않았고, 후자는 "무엇을 물으면 어떻게 답한다"가 훈련돼 있다.

**왜 중요한가 / 언제 쓰나:**
- **파인튜닝 출발점 선택이 결과를 좌우한다.** 형식·톤을 처음부터 내 데이터로 잡고 싶으면 베이스가, 일반적인 지시 이행을 유지한 채 특화만 얹고 싶으면 인스트럭트가 유리한 경우가 많다.
- 베이스 모델에 프롬프트만 주면 기대와 다른 출력이 나오기 쉽다 — 지시를 따르는 습관이 학습돼 있지 않기 때문이다.
- 모델 배포처의 이름표(`-base`, `-instruct`, `-chat`)로 구분되는 경우가 많지만, **명명 규칙은 배포처마다 다르다.**

**실무 예시 / AI에게 이렇게 말한다:**
- "이 과제를 튜닝할 건데 베이스 모델과 인스트럭트 모델 중 무엇에서 출발할지, 각각의 장단점을 데이터 규모와 함께 설명해줘."

**흔한 오해:**
- **"인스트럭트가 항상 더 좋다"** — 아니다. 이미 입혀진 응답 습관이 내 형식과 충돌하면 오히려 방해가 된다.
- **"베이스 = 성능이 낮은 모델"** — 규모·능력의 문제가 아니라 **후속 학습 단계의 차이**다.
- **"둘은 다른 모델"** — 인스트럭트는 대개 같은 베이스에서 갈라져 나온 것이다.

**함께 보기:** [Pretraining](01-llm-basics.md#pretraining--사전학습), [SFT](04-finetuning.md#sft--지도-파인튜닝-supervised-fine-tuning), [Instruction Tuning](04-finetuning.md#instruction-tuning--인스트럭션-튜닝), [Alignment](05-alignment-rl.md), [RLHF](05-alignment-rl.md#rlhf--인간-피드백-기반-강화학습-reinforcement-learning-from-human-feedback)

**출처:** Anthropic, *Glossary — Pretraining*, [platform.claude.com/docs](https://platform.claude.com/docs/en/about-claude/glossary) ("These pretrained models are not inherently good at answering questions or following instructions, and often require deep skill in prompt engineering to elicit desired behaviors. Fine-tuning and RLHF are used to refine these pretrained models"; 같은 문서 *Fine-tuning* 항목 "Claude is not a bare language model; it has already been fine-tuned to be a helpful assistant"; 확인 2026-08-27). (일반 개념 — 유일 창시 없음.)

---

### Training / Validation / Test split · 학습·검증·테스트 분리

> **한 줄 요약:** 배운 데이터로 채점하지 않기 위해 데이터를 세 몫으로 나누는 것. 파인튜닝 결과를 믿을 수 있게 만드는 최소 장치다.

**정의 (Definition)**
- KO: 데이터를 **학습셋**(모델이 학습하는 몫)·**검증셋**(학습 중 초기 점검과 하이퍼파라미터 조정용)·**테스트셋**(학습이 끝난 모델의 최종 평가용)으로 나누는 것.
- EN: Splitting data into a training set the model trains on, a validation set that performs initial testing while training, and a test set for evaluation of the trained model.

**비유 (쉽게):** **연습문제·모의고사·실제 시험**이다. 연습문제로 배우고, 모의고사로 공부 방법을 조정하고, 실제 시험은 마지막에 딱 한 번 본다. 모의고사를 반복해 보면 그 문제에 익숙해질 뿐이다.

**왜 중요한가 / 언제 쓰나:**
- **2분할보다 3분할을 권하는 이유가 분명하다** — 검증셋 없이 테스트셋으로 반복 조정하면 모델이 테스트셋의 특성에 맞춰지고, 테스트셋이 독립 평가 도구로서의 신뢰를 잃는다.
- 파인튜닝에서 [과적합](04-finetuning.md#overfitting--과적합)과 [파국적 망각](04-finetuning.md#catastrophic-forgetting--파국적-망각)을 잡아내는 유일한 실용 수단이다.
- **"Loss가 내려갔다"는 품질 지표가 아니다.** 학습 손실은 학습셋에서 잰 값이다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 데이터셋을 학습·검증·테스트로 나누고, 테스트셋은 마지막 한 번만 쓰도록 절차를 만들어줘."

**흔한 오해:**
- **"데이터가 적으니 다 학습에 쓰는 게 낫다"** — 그러면 잘됐는지 알 방법이 사라진다. 평가할 수 없는 개선은 개선인지 알 수 없다.
- **"테스트셋을 여러 번 봐도 된다"** — 볼수록 그 셋에 맞춰진다. 교사가 시험 문제를 가르치는 것과 같다.
- **"검증셋과 테스트셋은 같은 말"** — 역할이 다르다. 검증은 조정용, 테스트는 최종 확인용이다.

**함께 보기:** [Overfitting](04-finetuning.md#overfitting--과적합), [Evals](07-dev-stages.md#evals--평가벤치마크-evaluation--benchmarks), [SFT](04-finetuning.md#sft--지도-파인튜닝-supervised-fine-tuning), [Catastrophic forgetting](04-finetuning.md#catastrophic-forgetting--파국적-망각)

**출처:** Google, *Machine Learning Crash Course — Dividing the original dataset*, [developers.google.com](https://developers.google.com/machine-learning/crash-course/overfitting/dividing-datasets) ("a training set that the model trains on", "a validation set performs the initial testing on the model as it is being trained", "a test set for evaluation of the trained model"; "The more often you use the same test set, the more likely the model closely fits the test set."; 확인 2026-08-27). (표준 ML 개념 — 유일 창시 없음.)

---

### Full fine-tuning · 전체 파인튜닝

> **한 줄 요약:** 모델의 모든 가중치를 다시 학습하는 방식. [PEFT](04-finetuning.md#peft--파라미터-효율-파인튜닝-parameter-efficient-fine-tuning)가 피하려는 바로 그 비용이다.

**정의 (Definition)**
- KO: 사전학습된 모델의 **전체 파라미터를 다시 학습**하는 파인튜닝. 모델이 커질수록 실행 가능성이 떨어지고, 과제마다 별도 사본을 배포해야 해 비용이 급격히 커진다.
- EN: Fine-tuning that retrains all model parameters; it "becomes less feasible" as models grow, and deploying independent fine-tuned instances is prohibitively expensive at scale.

**비유 (쉽게):** 메뉴 하나 바꾸려고 **주방 설비를 통째로 새로 들이는 것**. 되긴 되고 결과도 좋지만, 지점마다 주방을 새로 지어야 한다면 감당이 안 된다.

**왜 중요한가 / 언제 쓰나:**
- **PEFT의 대조군**이다. 이 항목이 없으면 "일부만 학습한다"는 말의 기준점이 사라진다.
- 학습해야 할 것은 가중치만이 아니다 — **옵티마이저 상태·그래디언트·활성화값**이 함께 메모리에 올라가므로, "가중치가 몇 GB니까 그만큼이면 되겠지"라는 추정이 크게 빗나간다.
- 그럼에도 선택지로 남는 경우가 있다 — 데이터가 충분하고, 과제가 하나이며, 장비가 받쳐 주고, 최고 품질이 필요할 때.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 과제를 전체 파인튜닝으로 하면 필요한 GPU 메모리를 가중치·옵티마이저·그래디언트·활성화값으로 나눠서 추산해줘."

**흔한 오해:**
- **"전체를 학습하니 항상 더 좋다"** — LoRA 논문은 학습 파라미터를 대폭 줄이고도 "**on-par or better**"인 결과를 보고한다. 과제·데이터에 따라 갈린다.
- **"가중치 크기 = 필요한 메모리"** — 아니다. 학습 시에는 옵티마이저 상태 등이 더해져 몇 배가 된다.
- **"파인튜닝이면 다 전체 파인튜닝"** — 실무에서 파인튜닝이라 부르는 것의 상당수는 PEFT 계열이다.

**함께 보기:** [PEFT](04-finetuning.md#peft--파라미터-효율-파인튜닝-parameter-efficient-fine-tuning), [LoRA](04-finetuning.md#lora--로라저계수-적응-low-rank-adaptation), [SFT](04-finetuning.md#sft--지도-파인튜닝-supervised-fine-tuning), [Catastrophic forgetting](04-finetuning.md#catastrophic-forgetting--파국적-망각)

**출처:** Hu et al. (2021), *LoRA: Low-Rank Adaptation of Large Language Models*, [arXiv:2106.09685](https://arxiv.org/abs/2106.09685) ("full fine-tuning, which retrains all model parameters, becomes less feasible"; "deploying independent instances of fine-tuned models, each with 175B parameters, is prohibitively expensive"; 확인 2026-08-27). 보조 — Hugging Face, *PEFT documentation*, [huggingface.co/docs/peft](https://huggingface.co/docs/peft/index) ("without fine-tuning all of a model's parameters because it is prohibitively costly"). (표준 개념 — 유일 창시 없음.)

---

### PEFT · 파라미터 효율 파인튜닝 (Parameter-Efficient Fine-Tuning)

> **한 줄 요약:** 모델 전체를 다시 학습하는 대신, 아주 일부 파라미터만 손봐서 같은 효과를 훨씬 싸게 얻는 방법들의 총칭.

**정의 (Definition)**
- KO: 사전학습된 대형 모델의 파라미터 대부분을 그대로 둔 채, 소수의 (추가) 파라미터만 학습해 하위 과제에 적응시키는 기법군. 계산·저장 비용을 크게 줄이면서 전체 파인튜닝에 준하는 성능을 목표로 한다.
- EN: A family of methods that fine-tune only a small number of (extra) model parameters instead of all of them — significantly decreasing computational and storage costs — while yielding performance comparable to a fully fine-tuned model.

**비유 (쉽게):** 건물 전체를 리모델링하는 대신 **간판과 인테리어 일부만 바꾸는 것**. 구조(원 가중치)는 그대로 두니 공사비와 기간이 확 줄지만, 손님이 느끼는 인상은 새 가게가 된다.

**왜 중요한가 / 언제 쓰나:**
- 이 카테고리(4번)의 **상위 개념**이다. LoRA·QLoRA·Prefix·Adapter·P-Tuning·BitFit·Soft Prompts가 모두 PEFT의 하위 기법이다.
- 소비자용 GPU 한 장으로도 큰 모델을 맞춤화할 수 있게 만든 것이 이 계열의 실질적 기여다.
- 어댑터만 따로 저장·교체할 수 있어, 과제별로 모델 전체 사본을 두지 않아도 된다.

**실무 예시 / AI에게 이렇게 말한다:**
- "전체 파인튜닝은 장비가 안 되니, PEFT 계열 중에서 이 데이터 규모에 맞는 기법을 추천하고 이유를 설명해줘."

**흔한 오해:**
- **"PEFT = LoRA"** — 아니다. LoRA는 PEFT의 여러 기법 중 하나이고, 무엇을 학습하고 무엇을 얼리는지가 기법마다 다르다.
- **"파라미터를 적게 바꾸니 성능도 그만큼 떨어진다"** — 목표는 전체 파인튜닝에 **준하는** 성능이다. 다만 과제·데이터·기법에 따라 결과가 달라지므로 항상 검증셋으로 확인한다.
- **"PEFT면 지식을 새로 주입할 수 있다"** — 파인튜닝 일반의 한계가 그대로 적용된다. 최신 사실을 넣는 문제는 대개 [RAG](03-building.md)의 영역이다.

**함께 보기:** [LoRA](04-finetuning.md#lora--로라저계수-적응-low-rank-adaptation), [QLoRA](04-finetuning.md#qlora--큐로라-quantized-lora), [Adapter Tuning](04-finetuning.md#adapter-tuning--어댑터-튜닝), [SFT](04-finetuning.md#sft--지도-파인튜닝-supervised-fine-tuning)

**출처:** Hugging Face, *PEFT (Parameter-Efficient Fine-Tuning) documentation*, [huggingface.co/docs/peft](https://huggingface.co/docs/peft/index) ("PEFT methods only fine-tune a small number of (extra) model parameters — significantly decreasing computational and storage costs — while yielding performance comparable to a fully fine-tuned model"; 확인 2026-08-27). (총칭 개념 — 유일 창시 논문 없음.)

---

### SFT · 지도 파인튜닝 (Supervised Fine-Tuning)

> **한 줄 요약:** "이렇게 답하라"는 모범 답안 묶음을 보여 주며 모델을 다시 가르치는, 파인튜닝의 가장 기본 형태.

**정의 (Definition)**
- KO: 입력과 사람이 작성한 모범 출력이 짝지어진 데이터로 사전학습 모델을 지도학습 방식으로 추가 학습시키는 단계. InstructGPT에서는 사람 피드백 기반 강화학습(RLHF)에 앞서는 **1단계**로 쓰였다.
- EN: Further training a pretrained model with supervised learning on a dataset of labeler demonstrations of the desired behavior — the first stage before reward modeling and RL in the InstructGPT recipe.

**비유 (쉽게):** 신입에게 **잘 쓴 보고서 예시 묶음**을 주고 "이 형식과 톤으로 쓰라"고 시키는 것. 업무 지식을 새로 주입하는 게 아니라, **일하는 방식**을 맞추는 훈련이다.

**왜 중요한가 / 언제 쓰나:**
- 문체·형식·도메인 어휘·응답 형태를 맞출 때 가장 먼저 검토하는 방법이다.
- [Instruction Tuning](04-finetuning.md#instruction-tuning--인스트럭션-튜닝)은 지시문–응답 쌍으로 하는 SFT의 한 형태로 볼 수 있다.
- [PEFT](04-finetuning.md#peft--파라미터-효율-파인튜닝-parameter-efficient-fine-tuning) 기법과 배타적이지 않다 — LoRA로 SFT를 수행하는 구성이 실무에서 흔하다.

**실무 예시 / AI에게 이렇게 말한다:**
- "우리 의견서 30건을 입력–출력 쌍으로 만들어 SFT용 데이터셋 형식으로 정리해줘. 사건 식별 정보는 전부 빼고."

**흔한 오해:**
- **"SFT로 새 지식을 넣는다"** — 형식·문체·과제 수행 방식을 맞추는 데 강하고, 최신 사실을 주입하는 용도로는 신뢰하기 어렵다. 사실 갱신은 [RAG](03-building.md) 쪽 문제다.
- **"SFT = RLHF"** — 아니다. SFT는 RLHF 이전 단계이며, 선호 비교나 보상 모델 없이 모범 답안만으로 학습한다.
- **"데이터가 많을수록 무조건 낫다"** — 품질·일관성이 양보다 중요하며, 과하면 [과적합](04-finetuning.md#overfitting--과적합)과 [파국적 망각](04-finetuning.md#catastrophic-forgetting--파국적-망각)을 부른다.

**함께 보기:** [Instruction Tuning](04-finetuning.md#instruction-tuning--인스트럭션-튜닝), [PEFT](04-finetuning.md#peft--파라미터-효율-파인튜닝-parameter-efficient-fine-tuning), [RLHF](05-alignment-rl.md), [Overfitting](04-finetuning.md#overfitting--과적합)

**출처:** Ouyang et al. (2022), *Training language models to follow instructions with human feedback*, [arXiv:2203.02155](https://arxiv.org/abs/2203.02155) ("we collect a dataset of labeler demonstrations of the desired model behavior, which we use to fine-tune GPT-3 using supervised learning"; 확인 2026-08-27). (대표 출처이며 지도학습식 파인튜닝의 유일 창시는 아님.)

---

### LoRA · 로라·저계수 적응 (Low-Rank Adaptation)

> **한 줄 요약:** 원본 모델은 통째로 얼려 두고, 각 층에 끼운 작은 행렬만 학습해 저비용으로 모델을 맞춤화한다.

**정의 (Definition)**
- KO: 사전학습된 가중치는 동결한 채, 각 Transformer 층에 삽입한 저계수(low-rank) 분해 행렬(A·B)만 학습하는 파라미터 효율 파인튜닝(PEFT) 기법.
- EN: A PEFT method that freezes the pretrained weights and trains only small injected low-rank decomposition matrices at each layer.

**비유 (쉽게):** 두꺼운 교과서를 통째로 다시 쓰는 대신, 여백에 얇은 포스트잇 메모만 붙여 내용을 보정하는 것. 원본 책(원 가중치)은 그대로 두고, 포스트잇(저계수 행렬)만 새로 배운다.

**왜 중요한가 / 언제 쓰나:**
- 175B급 대형 모델도 학습 파라미터를 약 1만분의 1로 줄여, 적은 GPU로 파인튜닝할 수 있다.
- 학습한 저계수 행렬을 원 가중치에 **병합**할 수 있어 **추론 지연이 없다**(어댑터 방식과의 결정적 차이).
- 태스크마다 작은 LoRA만 갈아 끼우면 되므로, 하나의 베이스 모델을 여러 용도로 재사용하기 좋다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 오픈 모델을 LoRA로 우리 사내 데이터에 맞춰 파인튜닝해줘."
- "메모리가 부족하니 QLoRA로 단일 GPU에서 학습되게 해줘."

**흔한 오해:** LoRA를 prompt/prefix tuning과 혼동하는 것. LoRA는 **가중치 행렬에 델타(변화량)를 더하는** 방식이지, 입력 앞에 학습된 토큰(soft prompt)을 붙이는 방식이 아니다.

**함께 보기:** [QLoRA](04-finetuning.md#qlora--큐로라-quantized-lora), [Adapter Tuning](04-finetuning.md#adapter-tuning--어댑터-튜닝), [Soft Prompts](04-finetuning.md#soft-prompts--prompt-tuning--소프트-프롬프트), [Parameter/Weight](01-llm-basics.md)

**출처:** Hu et al. (2021), *LoRA: Low-Rank Adaptation of Large Language Models*, [arXiv:2106.09685](https://arxiv.org/abs/2106.09685).

---

### QLoRA · 큐로라 (Quantized LoRA)

> **한 줄 요약:** 원본 모델을 4비트로 압축해 얼려 두고, 그 위에 LoRA 어댑터만 학습해 단일 GPU로도 큰 모델을 파인튜닝한다.

**정의 (Definition)**
- KO: 사전학습 모델을 4비트(NF4)로 양자화해 동결하고, 그 위에서 LoRA 어댑터만 학습하는 파라미터 효율 파인튜닝 기법. **학습: LoRA 어댑터 / 동결: 4-bit 양자화된 베이스 가중치.**
- EN: A PEFT method that freezes a 4-bit (NF4) quantized pretrained model and trains only LoRA adapters on top.

**비유 (쉽게):** 두꺼운 교과서를 원본 그대로 두는 대신 부피를 줄인 **압축 복사본(4비트 양자화)**을 만들어 책장 자리를 아끼고, 그 위에 얇은 포스트잇(LoRA)만 붙여 내용을 보정한다.

**왜 중요한가 / 언제 쓰나:**
- 4비트로 압축한 덕에 메모리 사용이 크게 줄어, 큰 모델도 단일 GPU에서 파인튜닝할 수 있다.
- **무엇을 학습 vs 동결:** 압축된 베이스는 통째로 얼어 있고, 학습되는 것은 그 위에 얹힌 LoRA 어댑터뿐이다.

**실무 예시 / AI에게 이렇게 말한다:**
- "메모리가 부족하니 QLoRA로 단일 GPU에서 이 모델을 파인튜닝해줘."
- "베이스는 4비트로 얼리고 LoRA 어댑터만 학습하게 설정해줘."

**흔한 오해:** QLoRA를 완전히 새로운 알고리즘으로 여기는 것. **QLoRA = 양자화된 베이스 + LoRA**의 결합이다. 또한 베이스 모델을 4비트로 "학습"하는 것이 아니라, 4비트로 **동결**한 채 어댑터만 학습한다.

**함께 보기:** [LoRA](04-finetuning.md#lora--로라저계수-적응-low-rank-adaptation), [Adapter Tuning](04-finetuning.md#adapter-tuning--어댑터-튜닝), [Parameter/Weight](01-llm-basics.md)

**출처:** Dettmers et al. (2023), *QLoRA: Efficient Finetuning of Quantized LLMs*, [arXiv:2305.14314](https://arxiv.org/abs/2305.14314).

---

### Prefix Tuning · 프리픽스 튜닝

> **한 줄 요약:** 모델 전체는 얼려 두고, 모든 층의 어텐션 앞에 붙이는 학습된 '길잡이 벡터'만 훈련한다.

**정의 (Definition)**
- KO: 언어모델 전체를 동결하고, Transformer **전 층**의 어텐션에 붙는 연속적인 prefix 벡터만 학습하는 PEFT 기법. **학습: 전 층 prefix 벡터 / 동결: 언어모델 전부.**
- EN: A PEFT method that freezes the entire language model and trains only continuous prefix vectors prepended at the attention of **every** layer.

**비유 (쉽게):** 책의 모든 장(章) 맨 앞에 "이 장은 이렇게 읽어라"는 **길잡이 메모**를 끼워 두는 것. 본문(모델)은 한 글자도 고치지 않고, 각 장 앞의 길잡이만 새로 배운다.

**왜 중요한가 / 언제 쓰나:**
- 원본 모델을 건드리지 않고도 태스크마다 작은 prefix만 갈아 끼워 맞춤화할 수 있다.
- **무엇을 학습 vs 동결:** 언어모델 가중치는 전부 동결, 학습되는 것은 전 층에 붙는 prefix 벡터뿐이다.

**실무 예시 / AI에게 이렇게 말한다:**
- "모델은 얼리고 prefix 튜닝으로 이 생성 태스크에 맞춰줘."

**흔한 오해:** Prompt tuning과 같다고 여기는 것. **Prefix tuning은 전 층**에 벡터를 붙이지만, prompt tuning은 **입력층 한 곳**에만 붙인다.

**함께 보기:** [Soft Prompts](04-finetuning.md#soft-prompts--prompt-tuning--소프트-프롬프트), [P-Tuning](04-finetuning.md#p-tuning--피튜닝-v1v2), [LoRA](04-finetuning.md#lora--로라저계수-적응-low-rank-adaptation)

**출처:** Li & Liang (2021), *Prefix-Tuning: Optimizing Continuous Prompts for Generation*, [arXiv:2101.00190](https://arxiv.org/abs/2101.00190).

---

### Adapter Tuning · 어댑터 튜닝

> **한 줄 요약:** 원본 모델은 얼려 두고, 층과 층 사이에 끼운 작은 '변환 부품'만 학습한다.

**정의 (Definition)**
- KO: 원본 가중치를 전부 동결하고, 각 Transformer 층 **내부**(어텐션·피드포워드 서브모듈 뒤)에 삽입한 작은 병목(bottleneck) 모듈만 학습하는 PEFT 기법. **학습: 삽입된 어댑터 모듈 / 동결: 원본 가중치 전부.**
- EN: A PEFT method that freezes all original weights and trains only small bottleneck modules inserted inside each Transformer layer (after the attention and feed-forward sub-modules).

**비유 (쉽게):** 큰 기계는 그대로 두고, 기계 **각 단(段) 안에 작은 변환 부품(어댑터)**을 끼워 동작을 살짝 바꾸는 것. 본체는 손대지 않고 끼운 부품만 새로 배운다.

**왜 중요한가 / 언제 쓰나:**
- 원본을 동결한 채 소수의 모듈만 학습해, 태스크별로 적은 파라미터만 저장하면 된다.
- **무엇을 학습 vs 동결:** 원본 가중치는 전부 동결, 학습되는 것은 삽입된 어댑터 모듈뿐이다.

**실무 예시 / AI에게 이렇게 말한다:**
- "원본은 얼리고 어댑터 모듈만 학습해서 이 태스크에 붙여줘."

**흔한 오해:** LoRA처럼 추론 비용이 그대로일 거라 여기는 것. 어댑터는 모듈을 실제로 층 사이에 **삽입**하므로, LoRA(병합 가능·지연 없음)와 달리 **추론 지연이 발생**한다.

**함께 보기:** [LoRA](04-finetuning.md#lora--로라저계수-적응-low-rank-adaptation), [QLoRA](04-finetuning.md#qlora--큐로라-quantized-lora)

**출처:** Houlsby et al. (2019), *Parameter-Efficient Transfer Learning for NLP*, [arXiv:1902.00751](https://arxiv.org/abs/1902.00751).

---

### Instruction Tuning · 인스트럭션 튜닝

> **한 줄 요약:** 다양한 지시-응답 예시로 모델을 통째로 다시 가르쳐, "지시를 따르는 법" 자체를 익히게 하는 완전 미세조정이다.

**정의 (Definition)**
- KO: 여러 과제를 자연어 지시(instruction) 형태로 묶은 데이터로 모델을 미세조정해, 처음 보는 지시에도 일반화하도록 만드는 기법. **대개 전체 파라미터를 갱신하는 완전 미세조정 계열이다(=어느 파라미터를 얼리느냐의 문제가 아니라, 무엇을 학습하느냐 — 지시형 데이터 — 의 문제).**
- EN: Fine-tuning a model on many tasks phrased as natural-language instructions so it generalizes to unseen instructions. Typically **full fine-tuning** (updates all parameters).

**비유 (쉽게):** 한두 과목만 보강하는 게 아니라, **다양한 지시를 두루 시범 보여** "지시를 알아듣고 따르는 법" 자체를 몸에 익히게 하는 전체 재교육.

**왜 중요한가 / 언제 쓰나:**
- 처음 보는 형태의 지시에도 반응하는 '지시 추종' 능력을 길러, 범용 어시스턴트의 기본기를 만든다.
- **무엇을 학습 vs 동결(축이 다름):** 이 항목은 "어느 파라미터를 얼리나"의 축이 아니라 **"무엇을 학습하나(지시형 데이터)"**의 축이다. 방식으로는 대개 **전체 파라미터를 갱신**한다.

**실무 예시 / AI에게 이렇게 말한다:**
- "지시-응답 데이터셋으로 이 베이스 모델을 인스트럭션 튜닝해줘."

**흔한 오해:** Instruction tuning을 LoRA·어댑터 같은 **PEFT로 오해**하는 것. Instruction tuning은 PEFT가 아니라 **완전 미세조정(전체 파라미터 갱신)** 계열이며, "무엇을 학습하나"의 축이지 "무엇을 동결하나"의 축이 아니다.

**함께 보기:** [Multi-Task Fine-Tuning](04-finetuning.md#multi-task-fine-tuning--멀티태스크-미세조정), [LoRA](04-finetuning.md#lora--로라저계수-적응-low-rank-adaptation)

**출처:** Wei et al. (2021), *FLAN: Finetuned Language Models Are Zero-Shot Learners*, [arXiv:2109.01652](https://arxiv.org/abs/2109.01652); 보조 — Sanh et al. (2021), *T0: Multitask Prompted Training*, [arXiv:2110.08207](https://arxiv.org/abs/2110.08207).

---

### P-Tuning · 피튜닝 (v1/v2)

> **한 줄 요약:** 입력 앞에 붙는 '학습된 힌트(연속 프롬프트)'로 모델을 맞추는 기법. v1은 주로 입력층, v2는 전 층에 힌트를 넣는다.

**정의 (Definition)**
- KO: 언어모델을 (대체로) 얼려 두고, 연속적인 프롬프트 임베딩만 학습하는 기법. **v1**은 입력층 중심의 연속 프롬프트를, **v2**는 전 층에 걸친 deep prompt를 학습한다. **학습: 연속 프롬프트 임베딩 / 동결: 언어모델(대체로).**
- EN: Trains continuous prompt embeddings while (largely) freezing the LM. **v1** learns input-level continuous prompts; **v2** learns deep prompts across **all** layers.

**비유 (쉽게):** 입력 앞에 사람이 쓴 게 아니라 **학습으로 만들어진 힌트**를 끼워 넣어 모델을 유도하는 것. v1은 입구에만 힌트를 두고, **v2는 모든 층에 힌트**를 나눠 둔다.

**왜 중요한가 / 언제 쓰나:**
- 모델을 (대체로) 동결한 채 작은 프롬프트만 학습해 태스크에 맞출 수 있다.
- **무엇을 학습 vs 동결:** 언어모델은 대체로 동결(v1은 frozen/tuned 선택 가능), 학습되는 것은 연속 프롬프트 임베딩이다.

**실무 예시 / AI에게 이렇게 말한다:**
- "P-Tuning v2로 전 층에 deep prompt를 학습해서 이 분류 태스크에 맞춰줘."

**흔한 오해:** v1과 v2를 같은 것으로 뭉뚱그리는 것. 둘은 **별개 논문·별개 방법**이다 — v1은 입력층 중심, v2는 전 층 deep prompt tuning이다.

**함께 보기:** [Prefix Tuning](04-finetuning.md#prefix-tuning--프리픽스-튜닝), [Soft Prompts](04-finetuning.md#soft-prompts--prompt-tuning--소프트-프롬프트)

**출처:** Liu et al. (2021), *GPT Understands, Too* (P-Tuning), [arXiv:2103.10385](https://arxiv.org/abs/2103.10385); Liu et al. (2021), *P-Tuning v2*, [arXiv:2110.07602](https://arxiv.org/abs/2110.07602).

---

### BitFit · 비트핏

> **한 줄 요약:** 큰 모델은 그대로 얼려 두고, 이미 있는 편향(bias) 항만 미세조정한다.

**정의 (Definition)**
- KO: 모든 가중치 행렬을 동결하고, 모델에 이미 존재하는 bias 항만 학습하는 PEFT 기법. **학습: 기존 bias 항 / 동결: 모든 가중치 행렬.**
- EN: A PEFT method that freezes all weight matrices and trains only the existing bias terms.

**비유 (쉽게):** 큰 기계 본체는 손대지 않고, 이미 달려 있는 **미세조정 나사(bias)만 살짝 돌려** 출력을 맞추는 것.

**왜 중요한가 / 언제 쓰나:**
- 학습 대상이 bias뿐이라 극히 적은 파라미터만 갱신하고도 태스크에 적응할 수 있다.
- **무엇을 학습 vs 동결:** 가중치 행렬은 전부 동결, 학습되는 것은 기존 bias 항뿐이다.

**실무 예시 / AI에게 이렇게 말한다:**
- "가중치는 다 얼리고 BitFit으로 bias만 학습해줘."

**흔한 오해:** 새 파라미터를 추가한다고 여기는 것. BitFit은 새 파라미터를 **추가하지 않고**, 모델에 이미 존재하는 **bias 항만** 갱신한다.

**이름에 관하여:** 원 논문은 **이름을 풀어 쓰지 않는다**(확인 2026-08-27). 흔히 "Bias-terms Fine-tuning"으로 읽히지만 논문에 그 표기가 없으므로 정본으로 인용하지 않는다. 이름이 가리키는 바(bias 항만 학습)는 정의로 충분히 드러난다.

**함께 보기:** [LoRA](04-finetuning.md#lora--로라저계수-적응-low-rank-adaptation), [Adapter Tuning](04-finetuning.md#adapter-tuning--어댑터-튜닝), [Parameter/Weight](01-llm-basics.md)

**출처:** Ben-Zaken et al. (2021), *BitFit: Simple Parameter-efficient Fine-tuning*, [arXiv:2106.10199](https://arxiv.org/abs/2106.10199).

---

### Soft Prompts · Prompt Tuning · 소프트 프롬프트

> **한 줄 요약:** 사람이 읽을 수 없는, 학습된 벡터를 입력 앞에 붙여 모델을 유도한다. 모델이 얼려 있어도 태스크에 맞출 수 있다.

**정의 (Definition)**
- KO: 언어모델 전체를 동결하고, 입력층에 붙는 soft prompt(연속 벡터)만 학습하는 PEFT 기법. **학습: 입력층 soft prompt / 동결: 언어모델 전부.**
- EN: A PEFT method that freezes the entire LM and trains only a soft prompt (continuous vectors) prepended at the input layer.

**비유 (쉽게):** 입력 앞에 사람이 읽을 수 있는 문장이 아니라, **사람이 못 읽는 학습된 암호 같은 프롬프트(벡터)**를 붙여 모델을 원하는 방향으로 유도하는 것.

**왜 중요한가 / 언제 쓰나:**
- 모델을 통째로 얼린 채 작은 벡터만 학습해 태스크별로 저렴하게 맞출 수 있다.
- **무엇을 학습 vs 동결:** 언어모델은 전부 동결, 학습되는 것은 입력층 soft prompt뿐이다.
- 모델 규모가 커질수록 완전 미세조정과의 성능 격차가 사라지는 경향이 있다.

**실무 예시 / AI에게 이렇게 말한다:**
- "모델은 얼리고 prompt tuning으로 soft prompt만 학습해서 이 태스크에 맞춰줘."

**흔한 오해:** Soft prompt를 우리가 입력하는 **텍스트 프롬프트**로 여기는 것. Soft prompt는 **사람이 읽을 수 없는 학습된 벡터**다. 또한 모델이 클수록 완전 미세조정과의 격차가 소멸하는 경향이 있다.

**함께 보기:** [Prefix Tuning](04-finetuning.md#prefix-tuning--프리픽스-튜닝), [P-Tuning](04-finetuning.md#p-tuning--피튜닝-v1v2), [LoRA](04-finetuning.md#lora--로라저계수-적응-low-rank-adaptation)

**출처:** Lester et al. (2021), *The Power of Scale for Parameter-Efficient Prompt Tuning*, [arXiv:2104.08691](https://arxiv.org/abs/2104.08691).

---

### Multi-Task Fine-Tuning · 멀티태스크 미세조정

> **한 줄 요약:** 여러 과제를 한꺼번에 학습시켜 태스크끼리 서로 도움받게 하는 미세조정 방식.

**정의 (Definition)**
- KO: 여러 태스크를 동시에 학습시켜 공유 표현으로 서로의 성능을 끌어올리는 학습 방식. 대개 공유 백본에 태스크별 head를 얹어 함께 갱신하며, **방법 자체가 무엇을 동결할지 규정하지는 않는다(=동결 축이 아니라 "무엇을 학습하나"의 축).**
- EN: Training on several tasks jointly so shared representations help each other. Usually updates a shared backbone plus per-task heads; the method does not by itself dictate what to freeze.

**비유 (쉽게):** 한 과목만 파는 대신 **여러 과목을 한꺼번에 공부**시켜, 과목끼리 서로 도움받으며 전체 실력이 오르게 하는 것.

**왜 중요한가 / 언제 쓰나:**
- 태스크 간 공유 지식을 활용해 개별 태스크 성능과 일반화를 높인다.
- **무엇을 학습 vs 동결:** "무엇을/어디서 학습하나"의 축이며(여러 태스크 공동 학습), 어느 파라미터를 얼리느냐는 방법이 따로 규정하지 않는다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 여러 태스크를 묶어서 멀티태스크로 함께 학습시켜줘."

**흔한 오해:** 단일 창시 논문이 있다고 보는 것. **멀티태스크 학습은 단일 창시 논문이 없어** 개괄(서베이)을 대표 출처로 채택한다(유일 창시가 아님). 참고로 **instruction tuning은 멀티태스크 미세조정의 특수 사례**로 볼 수 있다.

**함께 보기:** [Instruction Tuning](04-finetuning.md#instruction-tuning--인스트럭션-튜닝), [Federated Fine-Tuning](04-finetuning.md#federated-fine-tuning--연합-미세조정)

**출처:** Ruder (2017), *An Overview of Multi-Task Learning in Deep Neural Networks* (개괄·유일 창시 아님), [arXiv:1706.05098](https://arxiv.org/abs/1706.05098).

---

### Federated Fine-Tuning · 연합 미세조정

> **한 줄 요약:** 원본 데이터는 각자 기기에 남겨 둔 채, 로컬에서 학습한 업데이트(요약노트)만 모아 하나의 모델로 합친다.

**정의 (Definition)**
- KO: 데이터를 한곳에 모으지 않고 각 참여자 기기에 남긴 채, 로컬에서 계산한 모델 업데이트만 취합(model averaging)해 공동 모델을 학습하는 방식. **원본 데이터는 로컬 잔류, 취합되는 것은 로컬 업데이트뿐이다.**
- EN: Training a shared model by keeping raw data local on each participant and aggregating only locally computed model updates (model averaging).

**비유 (쉽게):** 각자 **자기 집에서 공부**하고, 원본 교재(데이터)는 밖으로 내보내지 않은 채 **요약노트(모델 업데이트)만 모아** 하나로 합치는 스터디 모임.

**왜 중요한가 / 언제 쓰나:**
- 민감한 원본 데이터를 밖으로 내보내지 않고도 여러 곳의 데이터로 모델을 함께 개선할 수 있다.
- **무엇을 학습 vs 동결:** 프라이버시를 위한 **분산 학습 구조**의 축이며, PEFT의 동결 축과는 **직교**한다(흔히 PEFT와 결합해 씀).

**실무 예시 / AI에게 이렇게 말한다:**
- "데이터는 각 기기에 두고 연합 미세조정으로 업데이트만 모아 학습하게 해줘."

**흔한 오해:** 연합 미세조정 자체가 PEFT라고 여기는 것. 연합 학습(FL)과 PEFT는 **직교하는 개념**이며(자주 결합되지만 별개), 연합 미세조정 그 자체가 PEFT는 아니다.

**함께 보기:** [Multi-Task Fine-Tuning](04-finetuning.md#multi-task-fine-tuning--멀티태스크-미세조정), [LoRA](04-finetuning.md#lora--로라저계수-적응-low-rank-adaptation)

**출처:** McMahan et al. (*FedAvg*, 개념 창시 — arXiv 2016 / AISTATS 2017), [arXiv:1602.05629](https://arxiv.org/abs/1602.05629); LLM 적용 — Zhang et al. (2023), *FedIT*, [arXiv:2305.05644](https://arxiv.org/abs/2305.05644).

---

### Overfitting · 과적합

> **한 줄 요약:** 모델이 학습 데이터를 **외우다시피** 지나치게 맞춰진 나머지, 정작 처음 보는 새 데이터에서는 성능이 떨어지는 현상.

**정의 (Definition)**
- KO: 모델이 학습 데이터에 너무 가깝게(잡음까지) 맞춰진 나머지, 일반화에 실패해 새로운 데이터에서는 올바른 예측을 하지 못하는 현상.
- EN: Creating a model that matches (memorizes) the training set so closely that it fails to make correct predictions on new data.

**비유 (쉽게):** 시험 대비를 **기출문제의 답만 통째로 외운** 학생과 같다. 봤던 문제(학습 데이터)는 만점이지만, 숫자만 바꾼 새 문제(실제 데이터)가 나오면 무너진다. 원리를 배운 게 아니라 답을 외운 탓이다.

**왜 중요한가 / 언제 쓰나:**
- 파인튜닝·평가의 핵심 함정 — 데이터가 적거나 너무 오래 학습시키면, 모델이 일반적 패턴 대신 학습셋의 우연·잡음까지 외운다.
- 목표는 학습셋 점수가 아니라 **일반화(generalization)** — 처음 보는 데이터에서의 성능이다.
- 벤치마크(eval)에도 적용된다 — 특정 벤치마크에 과적합된 모델은 그 점수만 높고 실제 성능은 부풀려질 수 있다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 파인튜닝이 과적합됐는지 보게, 학습셋 말고 따로 떼어 둔 검증셋 성능도 함께 재줘."

**흔한 오해:**
- **"학습셋 점수가 높으면 좋은 모델"** — 아니다. 학습셋만 높고 검증·실데이터에서 낮으면 과적합이다. 좋고 나쁨은 **처음 보는 데이터** 성능으로 가린다.
- **"과적합은 무조건 학습을 오래 해서 생긴다"** — 그것도 한 원인이지만, 데이터가 너무 적거나 모델이 과제에 비해 지나치게 복잡할 때도 생긴다.

**함께 보기:** [Evals](07-dev-stages.md), [Instruction Tuning](04-finetuning.md#instruction-tuning--인스트럭션-튜닝), [Multi-Task Fine-Tuning](04-finetuning.md#multi-task-fine-tuning--멀티태스크-미세조정)

**출처:** Google, *Machine Learning Crash Course — Overfitting*, [developers.google.com](https://developers.google.com/machine-learning/crash-course/overfitting/overfitting) ("creating a model that matches (memorizes) the training set so closely that the model fails to make correct predictions on new data"; 확인 2026-07-12). 보조 — *Overfitting*, [en.wikipedia.org/wiki/Overfitting](https://en.wikipedia.org/wiki/Overfitting) ("corresponds too closely … may therefore fail to fit to additional data or predict future observations reliably"). (표준 ML 개념 — 유일 창시 없음.)

---

### Catastrophic forgetting · 파국적 망각

> **한 줄 요약:** 새 과제를 가르치면 이전에 잘하던 것을 급격히 잊어버리는 신경망의 고질적 성질.

**정의 (Definition)**
- KO: 신경망을 새로운 과제로 순차 학습시킬 때, 이전 과제에 필요한 가중치가 덮어써져 기존 성능이 급격히 떨어지는 현상.
- EN: The tendency of neural networks to abruptly lose performance on previously learned tasks when trained sequentially on a new task.

**비유 (쉽게):** 한 칠판에 계속 새 내용을 덧쓰는 것과 같다. 새 수업(새 과제)을 적으려면 지우고 써야 하는데, **지운 자리에 있던 지난 수업 내용이 함께 사라진다.**

**왜 중요한가 / 언제 쓰나:**
- 파인튜닝의 대표적 부작용이다. 좁은 도메인 데이터로 강하게 학습시키면 그 과제는 좋아지지만 **일반 능력이 무너질 수 있다.**
- [PEFT](04-finetuning.md#peft--파라미터-효율-파인튜닝-parameter-efficient-fine-tuning) 계열이 선호되는 이유 중 하나 — 원 가중치를 얼려 두면 덮어쓸 여지가 줄어든다.
- 파인튜닝 후에는 목표 과제뿐 아니라 **손대지 않은 일반 과제 성능도 함께** 재야 이 현상을 잡아낸다.

**실무 예시 / AI에게 이렇게 말한다:**
- "파인튜닝 전후로 목표 과제와 일반 상식 과제를 둘 다 평가해서, 파국적 망각이 일어났는지 비교해줘."

**흔한 오해:**
- **"학습을 더 시키면 지식이 쌓이기만 한다"** — 순차 학습에서는 쌓이는 게 아니라 **덮어써질** 수 있다.
- **"과적합과 같은 말"** — 다르다. 과적합은 *새 데이터*에서의 일반화 실패이고, 파국적 망각은 *이전에 하던 과제*의 성능 상실이다.
- **"완전히 해결된 문제"** — 완화 기법(정규화·리허설·어댑터 분리 등)이 있을 뿐 일반 해법은 아니다.

**함께 보기:** [Overfitting](04-finetuning.md#overfitting--과적합), [PEFT](04-finetuning.md#peft--파라미터-효율-파인튜닝-parameter-efficient-fine-tuning), [Multi-Task Fine-Tuning](04-finetuning.md#multi-task-fine-tuning--멀티태스크-미세조정)

**출처:** Kirkpatrick et al. (2016), *Overcoming catastrophic forgetting in neural networks*, [arXiv:1612.00796](https://arxiv.org/abs/1612.00796) ("it has been widely thought that catastrophic forgetting is an inevitable feature of connectionist models"; 확인 2026-08-27). (완화 기법을 제시한 대표 논문 — 현상 자체는 1980년대부터 보고된 것으로 유일 창시 아님.)
