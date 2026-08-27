# 8. 모델 포맷·경량화 · Model Formats & Compression

> 모델을 **어떤 파일로 저장하고, 어떻게 작게·빠르게** 만드는가. 공통 질문은 **"무엇을 줄이고(양자화·증류) 무엇에 담는가(포맷)"**.
> 출처 근거: [research/SOURCES.md](../research/SOURCES.md) 카테고리 8. 서식: [STYLE.md](../STYLE.md).

---

### Quantization · 양자화

> **한 줄 요약:** 모델 가중치를 표현하는 숫자의 비트 수를 줄여(예: 16→4비트) 크기와 메모리를 낮추는 압축 기법.

**정의 (Definition)**
- KO: 모델 가중치(및 때로 활성값)를 더 적은 비트의 정수·저정밀도 형식으로 표현해 저장·연산 비용을 줄이는 기법. 예를 들어 16비트 부동소수를 8·4비트 정수로 바꾼다.
- EN: A technique that represents a model's weights (and sometimes activations) in fewer-bit, lower-precision formats (e.g., 16-bit floats → 8- or 4-bit integers) to cut storage and compute cost.

**비유 (쉽게):** 고해상도 사진을 용량 큰 원본 대신 **적당히 압축한 JPG**로 저장하는 것. 파일은 훨씬 작아지고 눈으로 보기엔 거의 같지만, 아주 미세한 디테일은 조금 뭉개진다(정밀도 손실).

**왜 중요한가 / 언제 쓰나:**
- 같은 모델을 절반 이하의 메모리로 올릴 수 있어, 소비자용 GPU나 노트북에서도 큰 모델을 돌릴 수 있다. LLM.int8()은 175B급 모델을 8비트로 올려도 성능을 유지함을 보였다.
- 학습이 아니라 **추론(inference) 배포**를 값싸게 하려는 목적이 크다. QLoRA처럼 양자화를 **학습**과 결합하는 방식도 있다.
- GPTQ는 학습 후(post-training) 3~4비트까지 압축하면서 정확도를 지키는 대표 기법이다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 모델을 4비트로 양자화해서 내 로컬 GPU에 올라가게 해줘."
- "GPTQ로 3~4비트 양자화하되 정확도 저하는 최소로 해줘."

**흔한 오해:** 양자화가 곧 파인튜닝이라고 여기는 것. 양자화는 **표현 비트 수를 줄이는 압축**이지 학습이 아니다(단, QLoRA처럼 양자화된 베이스 위에서 학습하는 결합 기법은 별개다). 또한 "무조건 성능이 크게 떨어진다"는 오해 — 기법에 따라 성능을 거의 유지한다.

