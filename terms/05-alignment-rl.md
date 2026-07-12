# 5. 정렬 · 강화학습 · Alignment & Reinforcement Learning

> 모델을 사람이 원하는 방향으로 "정렬"시키는 방법들. 선호를 어떻게 배우느냐가 갈림길.
> 출처 근거: [research/SOURCES.md](../research/SOURCES.md) 카테고리 5. 서식: [STYLE.md](../STYLE.md).

---

### RLHF · 인간 피드백 기반 강화학습 (Reinforcement Learning from Human Feedback)

> **한 줄 요약:** 사람이 여러 답에 순위를 매겨 "코치(보상모델)"를 학습시키고, 그 코치를 심판 삼아 모델을 훈련하는 방식.

**정의 (Definition)**
- KO: 인간의 선호(어느 답이 더 나은지의 순위)로 보상모델을 학습시킨 뒤, 그 보상을 신호로 강화학습(PPO)을 돌려 모델을 미세조정하는 방법.
- EN: Training a reward model from human preference rankings, then fine-tuning the policy with reinforcement learning (PPO) against that reward signal.

**비유 (쉽게):** 직접 정답지를 나눠주는 게 아니라, 심사위원이 여러 답안을 놓고 **"이게 저것보다 낫다"**를 반복해 채점 기준(코치)을 먼저 만든다. 그다음 그 코치가 모델의 답마다 점수를 매기고, 모델은 점수가 높아지는 쪽으로 스스로 고쳐간다.

**왜 중요한가 / 언제 쓰나:**
- "도움되고 안전한" 답처럼 **정답을 명시적으로 적기 어려운 목표**를 사람의 비교 판단만으로 학습시킬 수 있다.
- ChatGPT·InstructGPT 계열의 지시 따르기(instruction following)를 만든 표준 정렬 기법이다.

**실무 예시 / AI에게 이렇게 말한다:**
- "두 답변 중 어느 쪽이 더 도움되는지 사람이 고른 데이터로 보상모델을 학습시켜 RLHF를 돌려줘."

**흔한 오해:** 사람이 **정답을 직접 써 주는 게 아니다.** 사람은 두 답 중 어느 쪽이 나은지 **선호 비교(순위)**만 하고, 그 비교로 보상모델을 학습시킨다.

