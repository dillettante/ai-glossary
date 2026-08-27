# 변경 이력 · Changelog

이 용어집의 의미 있는 변경을 기록합니다. **정의·출처가 바뀌면 이 글을 인용한 쪽에 영향**이 있으므로, 무엇이 언제 바뀌었는지 남깁니다.

형식은 [Keep a Changelog](https://keepachangelog.com/ko/1.1.0/), 버전은 [유의적 버전](https://semver.org/lang/ko/)을 따릅니다. 이 문서에서는 다음과 같이 해석합니다:

- **MAJOR** — 카테고리 체계 개편 등 구조 변경(기존 링크가 깨질 수 있음)
- **MINOR** — 용어 추가, 카테고리 추가
- **PATCH** — 정의·출처·오탈자 수정(내용 정정 포함)

## [Unreleased]

### Changed — 축약어 풀네임 병기 (2026-08-27)

축약어 표제에 풀네임을 병기했다. **다만 공식 확장형이 있는 것만.**

- 병기함: `LoRA(Low-Rank Adaptation)` · `QLoRA(Quantized LoRA)` · `SSH(Secure Shell)` · `CLI(Command-Line Interface)` · `API(Application Programming Interface)`
- **병기하지 않고 「이름에 관하여」로 사실을 적은 것 3건** — `GGUF`·`MLX`·`BitFit`. 셋 다 **공식 문서·원 논문이 이름을 풀어 쓰지 않는다**(2026-08-27 각 원문 확인). 널리 도는 "GPT-Generated Unified Format" 같은 표기는 비공식 통용어이므로 정본으로 쓰지 않고, 그 사실 자체를 항목에 남겼다.
- `Vector DB`의 "DB"는 병기 가치가 낮아 두었다.

### Added — 경량화·하드웨어·표기 표준 5건 (2026-08-27)

앞 커밋에서 "출처 확인이 남아 뺐다"고 적은 후보들을 **원문을 열어 확인하고** 채웠다.

- **Pruning · 프루닝** — Ch.8의 빠진 세 번째 경량화 갈래(양자화·증류는 있었다). 출처: Han et al. (2015), arXiv:1506.02626. AlexNet 61M→6.7M 등 원 수치를 **2015년 비전 모델 기준임을 밝혀** 인용했다.
- **SLM · 소형 언어 모델** — LLM은 있는데 대조어가 없었다. ⚠ **파라미터 기준에 합의된 정의가 없다**는 점을 항목의 핵심으로 삼았다(서베이마다 100M–5B, 1B–8B, 최대 10B). 출처: Lu et al. (2024), arXiv:2409.15790.
- **Active parameters · 활성 파라미터** — MoE는 있는데 그 실무적 귀결이 없었다. 총 파라미터는 메모리를, 활성은 속도·단가를 좌우한다. 출처: Jiang et al. (2024), arXiv:2401.04088.
- **VRAM** — 로컬 실행 장(Ch.9)의 가장 큰 구멍. "모니터링 도구의 표시량 ≠ 실제 텐서 사용량"(캐싱 할당자) 함정을 「흔한 오해」에 넣었다. 출처: PyTorch CUDA semantics 공식 문서.
- **CommonMark / GFM** — 표·체크박스가 **표준이 아니라 GitHub 확장**이라는 사실. 문서가 다른 곳에서 깨지는 원인이다. 출처: GitHub Flavored Markdown Spec(공식 명세).

전체 96 → **101개 항목**. 다섯 건 모두 2026-08-27에 원문 페이지를 열어 제1저자·연도·정의 문구를 대조했다.

### Added — Harness · Base/Instruct · 학습·검증·테스트 분리 (2026-08-27)

raw 승격 과정에서 나온 후보 중 **출처를 댈 수 있는 것만** 골라 넣었다.

- **Harness · 하네스** — 이 용어집에서 가장 큰 구멍이었다. 벤치마크 점수를 읽을 때 반드시 확인해야 할 변수인데 항목이 없었다. ⚠ **표준 정의가 없다** — Anthropic 공식 용어집에도 항목이 없음을 확인하고 그 사실을 항목에 적었다. 평가 하네스(EleutherAI, 2021)와 에이전트 하네스(Anthropic 블로그, 2026)의 두 쓰임을 함께 정리.
- **Base model vs Instruct model** — 파인튜닝 출발점 선택의 첫 갈림길인데 대조 항목이 없었다. 출처: Anthropic 공식 용어집의 Pretraining·Fine-tuning 항목.
- **Training / Validation / Test split** — [Overfitting]은 있는데 **그 처방이 없었다.** 3분할을 2분할보다 권하는 이유(테스트셋이 독립성을 잃는다)까지 담았다. 출처: Google ML Crash Course.

전체 93 → **96개 항목**.

**넣지 않은 후보와 이유** — raw에서 20건 이상이 후보로 올라왔지만, 상당수가 **한 유튜버의 조어**이거나(Company Brain·마크다운 직원·시동 의례·공유 두뇌·증류된 이해·궤적·품질 게이트) **원 보고서가 확인되지 않는 것**(Context Rot·프리픽스 슬라이딩)이라 뺐다. 이 용어집의 기준은 "출처 없는 정의 = 미완성"이고, 통용되지 않는 조어를 실으면 용어집이 아니라 특정 화자의 어휘집이 된다. 표준 출처가 생기면 그때 넣는다.

### Added — Full fine-tuning (2026-08-27)

- **Full fine-tuning · 전체 파인튜닝** — 직전 커밋에서 PEFT를 넣으면서 **그 대조군을 비워 둔 것**을 메웠다. "일부만 학습한다"는 말은 기준점이 있어야 성립한다. 학습 메모리가 가중치만이 아니라는 점(옵티마이저 상태·그래디언트·활성화값)을 「흔한 오해」에 넣었다. 출처: Hu et al. (2021), arXiv:2106.09685.

전체 92 → **93개 항목**.

### Added — 용어 4개 (2026-08-27)

파인튜닝 자료를 정리하다 **용어집이 쓰면서 정의하지 않은 말**들이 드러나 채웠다.

- **PEFT · 파라미터 효율 파인튜닝** — 카테고리 4의 이름이자 LoRA 정의 안에서 이미 쓰이고 있었는데 정작 항목이 없었다. 총칭 개념이라 유일 창시가 없음을 명시. 출처: Hugging Face PEFT 공식 문서.
- **SFT · 지도 파인튜닝** — Instruction Tuning은 있는데 그 상위인 SFT가 없었다. 출처: Ouyang et al. (2022), arXiv:2203.02155(InstructGPT 1단계).
- **Catastrophic forgetting · 파국적 망각** — 파인튜닝의 대표적 부작용이고 과적합과 자주 혼동된다. 두 개념의 차이를 「흔한 오해」에 명시. 출처: Kirkpatrick et al. (2016), arXiv:1612.00796.
- **Tokenizer · 토크나이저** — Token은 있는데 그것을 만드는 쪽이 없었다. 비용·컨텍스트 소모가 여기서 정해진다는 점과 한국어 토큰 수 불이익을 다룸. 출처: Sennrich et al. (2015), arXiv:1508.07909(BPE).

전체 88 → **92개 항목**. 네 건 모두 arXiv ID·제1저자·연도 또는 공식문서 문구를 **2026-08-27에 실제 페이지를 열어 대조**했고, `research/SOURCES.md`에 「5차 확장」으로 등재했다.


## [1.0.0] — 2026-07-15

**최초 공개 릴리스.** 88개 항목 · 12개 카테고리 · 국·영 병기 · 전 항목 검증 출처.

### 추가 — 12개 카테고리

| # | 카테고리 | 항목 |
|---|---|---|
| 1 | LLM 기초 | LLM/Foundation model, Transformer/Attention, Token, Context window, Embedding, Hallucination, Inference, Parameter/Weight, Temperature, Multimodal, Knowledge cutoff, Generative AI, Pretraining, Diffusion model |
| 2 | 프롬프트 & 상호작용 | Prompt, System prompt, Few-shot/In-context learning, Chain-of-thought, Reasoning/Thinking mode, Context engineering, Prompt injection/Jailbreak |
| 3 | AI 서비스 구축 | RAG, MCP, Agent, Tool use/Function calling, Vector DB/Embedding search, Embedding model, Chunking, Reranking, Structured output/JSON mode, Agent memory |
| 4 | 모델 커스터마이징(파인튜닝) | LoRA, QLoRA, Prefix Tuning, Adapter Tuning, Instruction Tuning, P-Tuning(v1/v2), BitFit, Soft Prompts/Prompt Tuning, Multi-Task FT, Federated FT, Overfitting |
| 5 | 정렬 · 강화학습 | Alignment, RLHF, RLAIF, DPO, GRPO (+ 사례: OpenPipe ART/RULER) |
| 6 | 바이브코딩 워크플로우 | Vendoring, Porting, Refactoring, Scaffolding, Wrapping, Modularization |
| 7 | 개발 단계 · 품질 | PoC, MVP, Production Ready, Evals |
| 8 | 모델 포맷 · 경량화 | Quantization, GGUF, MLX, safetensors, MoE, Distillation |
| 9 | 로컬 실행 · 셀프호스팅 | llama.cpp, Ollama, LM Studio, Self-hosting, Open weights vs Open source |
| 10 | 개발 · 인프라 기초 | SSH, CLI, cron, Docker/Container, API, Port/localhost, Environment variable |
| 11 | 버전 관리 · 협업 | Git vs GitHub, Repository, Commit, Branch/Merge, Clone/Pull/Push, Fork, Pull request |
| 12 | 안전 · 거버넌스 | Guardrails, Red-teaming, Model card, Watermarking, EU AI Act 위험등급 |

### 부속 문서
- `TABLE.md` — 전체 88개 한 장 표(각 용어 파일에서 자동 생성)
- `research/SOURCES.md` — 용어별 검증 출처 화이트리스트
- `STYLE.md` — 항목 템플릿·집필 규칙 / `CONTRIBUTING.md` — 기여 규칙

### 집필 원칙 (v1.0 기준)
- **모든 정의에 검증된 출처.** arXiv 원논문·공식 문서·법령 원문을 **실제로 열어** 대조(제목·제1저자·연도·ID).
- **단일 창시가 없는 용어**는 "대표 출처이며 유일 창시 아님"을 본문에 명시(중립성).
- **특정 제품은 표제어로 올리지 않고** 개념 항목에 예시로 흡수(추천처럼 읽히는 것·노후화 회피). 예: Qdrant 등은 Vector DB 항목 안에.
- 각 항목에 **흔한 오해**를 넣어 자주 틀리는 지점을 교정.

### 검증 기록 (v1.0에서 실제로 바로잡은 것들)
- **EU AI Act** 조문번호는 EUR-Lex 규정 텍스트로 축자 확인 후 기재(투명성=Art.50, GPAI=Ch.V Art.51·53·55).
- **GRPO** 원출처를 DeepSeek-R1이 아닌 **DeepSeekMath(2024)**로 정정.
- **DPO는 강화학습이 아님**(보상모델·롤아웃 없는 분류 손실)을 명시.
- **Instruction Tuning은 PEFT가 아님**(완전 미세조정)을 명시.
- **Ollama**의 실행 엔진 서술을 현재 상태(GGML 기반 자체 러너 병행)로 갱신.
- **OSAID**는 학습 데이터 원본이 아니라 "데이터에 관한 정보"를 요구한다는 점을 정밀화.
- 존재하지 않는 arXiv ID 1건(집필 도구가 생성)을 검증 단계에서 폐기.

[Unreleased]: https://github.com/dillettante/ai-glossary/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/dillettante/ai-glossary/releases/tag/v1.0.0