**함께 보기:** [QLoRA](04-finetuning.md#qlora--큐로라-quantized-lora), [GGUF](08-model-formats.md#gguf), [Parameter/Weight](01-llm-basics.md)

**출처:** Dettmers et al. (2022), *LLM.int8(): 8-bit Matrix Multiplication for Transformers at Scale*, [arXiv:2208.07339](https://arxiv.org/abs/2208.07339); Frantar et al. (2022), *GPTQ: Accurate Post-Training Quantization for Generative Pre-trained Transformers*, [arXiv:2210.17323](https://arxiv.org/abs/2210.17323). (양자화는 단일 창시가 아니며, 위는 LLM 저비트 양자화의 대표 논문이다.)

---

### GGUF

> **한 줄 요약:** llama.cpp 생태계에서 (주로 양자화된) 모델을 담아 배포·로딩하는 단일 파일 포맷.

**정의 (Definition)**
- KO: GGML 기반 추론 엔진(대표적으로 llama.cpp)에서 모델의 가중치와 메타데이터를 한 파일에 담아 추론에 쓰도록 만든 파일 포맷. 이전 포맷(GGML·GGMF·GGJT)의 후속으로, 흔히 저비트 양자화된 모델을 배포하는 데 쓰인다.
- EN: A file format for storing a model's weights and metadata in a single file for inference with GGML-based executors (notably llama.cpp). It succeeds the earlier GGML/GGMF/GGJT formats and is commonly used to distribute low-bit quantized models.

**비유 (쉽게):** 요리에 필요한 재료(가중치)와 조리법 메모(메타데이터)를 **하나의 도시락 통에 담아** 그대로 열어 바로 쓰게 만든 것. 통 규격이 정해져 있어 어느 주방(호환 엔진)에서도 열린다.

**왜 중요한가 / 언제 쓰나:**
- 로컬에서 오픈 모델을 돌릴 때(예: llama.cpp·Ollama·LM Studio) 사실상 표준 배포 파일이다.
- 양자화 정보·토크나이저 설정 등을 한 파일에 함께 담아, 여러 부속 파일 없이 모델 하나만 받으면 되게 한다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 모델의 4비트 GGUF 파일을 받아 llama.cpp로 돌려줘."

**흔한 오해:** GGUF 자체가 양자화 "방법"이라고 여기는 것. GGUF는 양자화 **알고리즘이 아니라 저장 포맷**이다 — 양자화된(또는 되지 않은) 모델을 담는 그릇이며, 어떤 비트로 줄일지는 별개 문제다.

**이름에 관하여:** 공식 명세는 **약어를 풀어 쓰지 않는다**(확인 2026-08-27). 널리 도는 "GPT-Generated Unified Format"은 비공식 통용어이므로 정본 표기로 쓰지 않는다. 확실한 것은 GGML 계열 실행기용 포맷이라는 점뿐이다.

**함께 보기:** [Quantization](08-model-formats.md#quantization--양자화), [safetensors](08-model-formats.md#safetensors), [llama.cpp](09-local-run.md)

**출처:** (원전 논문 없음 — 공식 스펙 문서) ggml-org, *GGUF 파일 포맷 명세*, [github.com/ggml-org/ggml/blob/master/docs/gguf.md](https://github.com/ggml-org/ggml/blob/master/docs/gguf.md); 상위 프로젝트 [github.com/ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp).

---

### MLX

> **한 줄 요약:** 애플 실리콘(M1·M2 등)에 최적화된, 로컬에서 학습·추론을 돌리는 머신러닝 프레임워크.

**정의 (Definition)**
- KO: 애플 머신러닝 연구팀이 만든, 애플 실리콘용 NumPy 유사 배열·머신러닝 프레임워크. CPU·GPU가 메모리를 공유하는 **통합 메모리(unified memory)**를 활용해 데이터 복사 없이 연산하고, 지연 계산·함수 변환(자동미분 등)을 지원한다.
- EN: A NumPy-like array and machine learning framework for Apple silicon, from Apple machine learning research. It exploits unified memory (CPU and GPU share memory, so no data transfer), with lazy computation and composable function transformations (e.g., autodiff).

**비유 (쉽게):** 애플 칩이라는 **한 작업대 위에서** CPU와 GPU가 같은 재료(메모리)를 공유하며 일하는 공방. 재료를 이 손 저 손으로 옮겨 담는 수고(데이터 복사)가 없어 매끄럽다.

**왜 중요한가 / 언제 쓰나:**
- 별도 GPU 서버 없이 **맥에서 직접** 모델을 추론·미세학습하려는 개발자·연구자에게 유용하다.
- Python(NumPy 스타일) 외에 C++·C·Swift API도 제공해 애플 생태계 앱에 붙이기 쉽다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 모델을 MLX로 내 맥(M-시리즈)에서 로컬 추론되게 짜줘."

**흔한 오해:** MLX가 애플 전용 "모델"이나 양자화 포맷이라고 여기는 것. MLX는 **프레임워크(연산 라이브러리)**이지 특정 모델도, 파일 포맷도 아니다. 또 애플 실리콘에 최적화되어 있어 범용 엔비디아 GPU 워크플로의 대체가 목적은 아니다.

**이름에 관하여:** 애플 공식 문서·저장소는 **MLX를 약어로 풀어 쓰지 않는다**(확인 2026-08-27). "애플 실리콘용 NumPy 유사 배열·머신러닝 프레임워크"라는 설명만 있으므로, 임의의 확장형을 만들어 쓰지 않는다.

**함께 보기:** [Quantization](08-model-formats.md#quantization--양자화), [safetensors](08-model-formats.md#safetensors)

**출처:** (원전 논문 없음 — 공식 저장소·문서) ml-explore(Apple), *MLX*, [github.com/ml-explore/mlx](https://github.com/ml-explore/mlx); 공식 문서 [ml-explore.github.io/mlx](https://ml-explore.github.io/mlx/build/html/index.html).

---

### safetensors

> **한 줄 요약:** 텐서(모델 가중치)를 **안전하고 빠르게** 저장하는 파일 포맷. 코드 실행 위험이 있는 pickle을 대체한다.

**정의 (Definition)**
- KO: 모델의 텐서를 안전(코드 실행 불가)하고 빠르게(제로카피에 가깝게) 저장하기 위한 단순 직렬화 포맷. 헤더 크기 제한·비중첩 주소 등 설계로 pickle의 임의 코드 실행 위험을 없애고, CPU 로딩 시 불필요한 메모리 복사를 피한다.
- EN: A simple serialization format for storing model tensors safely (no code execution, unlike pickle) and fast (near zero-copy). Design constraints (header-size limit, non-overlapping addresses) remove pickle's arbitrary-code-execution risk, and it avoids an extra memory copy on CPU load.

**비유 (쉽게):** 짐을 부칠 때, 열면 무슨 일이 벌어질지 모르는 **봉인 안 된 상자(pickle)** 대신, **내용물 목록이 겉면에 규격대로 붙고 함부로 코드가 실행되지 않는 표준 컨테이너**에 담는 것. 열어보기도 빠르고 안전하다.

**왜 중요한가 / 언제 쓰나:**
- pickle 형식 가중치는 로딩 시 **임의 코드가 실행될 수 있어** 신뢰할 수 없는 모델 파일이 보안 위험이 된다. safetensors는 이 위험을 구조적으로 차단한다.
- 메모리 맵 기반 로딩으로 대형 모델을 빠르게 올린다(문서상 BLOOM을 8 GPU에서 10분→45초로 단축한 예).

**실무 예시 / AI에게 이렇게 말한다:**
- "이 체크포인트를 pickle 대신 safetensors로 저장해줘."

**흔한 오해:** safetensors가 가중치를 **압축**(양자화)한다고 여기는 것. safetensors는 텐서를 **안전·고속으로 담는 직렬화 포맷**이지 값의 비트 수를 줄이는 압축 기법이 아니다. "제로카피"도 문자 그대로 완전한 0복사가 아니라(디스크→RAM 전송은 있음) 불필요한 복사를 최소화한다는 뜻이다.

**함께 보기:** [GGUF](08-model-formats.md#gguf), [Quantization](08-model-formats.md#quantization--양자화)

**출처:** (원전 논문 없음 — 공식 저장소) Hugging Face, *safetensors*, [github.com/huggingface/safetensors](https://github.com/huggingface/safetensors).

---

### MoE · 전문가 혼합(Mixture of Experts)

> **한 줄 요약:** 입력마다 전체가 아니라 **일부 전문가 서브네트워크만** 켜서 계산해, 큰 용량을 적은 연산으로 쓰는 구조.

**정의 (Definition)**
- KO: 여러 개의 전문가(expert) 서브네트워크와, 입력마다 그중 일부만 고르는 게이팅(gating) 네트워크로 이뤄진 층. 각 입력은 소수 전문가만 활성화하므로 파라미터 수는 크되 **입력당 실제 계산량(활성 파라미터)은 작다(희소 활성화).**
- EN: A layer made of many expert sub-networks plus a gating network that selects only a few of them per input. Each input activates only a small subset, so the model has many parameters but low per-input compute (sparse activation).

**비유 (쉽게):** 모든 질문을 전 직원이 다 같이 붙어 처리하는 대신, **접수 담당(게이팅)이 질문마다 알맞은 전문가 몇 명에게만** 넘기는 회사. 회사 전체 규모는 크지만, 한 건에 실제 투입되는 사람은 소수다.

**왜 중요한가 / 언제 쓰나:**
- 파라미터(용량)를 크게 늘리면서도 입력당 계산 비용은 거의 그대로 두어, 같은 연산 예산으로 더 큰 모델을 만들 수 있다. Switch Transformer는 이 방식으로 조 단위 파라미터까지 확장했다.
- 최근 여러 대형 오픈·상용 모델이 MoE 구조를 채택한다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 모델이 MoE 구조인데, 입력당 활성화되는 전문가 수(top-k)가 몇인지 알려줘."

**흔한 오해:** MoE 모델이 "선언된 전체 파라미터를 매 입력마다 다 쓴다"고 여기는 것. **입력당 실제 활성 파라미터는 일부**다(그래서 총 파라미터 수와 계산량이 다르다). 또한 MoE는 단일 창시가 아니며, 아래는 현대 딥러닝에서의 대표 출처다.

**함께 보기:** [Distillation](08-model-formats.md#distillation--증류지식-증류), [Parameter/Weight](01-llm-basics.md)

**출처:** Shazeer et al. (2017), *Outrageously Large Neural Networks: The Sparsely-Gated Mixture-of-Experts Layer*, [arXiv:1701.06538](https://arxiv.org/abs/1701.06538); 확장 사례 — Fedus et al. (2021), *Switch Transformers*, [arXiv:2101.03961](https://arxiv.org/abs/2101.03961). (MoE 개념은 이보다 오래되었으며, 위는 딥러닝 대표 출처로 유일 창시가 아님.)

---

### Distillation · 증류(지식 증류)

> **한 줄 요약:** 크고 똑똑한 '교사' 모델의 지식을 작고 가벼운 '학생' 모델로 옮겨, 작은 모델의 성능을 끌어올리는 압축 기법.

**정의 (Definition)**
- KO: 큰 교사(teacher) 모델(또는 앙상블)의 출력 확률 분포 같은 '부드러운' 신호를 학생(student) 모델이 모방하도록 학습시켜, 지식을 작은 모델로 압축·이전하는 기법.
- EN: A technique that trains a small student model to mimic the "soft" signals (e.g., output probability distribution) of a large teacher model (or ensemble), compressing and transferring knowledge into the smaller model.

**비유 (쉽게):** 베테랑 선생님(교사 모델)이 정답만 알려주는 게 아니라 **"이건 이래서 이렇게 헷갈릴 수 있다"는 판단의 뉘앙스(부드러운 확률)까지** 함께 시범 보여, 학생(작은 모델)이 그 감각을 배우게 하는 것.

**왜 중요한가 / 언제 쓰나:**
- 큰 모델의 성능에 근접하면서 훨씬 작고 빠른 모델을 만들어, 배포·추론 비용을 낮춘다.
- 정답 라벨(0/1)만이 아니라 교사의 **확률 분포(soft targets)**를 학습해, 정답 라벨만 쓸 때보다 더 풍부한 정보를 전달한다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 큰 모델을 교사로 삼아 더 작은 학생 모델로 지식 증류해줘."

**흔한 오해:** 증류가 곧 양자화라고 여기는 것. **양자화는 같은 모델의 비트 수를 줄이는 것**이고, **증류는 지식을 별개의 더 작은 모델로 옮겨 새 모델을 만드는 것**이다. 둘 다 경량화 수단이지만 방식이 다르며, 함께 쓰기도 한다.

**함께 보기:** [Quantization](08-model-formats.md#quantization--양자화), [MoE](08-model-formats.md#moe--전문가-혼합mixture-of-experts)

**출처:** Hinton, Vinyals & Dean (2015), *Distilling the Knowledge in a Neural Network*, [arXiv:1503.02531](https://arxiv.org/abs/1503.02531).
