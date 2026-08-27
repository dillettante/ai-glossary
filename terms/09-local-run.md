# 9. 로컬 실행 · 셀프호스팅 · Running Models Locally

> 클라우드 API에 의존하지 않고, 내 노트북·데스크톱·사내 서버에서 오픈 모델을 직접 돌리는 도구와 개념들. 무거운 모델을 일반 하드웨어에서 켜는 엔진(llama.cpp)부터, 손쉬운 실행 도구(Ollama·LM Studio), 그리고 이 모든 것을 아우르는 운영 방식(셀프호스팅)까지.
> 출처 근거: [research/SOURCES.md](../research/SOURCES.md) 카테고리 9. 서식: [STYLE.md](../STYLE.md).

---

### VRAM · 지피유 전용 메모리 (Video RAM)

> **한 줄 요약:** GPU에 붙은 전용 메모리. 로컬에서 무엇을 돌릴 수 있고 무엇을 못 돌리는지를 사실상 혼자 정한다.

**정의 (Definition)**
- KO: GPU가 직접 읽고 쓰는 전용 메모리. 모델 가중치·활성값·중간 버퍼가 여기 올라가야 연산이 되며, 용량을 넘기면 실행이 실패한다(OOM).
- EN: Memory attached to the GPU that the device reads and writes directly; model weights and intermediate tensors must fit in it, and exceeding it causes out-of-memory failures.

**비유 (쉽게):** **작업대의 넓이**다. 창고(디스크)에 재료가 아무리 많아도, 작업대에 못 올리면 지금 만들 수 없다. 작업대를 넓히는 것이 곧 다룰 수 있는 물건의 크기를 정한다.

