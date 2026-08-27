<!-- 제목: AI 용어집 바이블 -->

# AI 용어집 바이블 · The AI Vocabulary Bible

> AI 시대에 누구나 쓰는 실용 용어집. AI와 **함께 일하고**, AI를 **직접 맞춤화**할 때 만나는 핵심 용어를, **비유·실무 예시·검증된 출처**와 함께 정리합니다.

AI 코딩 도구(Claude Code, Cursor, Codex, Gemini CLI 등)와 협업하거나, 나만의 모델을 파인튜닝하려 할 때 — 이 용어들만 익히면 대화가 통합니다. 개발자가 아니어도 읽을 수 있게 썼습니다.

**원칙:** 모든 정의에 검증된 출처를 답니다. 추측·할루시네이션을 넣지 않습니다. → [SOURCES](research/SOURCES.md) · [집필 규칙](CONTRIBUTING.md)

**전체 용어를 한 장 표로:** [TABLE.md](TABLE.md) (93개 항목 한눈에)

---

## 🚀 치트시트

### 1. LLM 기초 — [자세히](terms/01-llm-basics.md)
| 용어 | 한 줄 요약 |
|---|---|
| Token (토큰) | 모델이 읽고 쓰는 최소 단위(단어보다 작거나 클 수 있음) |
| Tokenizer (토크나이저) | 글을 토큰으로 자르는 규칙·프로그램(비용과 길이가 여기서 정해짐) |
| Context window (컨텍스트 윈도우) | 한 번에 참조 가능한 텍스트 양 = 모델의 작업기억 |
| Embedding (임베딩) | 데이터를 의미관계를 담은 숫자 벡터로 바꾼 표현 |
| Hallucination (환각) | 모델이 사실 아닌 내용을 그럴듯하게 지어내는 현상 |
| Inference (추론·실행) | 학습 끝난 모델을 실제로 써서 답을 생성하는 단계 |
| Parameter/Weight (파라미터·가중치) | 학습으로 조정되는 모델 내부 변수(=수백억 개 손잡이) |
| Temperature (온도) | 출력의 무작위성 조절값(高=창의, 低=보수) |
| Multimodal (멀티모달) | 이미지·음성·영상까지 함께 다루는 모델 |
| Knowledge cutoff (지식 컷오프) | 학습이 멈춘 시점 — 이후는 도구 없이 모름 |
| LLM / Foundation model | "다음 말" 예측으로 언어 이해·생성하는 거대 신경망(범용=파운데이션) |
| Transformer / Attention | 현대 LLM의 뼈대 — 단어 간 '주의'로 문맥 파악 |
| Generative AI (생성형 AI) | 새 콘텐츠(글·이미지·코드)를 만드는 AI 총칭 |
| Pretraining (사전학습) | 과제 학습 전, 대규모 데이터로 기초 체력 기르기 |
| Diffusion model (디퓨전) | 노이즈를 걷어내며 이미지·영상 생성(미드저니·SD·Sora) |

### 2. 프롬프트 & 상호작용 — [자세히](terms/02-prompting.md)
| 용어 | 한 줄 요약 |
|---|---|
| Prompt (프롬프트) | 응답을 끌어내려 모델에 보내는 자연어 요청 |
| System prompt (시스템 프롬프트) | 모델의 역할·어조·경계를 앞단에서 규정하는 지시문 |
| Few-shot / In-context learning | 프롬프트 안 예시 몇 개로 그 자리에서 과제 시연 |
| Chain-of-thought (생각의 사슬) | 중간 풀이 단계를 쓰게 해 복잡추론 성능↑ |
| Reasoning / Thinking mode (추론 모드) | 답 전에 속으로 더 길게 '생각'하는 내장 모드 |
| Context engineering (컨텍스트 엔지니어링) | 컨텍스트에 무엇을 채울지 통째로 설계 |
| Prompt injection / Jailbreak (인젝션/탈옥) | 숨은 지시로 모델 탈취 vs 안전장치 우회 |

