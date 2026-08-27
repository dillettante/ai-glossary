# 변경 이력 · Changelog

이 용어집의 의미 있는 변경을 기록합니다. **정의·출처가 바뀌면 이 글을 인용한 쪽에 영향**이 있으므로, 무엇이 언제 바뀌었는지 남깁니다.

형식은 [Keep a Changelog](https://keepachangelog.com/ko/1.1.0/), 버전은 [유의적 버전](https://semver.org/lang/ko/)을 따릅니다. 이 문서에서는 다음과 같이 해석합니다:

- **MAJOR** — 카테고리 체계 개편 등 구조 변경(기존 링크가 깨질 수 있음)
- **MINOR** — 용어 추가, 카테고리 추가
- **PATCH** — 정의·출처·오탈자 수정(내용 정정 포함)

## [Unreleased]

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