**왜 중요한가 / 언제 쓰나:**
- 로컬 실행·파인튜닝의 **첫 번째 제약**이다. 모델을 고르기 전에 재야 할 값이다.
- **학습은 추론보다 훨씬 많이 쓴다.** 가중치 말고도 옵티마이저 상태·그래디언트·활성값이 함께 올라간다(→ [Full fine-tuning](04-finetuning.md#full-fine-tuning--전체-파인튜닝)).
- 그래서 [양자화](08-model-formats.md#quantization--양자화)·[QLoRA](04-finetuning.md#qlora--큐로라-quantized-lora)·[SLM](08-model-formats.md#slm--소형-언어-모델-small-language-model)이 전부 같은 문제를 다른 방향에서 푼다.

**실무 예시 / AI에게 이렇게 말한다:**
- "내 GPU의 VRAM이 24GB인데, 이 모델을 4비트로 올리면 추론과 파인튜닝 각각 가능한지 계산해줘."

**흔한 오해:**
- **"모니터링 도구에 표시된 사용량 = 실제 텐서가 쓰는 양"** — 아니다. PyTorch는 **캐싱 할당자**를 쓰기 때문에, 반환된 메모리도 외부 도구에는 사용 중으로 보인다. 실제 텐서 사용량은 별도 함수로 봐야 한다.
- **"캐시를 비우면 GPU 메모리가 늘어난다"** — 이미 텐서가 점유한 메모리는 해제되지 않는다. 비워지는 것은 **쓰이지 않는 캐시**뿐이다.
- **"시스템 RAM으로 대체된다"** — 일부 오프로딩이 가능하지만 속도가 크게 떨어진다. 통합 메모리 구조(→ [MLX](08-model-formats.md#mlx))는 사정이 다르다.

**함께 보기:** [Quantization](08-model-formats.md#quantization--양자화), [SLM](08-model-formats.md#slm--소형-언어-모델-small-language-model), [Active parameters](08-model-formats.md#active-parameters--활성-파라미터), [Self-hosting](09-local-run.md#self-hosting--셀프호스팅)

**출처:** PyTorch, *CUDA semantics — Memory management*, [docs.pytorch.org](https://docs.pytorch.org/docs/stable/notes/cuda.html) ("PyTorch uses a caching memory allocator to speed up memory allocations."; "the unused memory managed by the allocator will still show as if used in `nvidia-smi`"; "the occupied GPU memory by tensors will not be freed so it can not increase the amount of GPU memory available for PyTorch"; 확인 2026-08-27). (하드웨어 일반 용어 — 유일 창시 없음.)

---

### llama.cpp · 라마씨피피

> **한 줄 요약:** 노트북·데스크톱 같은 평범한 하드웨어에서도 LLM을 돌릴 수 있게 만든 C/C++ 경량 추론 엔진.

**정의 (Definition)**
- KO: GGML 라이브러리 위에 구현된, 외부 의존성 없는 순수 C/C++ LLM **추론** 엔진. 모델을 GGUF 형식으로 변환해 CPU와 다양한 GPU 백엔드에서 최소 설정으로 실행한다.
- EN: "LLM inference in C/C++" — a dependency-free implementation that enables LLM inference with minimal setup and state-of-the-art performance across a wide range of hardware (CPU and many GPU backends), using the GGUF model format.

**비유 (쉽게):** 슈퍼컴 전용으로 만들어진 무거운 소프트웨어를, **집에 있는 낡은 노트북에서도 켜지도록 다이어트시킨 엔진.** 군더더기(외부 라이브러리 의존성)를 걷어내 어디서든 가볍게 돈다.

**왜 중요한가 / 언제 쓰나:**
- 클라우드 없이 **내 기기**에서 오픈 모델을 실행하고 싶을 때. Apple Silicon(Metal)·x86(AVX)·NVIDIA(CUDA)·AMD·Vulkan 등 폭넓은 하드웨어를 지원한다.
- Ollama·LM Studio 등 많은 로컬 실행 도구가 내부적으로 이 엔진(또는 그 파생)을 **하부 백엔드**로 쓴다 — 로컬 실행 생태계의 기반 부품이다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 GGUF 모델을 llama.cpp로 CPU에서 돌려줘."
- "llama.cpp 서버 모드로 띄워서 API로 붙일 수 있게 해줘."

**흔한 오해:**
- **"모델을 학습(훈련)하는 도구다"** — 아니다. 학습이 아니라 이미 만들어진 모델을 실행하는 **추론 엔진**이다.
- **"로컬 실행의 유일한 방법이다"** — 아니다. MLX(Apple)·vLLM·ExLlama 등 다른 실행 엔진도 병존한다. llama.cpp는 그중 가장 널리 쓰이는 하나일 뿐이다.

**함께 보기:** [GGUF / 모델 포맷](08-model-formats.md), [Inference](01-llm-basics.md), [Ollama](#ollama--올라마), [LM Studio](#lm-studio--엘엠-스튜디오)

**출처:** ggml-org, *llama.cpp — LLM inference in C/C++*, [github.com/ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp) (확인 2026-07-11). GGML 프로젝트 창시: Georgi Gerganov. (특정 도구 — 유일 창시 아님)

---

### Ollama · 올라마

> **한 줄 요약:** 명령어 한 줄로 오픈 모델을 받아 로컬에서 실행하게 해주는 도구.

**정의 (Definition)**
- KO: 오픈 웨이트 모델을 내려받아 로컬에서 실행·관리하는 오픈소스(MIT) 도구. 초기에는 llama.cpp를 실행 백엔드로 썼고, 2025년부터는 GGML 기반의 자체 러너(Go)도 병행한다(모델에 따라 경로가 다름).
- EN: An open-source (MIT-licensed) tool to "get up and running with" open models locally, using backends such as the llama.cpp project (founded by Georgi Gerganov).

**비유 (쉽게):** 앱스토어에서 앱을 설치하듯, **`ollama run <모델>` 한 줄**로 모델을 받아 곧바로 켜는 것. 어려운 변환·설정을 도구가 대신 처리해준다.

**왜 중요한가 / 언제 쓰나:**
- macOS·Windows·Linux·Docker에서 모델 다운로드·전환·삭제를 명령 한 줄로 관리하고 싶을 때.
- 로컬 API 서버를 함께 띄워, 내 코드가 로컬 모델에 붙게 할 때.

**실무 예시 / AI에게 이렇게 말한다:**
- "ollama로 오픈 모델 받아서 로컬에서 돌려줘."
- "ollama 서버를 띄우고 사내 스크립트가 붙게 해줘."

**흔한 오해:**
- **"밑바닥부터 새 추론 엔진을 짰다"** — 아니다. Ollama는 처음엔 llama.cpp를 백엔드로 썼고, 이후 GGML 텐서 라이브러리 위에 자체 러너(Go)를 더했다. 커널을 맨땅에서 짜기보다 **기존 엔진·라이브러리 위에 모델 관리·배포·API를 편하게 얹은 계층**에 가깝다.
- **"로컬 실행은 Ollama뿐"** — 아니다. LM Studio(GUI)나 llama.cpp 직접 실행 등 대안이 있다. 용도(CLI·GUI·서버)에 따라 고른다.

**함께 보기:** [llama.cpp](#llamacpp--라마씨피피), [LM Studio](#lm-studio--엘엠-스튜디오), [GGUF / 모델 포맷](08-model-formats.md)

**출처:** Ollama, [github.com/ollama/ollama](https://github.com/ollama/ollama) (MIT, 확인 2026-07-11) · [ollama.com](https://ollama.com). (특정 도구 — 유일 창시 아님)

---

### LM Studio · 엘엠 스튜디오

> **한 줄 요약:** 로컬에서 LLM을 받아 실행하고, OpenAI 호환 API 서버까지 띄워주는 GUI 앱.

**정의 (Definition)**
- KO: 로컬 하드웨어에서 LLM을 실행하는 GUI 데스크톱 앱 겸 헤드리스 서버. llama.cpp와 Apple MLX를 엔진으로 GGUF·MLX 모델을 돌리고, OpenAI 호환 엔드포인트로 서빙한다.
- EN: A desktop GUI app and headless server that runs LLMs locally on Mac/Windows/Linux, using llama.cpp and Apple's MLX as engines, and serves models on OpenAI-compatible endpoints.

**비유 (쉽게):** 여러 로컬 모델을 클릭 몇 번으로 받아 켜고, 다른 프로그램이 **"OpenAI인 척" 붙을 수 있는 콘센트(호환 API)**를 열어주는 스튜디오. 기존 OpenAI용 코드의 접속 주소만 로컬로 바꾸면 그대로 붙는다.

**왜 중요한가 / 언제 쓰나:**
- 명령줄이 익숙하지 않아도 **GUI로** 로컬 모델을 받아 실행하고 싶을 때.
- 이미 OpenAI SDK로 짠 코드를, 접속 주소(base URL)만 로컬 서버로 바꿔 **거의 그대로 재사용**하고 싶을 때.
- 데이터를 외부로 보내지 않고 오프라인으로 처리해야 할 때(프라이버시).

**실무 예시 / AI에게 이렇게 말한다:**
- "LM Studio에서 모델 로드하고 로컬 서버 켜서 OpenAI 호환 API로 붙여줘."

**흔한 오해:**
- **"오픈소스다"** — 앱 자체는 가정·업무 무료로 쓸 수 있으나, 그것이 곧 오픈소스라는 뜻은 아니다. 공개된 것은 하부 엔진(llama.cpp·MLX)과 LM Studio의 CLI·SDK(`lms`, MIT)이며, **데스크톱 앱 본체는 독점**이다.
- **"OpenAI 호환 API = OpenAI를 쓰는 것"** — 아니다. 응답 **형식**만 OpenAI와 같게 맞춘 로컬 서버이며, 실제 연산은 내 기기에서 로컬 모델이 한다.
- **"유일한 GUI 실행 도구"** — 아니다. Ollama(+GUI 프런트엔드)나 다른 데스크톱 앱도 있다.

**함께 보기:** [llama.cpp](#llamacpp--라마씨피피), [Ollama](#ollama--올라마), [Tool use / API](03-building.md), [GGUF / 모델 포맷](08-model-formats.md)

**출처:** LM Studio, *Run AI models, locally and privately*, [lmstudio.ai](https://lmstudio.ai) · 엔진·API: [lmstudio.ai/docs](https://lmstudio.ai/docs) (확인 2026-07-11). (특정 제품 — 유일 창시 아님)

---

### Self-hosting · 셀프호스팅

> **한 줄 요약:** 외부 클라우드에 맡기지 않고, 내 서버·PC에서 직접 소프트웨어·모델·서비스를 운영하는 것.

**정의 (Definition)**
- KO: 웹사이트·서비스(또는 AI 모델)를 제3자 클라우드가 아니라, 관리자 통제 하의 자체(사설) 서버에서 직접 운영·유지하는 방식.
- EN: The practice of running and maintaining a service (or model) on one's own private server, under the administrator's own control, instead of using a service outside that control.

**비유 (쉽게):** 밥을 매번 **식당(클라우드)**에서 사 먹는 대신 **내 집 부엌(내 서버)**에서 직접 해 먹는 것. 재료·위생·비용을 스스로 통제하지만, 설거지와 관리도 내 몫이다.

**왜 중요한가 / 언제 쓰나:**
- 데이터·프라이버시를 스스로 통제해야 할 때. 사내 기밀·개인정보를 **외부로 전송하지 않고** 처리할 수 있다(법률·의료 등 기밀 업무에 특히).
- 외부 서비스 종속과 장기 비용을 피하고 싶을 때. 위 llama.cpp·Ollama·LM Studio는 모델을 셀프호스팅하는 대표 수단이다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 모델을 외부 API 대신 사내 서버에 셀프호스팅해서 데이터가 밖으로 안 나가게 해줘."

**흔한 오해:**
- **"무조건 더 싸고 쉽다"** — 아니다. 서버 운영·보안 패치·가용성·업데이트 부담이 모두 **본인 몫**이다. 규모·역량에 따라 클라우드보다 비싸거나 어려울 수 있다.
- **"특정 창시자·표준 정의가 있다"** — 없다. 셀프호스팅은 단일 창시나 정본 정의가 없는 **일반 개념**이며, 클라우드(예: 2006년 AWS 등장) 확산과 오픈소스 운동을 배경으로 형성됐다.

**함께 보기:** [llama.cpp](#llamacpp--라마씨피피), [Ollama](#ollama--올라마), [LM Studio](#lm-studio--엘엠-스튜디오)

**출처:** 단일 정본 없는 일반 개념(유일 창시 없음). 보조 설명: Wikipedia, *Self-hosting (web services)*, [en.wikipedia.org](https://en.wikipedia.org/wiki/Self-hosting_(web_services)) (확인 2026-07-11).

---

### Open weights vs Open source · 오픈 웨이트 vs 오픈소스

> **한 줄 요약:** 가중치를 내려받아 쓸 수 있는 것(오픈 웨이트)과, 사용·연구·수정·재배포가 자유롭고 학습 데이터 정보·코드까지 공개된 것(오픈소스)은 다르다 — "가중치 공개 ≠ 오픈소스".

**정의 (Definition)**
- KO: **오픈 웨이트**는 학습이 끝난 모델의 가중치(파라미터)를 내려받아 실행·파인튜닝할 수 있게 공개한 것을 말한다. 다만 학습 데이터·코드가 공개되었는지, 라이선스가 상업 이용·재배포를 어디까지 허용하는지는 **별개의 문제**다. **오픈소스(AI)**는 OSI의 *Open Source AI Definition*(OSAID) 기준으로, 사용·연구·수정·재배포의 자유가 보장되고 학습 데이터에 관한 충분한 정보와 학습·실행 코드까지 공개된 것을 말한다.
- EN: **Open weights** means the trained model's weights are published so they can be run and fine-tuned — but whether the training data/code are released, and how far the license permits commercial use or redistribution, are separate questions. **Open source (AI)**, per the OSI Open Source AI Definition (OSAID v1.0), grants the freedoms to use, study, modify, and share, and additionally requires sufficiently detailed information about the training data plus the code used to train and run the system.

**비유 (쉽게):** 오픈 웨이트는 **완성된 요리(가중치)를 통째로 받아 먹고 다시 간을 볼 수 있는 것**. 오픈소스는 그 요리의 **레시피와 재료 출처(학습 데이터·코드)까지 공개하고, 누구나 고쳐서 다시 팔아도 된다고 허락한 것**. 요리를 받았다고 레시피까지 받은 것은 아니다.

**왜 중요한가 / 언제 쓰나:**
- 모델을 셀프호스팅·파인튜닝·재배포하려 할 때, "받았으니 마음대로 써도 되는지"는 **가중치 공개 여부가 아니라 라이선스**가 정한다. 상업 이용 금지·사용자 수 상한·재배포 제한이 붙어 있을 수 있다.
- 실제로 널리 쓰이는 다수 모델(예: Llama, DeepSeek 계열)은 가중치를 내려받을 수 있어 흔히 "오픈"으로 불리지만, 그 라이선스가 OSI 기준의 오픈소스인지는 논쟁·검토 대상이다(예: Llama 라이선스에 붙은 사용 조건을 두고 OSI가 "오픈소스가 아니다"라고 본 논쟁).
- 법률·기밀 업무에서 모델을 도입하기 전, **라이선스 조항(상업 이용·재배포·책임·출력물 권리)**을 실제로 확인해야 한다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 모델이 오픈 웨이트인지 OSI 기준 오픈소스인지 구분하고, 상업 이용·재배포를 허용하는 라이선스인지 확인해줘."

**흔한 오해:**
- **"무료로 받았으니 마음대로 써도 된다"** — 아니다. 무상 배포와 자유로운 사용은 별개다. 가중치가 공개돼도 라이선스가 상업 이용·재배포를 제한할 수 있다.
- **"'open'이 붙으면 오픈소스다"** — 아니다. 제품·모델명이나 홍보의 "open"은 OSI가 정의한 오픈소스와 같지 않다. 오픈 웨이트만으로는 OSAID의 오픈소스 요건(학습 데이터 정보·코드 공개)을 충족하지 못할 수 있다.

**함께 보기:** [Self-hosting](09-local-run.md#self-hosting--셀프호스팅), [Ollama](09-local-run.md#ollama--올라마), [llama.cpp](09-local-run.md#llamacpp--라마씨피피)

**출처:** Open Source Initiative, *The Open Source AI Definition — 1.0* (OSAID v1.0, 2024), [opensource.org/ai](https://opensource.org/ai/open-source-ai-definition) (확인 2026-07-12 — 사용·연구·수정·공유 4대 자유 + 학습 데이터 정보·학습/실행 코드·가중치 공개 요건). (일반 개념·라이선스 판단은 개별 모델 라이선스 원문 우선)