### 3. AI 서비스 구축 — [자세히](terms/03-building.md)
| 용어 | 한 줄 요약 |
|---|---|
| RAG (검색 증강 생성) | 외부 문서를 검색해 근거로 답을 생성(오픈북 시험) |
| MCP (모델 컨텍스트 프로토콜) | AI와 외부 도구·데이터를 잇는 개방형 표준(AI용 USB-C) |
| Agent (에이전트) | 목표를 받아 스스로 도구를 쓰고 여러 단계를 수행하는 AI |
| Tool use / Function calling | 모델이 필요 시 정의된 도구를 스스로 호출 |
| Vector DB / Embedding search | 의미가 가까운 것끼리 찾아주는 벡터 저장·검색 |
| Embedding model (임베딩 모델) | 데이터를 임베딩 벡터로 바꾸는 전용 모델 |
| Chunking (청킹) | 긴 문서를 검색·임베딩용 조각으로 자르기 |
| Reranking (리랭킹) | 1차 검색 후보를 정밀 모델로 재정렬 |
| Structured output / JSON mode | 정해진 틀(JSON)에 맞춰 답하게 강제 |
| Agent memory (에이전트 메모리) | 세션 넘어 정보 저장·회상해 맥락 유지 |

### 4. 모델 커스터마이징 (파인튜닝) — [자세히](terms/04-finetuning.md)
| 용어 | 한 줄 요약 |
|---|---|
| Full fine-tuning (전체 파인튜닝) | 모든 가중치를 다시 학습 — PEFT의 대조군 |
| PEFT (파라미터 효율 파인튜닝) | 일부 파라미터만 학습하는 기법군의 총칭(4번 카테고리의 상위 개념) |
| SFT (지도 파인튜닝) | 모범 답안 쌍으로 다시 가르치는 파인튜닝의 기본형 |
| LoRA / QLoRA (Low-Rank Adaptation / Quantized LoRA) | 원본은 얼리고 작은 저계수 행렬만 학습(+4bit 양자화) |
| Prefix / Adapter / P-Tuning / BitFit / Soft Prompts | 소수 파라미터만 학습하는 경량 파인튜닝(PEFT) 계열 |
| Instruction Tuning | 지시문 데이터로 튜닝(※PEFT 아님·완전 미세조정) |
| Multi-Task / Federated FT | 여러 과제 동시 / 데이터 반출 없이 분산 학습 |
| Overfitting (과적합) | 학습 데이터에 과하게 맞춰져 새 데이터서 성능 하락 |
| Catastrophic forgetting (파국적 망각) | 새 과제를 배우며 이전 과제 성능을 잃는 현상 |

### 5. 정렬 · 강화학습 — [자세히](terms/05-alignment-rl.md)
| 용어 | 한 줄 요약 |
|---|---|
| RLHF / RLAIF | 인간(또는 AI) 선호로 보상모델 학습→RL 미세조정 |
| DPO | 보상모델·RL 없이 선호데이터로 직접 최적화(RL 아님) |
| GRPO | critic 없이 그룹 내 상대우열로 학습(DeepSeekMath 2024) |
| Alignment (정렬) | AI를 인간 의도·가치에 안전하게 맞추기(RLHF 등이 방법) |

### 6. 바이브코딩 워크플로우 — [자세히](terms/06-vibe-coding.md)
| 용어 | 한 줄 요약 |
|---|---|
| Vendoring (벤더링) | 외부 코드 복사본을 내 프로젝트에 넣어 고정 |
| Porting (포팅) | 다른 환경(언어·플랫폼)에서 돌게 고쳐 옮김 |
| Refactoring (리팩터링) | 동작은 그대로, 내부 구조만 개선 |
| Scaffolding (스캐폴딩) | 기본 뼈대·보일러플레이트 자동 생성 |
| Wrapping (래핑) | 기존 코드를 껍데기로 감싸 쓰기 쉽게 |
| Modularization (모듈화) | 기능을 독립 모듈로 분리 |

### 7. 개발 단계 · 품질 — [자세히](terms/07-dev-stages.md)
| 용어 | 한 줄 요약 |
|---|---|
| PoC (개념 증명) | 기술적으로 되는지만 빠르게 확인하는 일회용 실험 |
| MVP (최소 기능 제품) | 최소 노력으로 배움을 최대로 얻는 출시 가능 제품 |
| Production Ready (프로덕션 레디) | 실제 운영에 안전히 올릴 수 있는 상태 |
| Evals (평가·벤치마크) | 정해진 과제·지표로 성능 측정, 점수의 근거 읽기 |

### 8. 모델 포맷·경량화 — [자세히](terms/08-model-formats.md)
| 용어 | 한 줄 요약 |
|---|---|
| Quantization (양자화) | 가중치 비트 수를 줄여 크기·메모리 압축 |
| GGUF | llama.cpp 생태계의 모델 파일 포맷 |
| MLX | 애플 실리콘용 ML 프레임워크 |
| safetensors | 안전·고속 텐서 저장 포맷(pickle 대체) |
| MoE (전문가 혼합) | 입력마다 일부 전문가만 켜는 구조 |
| Distillation (증류) | 큰 교사 모델 지식을 작은 모델로 이전 |