**함께 보기:** [RLAIF](05-alignment-rl.md#rlaif--ai-피드백-기반-강화학습-reinforcement-learning-from-ai-feedback), [DPO](05-alignment-rl.md#dpo--직접-선호-최적화-direct-preference-optimization), [GRPO](05-alignment-rl.md#grpo--그룹-상대-정책-최적화-group-relative-policy-optimization), [Instruction Tuning](04-finetuning.md)

**출처:** Christiano et al. (2017), *Deep Reinforcement Learning from Human Preferences*, [arXiv:1706.03741](https://arxiv.org/abs/1706.03741) (기원). LLM 표준화: Ouyang et al. (2022), *Training Language Models to Follow Instructions with Human Feedback* (통칭 InstructGPT), [arXiv:2203.02155](https://arxiv.org/abs/2203.02155).

---

### RLAIF · AI 피드백 기반 강화학습 (Reinforcement Learning from AI Feedback)

> **한 줄 요약:** 답에 순위를 매기는 "코치" 역할을 사람 대신 다른 AI가 맡는 RLHF.

**정의 (Definition)**
- KO: 선호 라벨(어느 답이 나은지)을 붙이는 주체를 인간에서 다른 LLM으로 교체해, AI가 만든 선호로 보상신호를 학습시키는 방법.
- EN: Replacing the human preference-labeler with another LLM, so preference labels for training come from AI rather than people.

**비유 (쉽게):** RLHF의 심사위원석에 사람 대신 **또 다른 AI**를 앉힌다. 다만 그 AI가 무엇을 기준으로 채점할지 정하는 **원칙(헌법)은 사람이 먼저 써 준다.**

**왜 중요한가 / 언제 쓰나:**
- 사람이 일일이 비교 라벨을 다는 병목·비용을 줄여, 선호 데이터를 훨씬 대량으로 만들 수 있다.
- Anthropic의 Constitutional AI가 대표 사례로, 사람이 정한 "헌법(원칙)"에 따라 AI가 스스로 답을 비판·수정·평가한다.

**실무 예시 / AI에게 이렇게 말한다:**
- "사람 라벨 대신, 정한 원칙에 따라 다른 모델이 선호를 매기게 해서 RLAIF로 정렬해줘."

**흔한 오해:** **인간을 완전히 배제하는 게 아니다.** 채점 기준이 되는 원칙·헌법은 인간이 설계하고, AI는 그 원칙을 적용해 선호를 매길 뿐이다.

**함께 보기:** [RLHF](05-alignment-rl.md#rlhf--인간-피드백-기반-강화학습-reinforcement-learning-from-human-feedback), [DPO](05-alignment-rl.md#dpo--직접-선호-최적화-direct-preference-optimization)

**출처:** Bai et al. (2022), *Constitutional AI: Harmlessness from AI Feedback*, [arXiv:2212.08073](https://arxiv.org/abs/2212.08073) (개념). 명명·직접 비교: Lee et al. (2023), *RLAIF vs. RLHF: Scaling Reinforcement Learning from Human Feedback with AI Feedback*, [arXiv:2309.00267](https://arxiv.org/abs/2309.00267).

---

### DPO · 직접 선호 최적화 (Direct Preference Optimization)

> **한 줄 요약:** 코치(보상모델)도 강화학습 루프도 없이, "이게 더 낫다"는 선호쌍만으로 모델을 곧바로 교정하는 방식.

**정의 (Definition)**
- KO: 별도의 보상모델과 RL 루프 없이, 선호 데이터(선택된 답 vs 거부된 답)를 단순 분류 손실로 직접 최적화해 최적 정책을 유도하는 방법.
- EN: Deriving the optimal policy directly from preference data via a simple classification loss, without training a separate reward model or running an RL loop.

**비유 (쉽게):** RLHF가 "코치를 먼저 키우고 → 그 코치로 훈련"하는 2단계라면, DPO는 코치를 건너뛴다. **"이 답이 저 답보다 낫다"는 짝(pair)** 자체를 바로 학습 신호로 써서, 선호되는 답의 확률은 올리고 거부된 답의 확률은 내린다.

**왜 중요한가 / 언제 쓰나:**
- 보상모델 학습·PPO 튜닝 같은 복잡하고 불안정한 단계를 없애, 구현이 단순하고 안정적이다.
- 같은 선호 데이터로 RLHF에 준하는 정렬 효과를 더 가볍게 얻으려 할 때 쓴다.

**실무 예시 / AI에게 이렇게 말한다:**
- "보상모델 없이 선택/거부 선호쌍 데이터로 DPO로 바로 미세조정해줘."

**흔한 오해:** ⚠️ **DPO는 강화학습(RL)이 아니다.** 보상모델도, 정책이 환경과 상호작용하며 답을 생성하는 롤아웃도 없다. 이름과 달리 본질은 선호쌍에 대한 **분류 손실 최적화**다.

**함께 보기:** [RLHF](05-alignment-rl.md#rlhf--인간-피드백-기반-강화학습-reinforcement-learning-from-human-feedback), [GRPO](05-alignment-rl.md#grpo--그룹-상대-정책-최적화-group-relative-policy-optimization)

**출처:** Rafailov et al. (2023), *Direct Preference Optimization: Your Language Model is Secretly a Reward Model*, [arXiv:2305.18290](https://arxiv.org/abs/2305.18290).

---

### GRPO · 그룹 상대 정책 최적화 (Group Relative Policy Optimization)

> **한 줄 요약:** 한 문제에 여러 답을 내게 하고, 별도 심판(critic) 없이 그 조(組) 안에서 상대적으로 나은 답 쪽으로 학습하는 강화학습.

**정의 (Definition)**
- KO: PPO의 변형으로, 별도의 가치함수(critic) 없이 한 질문에 대해 생성한 여러 출력의 그룹 내 상대적 우열로 어드밴티지(얼마나 좋은 답인지)를 추정하는 강화학습 기법.
- EN: A PPO variant that estimates the advantage by normalizing the rewards of multiple outputs sampled for the same prompt (group-relative), removing the need for a separate value-function (critic).

**비유 (쉽게):** 한 문제에 답을 **여러 개** 뽑게 한다. 그다음 "이 답이 절대적으로 몇 점"이라고 매겨줄 심판을 따로 두지 않고, **그 조(組) 안에서 서로 비교**해 평균보다 나은 답은 밀어주고 못한 답은 눌러준다. 반 평균을 기준선으로 삼아 상대평가하는 셈이다.

**왜 중요한가 / 언제 쓰나:**
- PPO에 필요한 무거운 critic 모델을 없애 메모리·연산을 줄이면서 강화학습을 돌릴 수 있다.
- 수학·추론처럼 정답 채점이 가능한 과제의 정책 최적화에 쓰인다(원 논문은 수학 추론 맥락).

**실무 예시 / AI에게 이렇게 말한다:**
- "critic 없이 한 문제에 여러 답을 샘플링해 그룹 상대 어드밴티지로 GRPO 학습을 돌려줘."

**흔한 오해:** ⚠️ **원출처는 DeepSeek-R1이 아니다.** GRPO는 2024년 **DeepSeekMath** 논문에서 제안됐고, 널리 알려진 DeepSeek-R1은 이를 나중에 적용한 **후속 사례**다.

**함께 보기:** [RLHF](05-alignment-rl.md#rlhf--인간-피드백-기반-강화학습-reinforcement-learning-from-human-feedback), [DPO](05-alignment-rl.md#dpo--직접-선호-최적화-direct-preference-optimization)

**출처:** Shao et al. (2024), *DeepSeekMath: Pushing the Limits of Mathematical Reasoning in Open Language Models*, [arXiv:2402.03300](https://arxiv.org/abs/2402.03300).

---

### Alignment · 정렬 (AI Alignment)

> **한 줄 요약:** AI가 **똑똑하기만** 한 게 아니라 **우리가 원하는 대로, 안전하게** 행동하도록 사람의 의도·가치에 맞춰 두는 것. 이 장의 RLHF·RLAIF·DPO·GRPO가 그 구체적 방법들이다.

**정의 (Definition)**
- KO: AI 시스템이 인간(개인·집단)이 의도한 목표·선호·윤리 원칙에 부합하게 행동하도록 유도하는 것.
- EN: Steering AI systems toward a person's or group's intended goals, preferences, or ethical principles.

**비유 (쉽게):** 유능한 신입에게 일을 맡길 때, **능력(무엇을 할 수 있나)**과 **정렬(시킨 대로, 선을 지켜 하나)**은 별개의 문제다. 아무리 일 잘하는 사람도 조직이 원하는 방향과 어긋나게 열심히 하면 곤란하다. 정렬은 바로 그 "우리가 원하는 방향"에 AI를 맞춰 두는 일이다.

**왜 중요한가 / 언제 쓰나:**
- "유능함"과 "안전하게 우리 뜻대로"는 **다른 축**이다 — 성능이 아무리 높아도 의도와 어긋나면 위험하다.
- 이 장(章)의 RLHF·RLAIF·DPO·GRPO는 모두 "무엇에(사람의 선호) 정렬시키느냐"를 놓고 겨루는 **정렬의 구체적 기법**이다.
- 목표를 대충 정하면(proxy goal) AI가 그 허점을 파고드는 reward hacking이 생겨, "무엇에 정렬시킬지"를 정확히 정하는 것 자체가 어려운 문제다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 모델을 우리 안전 가이드라인에 정렬(align)시키려면 RLHF와 DPO 중 뭐가 맞을지 정리해줘."

**흔한 오해:**
- **"정렬 = 성능 향상"** — 아니다. 정렬은 *유능함*이 아니라 *의도·가치와의 부합*을 다룬다. 둘은 별개 축이다.
- **단일 표준 정의 없음** — 아래는 대표적 서술이며, "정렬"은 넓은 개념이라 단일 정본 정의가 없다(연구자·기관마다 강조점이 다르다).

**함께 보기:** [RLHF](05-alignment-rl.md#rlhf--인간-피드백-기반-강화학습-reinforcement-learning-from-human-feedback), [RLAIF](05-alignment-rl.md#rlaif--ai-피드백-기반-강화학습-reinforcement-learning-from-ai-feedback), [DPO](05-alignment-rl.md#dpo--직접-선호-최적화-direct-preference-optimization), [Guardrails](12-safety-governance.md)

**출처:** 대표 서술 — *AI alignment*, [en.wikipedia.org/wiki/AI_alignment](https://en.wikipedia.org/wiki/AI_alignment) ("steer AI systems toward a person's or group's intended goals, preferences, or ethical principles"; 확인 2026-07-12). 기관 예시 — Anthropic은 정렬 목표를 "helpful, honest, and harmless(도움되고·정직하고·해롭지 않게)"로 서술한다(*Alignment faking in large language models*, [anthropic.com](https://www.anthropic.com/research/alignment-faking)). (넓은 개념 — 단일 정본 정의 없음.)

---

## 사례 (Case Study)

### 사례: OpenPipe ART / RULER — 수작업 보상 설계를 건너뛰는 에이전트 RL

**무엇인가:** ART(**A**gent **R**einforcement **T**rainer)는 LLM 에이전트를 실제 다단계(multi-step) 과제에서 강화학습으로 훈련시키는 오픈소스 프레임워크다. 핵심 RL 알고리즘으로 앞의 [GRPO](05-alignment-rl.md#grpo--그룹-상대-정책-최적화-group-relative-policy-optimization)를 사용한다. 라이선스는 Apache-2.0(100% 오픈소스).

**RULER — 자동 LLM 채점 보상:** 강화학습의 가장 큰 수고 중 하나가 "무엇을 잘한 것으로 볼지" 보상 함수를 사람이 손으로 짜는 일이다. RULER(**R**elative **U**niversal **L**LM-**E**licited **R**ewards)는 이를 LLM 심판(LLM-as-judge)에게 맡긴다. 한 과제에 대해 에이전트가 만든 **여러 궤적(trajectory)을 나란히 놓고 상대 순위**를 매겨 각 궤적에 0~1 점수를 주고, 그 점수를 그대로 GRPO의 보상 신호로 쓴다. 라벨 데이터·수작업 보상 함수·인간 피드백 없이 모델을 개선한다는 것이 핵심 주장이다("여러 해를 나란히 비교·순위화하는 편이 하나씩 절대 채점하는 것보다 쉽다").

**왜 흥미로운가:** [RLHF](05-alignment-rl.md#rlhf--인간-피드백-기반-강화학습-reinforcement-learning-from-human-feedback)가 인간 선호 라벨을, [RLAIF](05-alignment-rl.md#rlaif--ai-피드백-기반-강화학습-reinforcement-learning-from-ai-feedback)가 원칙 기반 AI 라벨을 쓴다면, RULER는 **보상 설계 자체를 LLM 심판에게 위임**해 정렬 파이프라인의 병목(보상 함수 작성)을 줄이려는 접근이다. 원 저자는 4개 과제 중 3개에서 수작업 보상 함수 방식을 넘어섰다고 보고한다.

**출처:** OpenPipe, *ART (Agent Reinforcement Trainer)*, [github.com/openpipe/art](https://github.com/openpipe/art); *RULER*, [openpipe.ai/blog/ruler](https://openpipe.ai/blog/ruler).