### 9. 로컬 실행·셀프호스팅 — [자세히](terms/09-local-run.md)
| 용어 | 한 줄 요약 |
|---|---|
| llama.cpp | 평범한 하드웨어용 경량 LLM 추론 엔진 |
| Ollama | 명령 한 줄로 오픈 모델 로컬 실행 |
| LM Studio | 로컬 LLM 실행·API 서버 GUI 앱 |
| Self-hosting (셀프호스팅) | 내 서버·PC에서 직접 운영 |
| Open weights vs Open source | 가중치 공개 ≠ 오픈소스(라이선스가 좌우) |

### 10. 개발·인프라 기초 — [자세히](terms/10-dev-infra.md)
| 용어 | 한 줄 요약 |
|---|---|
| SSH (Secure Shell) | 원격 컴퓨터에 암호화 채널로 안전 접속 |
| CLI (커맨드라인 인터페이스 · Command-Line Interface) | 텍스트 명령으로 프로그램 조작 |
| cron (크론) | 정해둔 시각에 작업 자동 실행 |
| Docker / Container (컨테이너) | 앱+의존성을 밀봉해 어디서든 동일 실행 |
| API (Application Programming Interface) | 프로그램끼리 약속된 방식으로 소통하는 창구 |
| Port / localhost | 서비스 구분 번호와 '내 컴퓨터 자신' 주소 |
| Environment variable (환경변수/.env) | 코드 밖에서 설정·비밀키 주입 |

### 11. 버전 관리·협업 — [자세히](terms/11-version-control.md)
| 용어 | 한 줄 요약 |
|---|---|
| Git vs GitHub (깃/깃허브) | Git=이력관리 도구, GitHub=온라인 호스팅 장소 |
| Repository (repo) | 프로젝트 파일 + 변경 이력 저장 단위 |
| Commit (커밋) | 변경을 메시지와 함께 남기는 스냅샷 |
| Branch / Merge (브랜치·머지) | 본류 밖 갈래 작업 / 다시 합치기 |
| Clone / Pull / Push (클론·풀·푸시) | 원격과 동기화 3동작(복제·받기·올리기) |
| Fork (포크) | 남의 저장소를 내 계정으로 통째 복제 |
| Pull request (PR) | 변경을 제안·리뷰·병합 요청 |

### 12. 안전·거버넌스 — [자세히](terms/12-safety-governance.md)
| 용어 | 한 줄 요약 |
|---|---|
| Guardrails (가드레일) | 입출력 앞뒤에 세운 안전 울타리 |
| Red-teaming (레드팀) | 배포 전 일부러 공격해 약점 찾기 |
| Model card (모델 카드) | 모델 용도·성능·한계 설명서(성분표) |
| Watermarking (워터마킹) | AI 생성물에 사람 눈엔 안 보이는 표식 심기 |
| EU AI Act 위험등급 | EU AI 법의 4단계 위험 분류(+GPAI 별도) |

---

## 사용법
- 급하면 위 치트시트만. 깊이 알려면 카테고리 파일로.
- 각 항목: **정의(국·영) · 비유 · 언제 쓰나 · AI에게 하는 말 · 흔한 오해 · 출처**.

## 최신본 · 업데이트 받기

- **웹에서 읽는 경우 — 아무것도 안 해도 됩니다.** 지금 보는 이 페이지가 늘 최신본입니다.
- **`git clone`한 경우** — 자동으로 갱신되지 않습니다. `git pull`로 직접 당겨오세요.
- **fork한 경우** — 자동 갱신되지 않습니다. GitHub의 **Sync fork** 버튼을 쓰세요.
- **업데이트 알림을 받으려면** — 우측 상단 **Watch → Custom → Releases**를 켜두면 새 릴리스마다 알림이 옵니다.
- **무엇이 언제 바뀌었나** — [CHANGELOG.md](CHANGELOG.md)

## 기여 · 라이선스
- 기여 규칙(출처 필수 등): [CONTRIBUTING.md](CONTRIBUTING.md)
- 라이선스: [CC BY 4.0](LICENSE) — 출처 표기 시 자유 이용.

## 면책
정확성을 위해 1차 출처를 답았으나, 기술 용어의 정의·경계는 진화합니다. 중요한 판단에는 원 출처를 직접 확인하세요.
