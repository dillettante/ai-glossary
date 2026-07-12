# SOURCES.md — 골든소스 화이트리스트 (Phase 0 산출물)

> **용도:** 이 파일은 집필의 **단일 인용 원천**이다. Phase 2·3 집필 시 여기 등재된 출처에서만 정의를 인용한다. 여기 없는 주장·수치·논문 ID는 넣지 않는다.
> **검증:** 아래 arXiv ID·연도·제1저자·공식문서 URL은 5개 디스커버리 에이전트가 **실제로 페이지를 열어** 대조한 것. 미확정 항목은 "⚠️확인 필요"로 표시.
> **작성:** 2026-07-11 · 원자료: 5개 리서치 스트림 도시어(정의·비유·흔한 오해 포함, 세션 원본 보존).

---

## 확정 택소노미 (7 카테고리 · 약 40 용어)

| # | 카테고리 | 용어 |
|---|---|---|
| 1 | LLM 기초 | Token, Context window, Embedding, Hallucination, Inference, Parameter/Weight, Temperature |
| 2 | 프롬프트 & 상호작용 | Prompt, System prompt, Few-shot / In-context learning, Chain-of-thought |
| 3 | AI 서비스 구축 | RAG, MCP, Agent, Tool use / Function calling, Vector DB / Embedding search |
| 4 | 모델 커스터마이징 (PEFT·파인튜닝) | LoRA, QLoRA, Prefix Tuning, Adapter Tuning, Instruction Tuning, P-Tuning (+v2), BitFit, Soft Prompts / Prompt Tuning, Multi-Task FT, Federated FT |
| 5 | 정렬 · 강화학습 | RLHF, RLAIF, DPO, GRPO (+ 사례: OpenPipe ART) |
| 6 | 바이브코딩 워크플로우 | Vendoring, Porting, Refactoring, Scaffolding, Wrapping, Modularization |
| 7 | 개발 단계 · 품질 | PoC, MVP, Production Ready |
| 8 | 모델 형식·양자화·경량화 | GGUF, MLX, safetensors, Quantization, MoE, Distillation |
| 9 | 로컬 실행·셀프호스팅 | llama.cpp, Ollama, LM Studio, Self-hosting |
| 10 | 개발·운영 인프라 | SSH, CLI, cron, Docker/Container, API, Port/localhost, Environment variable |
| +2 | (프롬프트 추가) | Reasoning/Thinking mode, Context engineering |
| +3 | (구축 추가) | Embedding model, Chunking |
| +δ | (3차 확장·분산) | Prompt injection/Jailbreak(2), Evals(7), Multimodal(1), Knowledge cutoff(1), Reranking(3), Open weights vs Open source(9) |
| 11 | 버전 관리·협업 | Git vs GitHub, Repository, Commit, Branch/Merge, Clone/Pull/Push, Fork, Pull request |
| 12 | 안전·거버넌스 | Guardrails, Red-teaming, Model card, Watermarking, EU AI Act 위험등급 |
| +ε | (4차 확장·토대/분산) | LLM/Foundation model(1), Transformer/Attention(1), Generative AI(1), Pretraining(1), Diffusion model(1), Alignment(5), Overfitting(4), Structured output(3), Agent memory(3) |

---

## 카테고리 1 — LLM 기초

| 용어(원어/한글) | 검증 출처 | 한 줄 정의 | 흔한 오해 |
|---|---|---|---|
| **Token / 토큰** | Anthropic Glossary(platform.claude.com/docs) · Google Cloud GenAI glossary | 모델이 처리하는 최소 단위(단어·부분단어·문자·바이트). Claude 기준 1토큰≈영어 3.5자 | "토큰=단어" 아님. 한국어는 같은 뜻에 토큰을 더 씀 |
| **Context window / 컨텍스트 윈도우** | Anthropic Glossary · Google Cloud | 생성 시 되돌아볼 수 있는 텍스트 양 = 모델의 작업기억 | 학습 데이터량과 혼동. 창 밖 초반 대화는 자동 기억 안 됨 |
| **Embedding / 임베딩** | word2vec: Mikolov et al. **arXiv:1301.3781**(2013) · 정의보조 Google Cloud | 데이터를 의미관계를 담은 수치 벡터로 변환한 표현 | 임베딩=검색(RAG) 혼동. ⚠️word2vec을 대표로 인용하되 "용어의 유일 창시 논문"으로 단정 금지 |
| **Hallucination / 환각** | 서베이: Ji et al. **arXiv:2202.03629**(2022, ACM CSUR) · 정의보조 Google Cloud | 모델이 사실 아닌 내용을 생성하는 현상 | "거짓말"로 의인화 금지(확률적 부산물). 온도0으로도 제거 안 됨. ⚠️단일 창시논문 특정 불가(서베이=2차문헌) |
| **Inference / 추론(실행단계)** | Google ML Glossary(developers.google.com/machine-learning/glossary) | 학습 끝난 모델을 실제로 써서 입력→출력 생성하는 단계 | training과 혼동(추론 중 가중치 불변). reasoning(사고력)과 오역 주의 ⚠️개별페이지 verbatim 재확인 권장 |
| **Parameter·Weight / 파라미터·가중치** | Google Cloud · IBM(ibm.com/think/topics/llm-parameters) | 입력처리·출력을 결정하는 내부변수; 학습으로 조정되는 대표값이 weight·bias | "많을수록 항상 똑똑" 아님. 하이퍼파라미터(온도 등)와 혼동 |
| **Temperature / 온도** | Anthropic Glossary · Google Cloud | 생성 무작위성 조절값. 高=창의·다양, 低=보수·결정론 | 온도0=완전동일출력 아님(Anthropic 명시). 高가 "더 똑똑" 아님 |

## 카테고리 2 — 프롬프트 & 상호작용

| 용어(원어/한글) | 검증 출처 | 한 줄 정의 | 흔한 오해 |
|---|---|---|---|
| **Prompt / 프롬프트** | Google Cloud GenAI glossary · OpenAI prompting guide | 응답을 끌어내려 모델에 보내는 자연어 요청 | "짧을수록 좋다" 아님. 모델이 지시를 "이해"한다 과신 금지 |
| **System prompt / 시스템 프롬프트** | OpenAI prompting guide · Anthropic Glossary | 대화 앞단에서 모델의 역할·어조·경계를 규정하는 'system' 지시문 | 절대 방어벽 아님(프롬프트 인젝션). 토큰·컨텍스트 소모함. ⚠️"A system message is…" 한줄 verbatim 정의는 미확정(Azure 스니펫, 페이지 미열람) |
| **Few-shot / In-context learning / 퓨샷·인컨텍스트 러닝** | GPT-3: Brown et al. **arXiv:2005.14165**(2020, NeurIPS) · 보조 Google Cloud | 가중치 갱신 없이 프롬프트 안 소수 예시로 그 자리에서 과제 수행 | 재훈련 아님(가중치 불변, 그 대화만 유효). 예시 多가 항상 좋진 않음 |
| **Chain-of-thought / 생각의 사슬(CoT)** | Wei et al. **arXiv:2201.11903**(2022, NeurIPS) | 중간 추론단계 연쇄를 생성케 해 복잡추론 성능↑ | 출력 추론=진짜 내부사고 아님(사후합리화 가능). 작은 모델엔 이득 제한적 |

## 카테고리 3 — AI 서비스 구축

| 용어(원어/한글) | 검증 출처 | 한 줄 정의 | 흔한 오해 |
|---|---|---|---|
| **RAG / 검색 증강 생성** | Lewis et al. **arXiv:2005.11401**(2020, NeurIPS), 저자 12인 | 파라메트릭 지식 + 외부 문서 실시간 검색(비파라메트릭)을 결합해 생성 | 파인튜닝의 대체 아님(보완재). 문서 넣어도 검색품질 낮으면 환각 |
| **MCP / 모델 컨텍스트 프로토콜** | Anthropic 발표(2024-11-25) · modelcontextprotocol.io · 기부발표 Anthropic(2025-12-09) | AI 앱↔외부 데이터·도구를 잇는 개방형 표준(N×M 통합문제 해소) | Anthropic/Claude 전용 아님(2025-12 LF 산하 **Agentic AI Foundation** 기부, OpenAI·Block 공동창립). MCP=에이전트 아님(연결규격) |
| **Agent / AI 에이전트** | Anthropic "Building Effective Agents"(2024-12-19) | LLM이 자기 처리·도구사용을 동적으로 지휘하는 시스템 | 챗봇 아님. "항상 에이전트가 낫다" 아님(고정경로면 워크플로우가 나음). ⚠️한 회사 정의임을 명시 |
| **Tool use / Function calling / 도구 사용·함수 호출** | Anthropic tool use docs(platform.claude.com) | 모델이 필요 판단 시 정의된 도구를 스스로 호출(의도 JSON 산출, 실행은 앱) | 모델이 직접 실행 아님. 도구 多가 항상 좋진 않음(설명·스키마 품질이 좌우). ⚠️OpenAI 2023 계보 병기 권장 |
| **Vector DB / Embedding search / 벡터DB·임베딩 검색** | word2vec **arXiv:1301.3781** · HNSW: Malkov & Yashunin **arXiv:1603.09320**(2016) · 정의보조 Elastic/Google/IBM | 고차원 임베딩을 저장·색인, 질의벡터와 최근접(ANN)을 의미유사도순 반환 | 정확검색 아님(근사 ANN). 키워드검색 불필요 아님(하이브리드 권장). ⚠️벤더 중립 서술 |

## 카테고리 4 — 모델 커스터마이징 (PEFT·파인튜닝)

> 공통 축 = **무엇을 학습 vs 동결하는가**. Instruction Tuning·Multi-Task·Federated는 "어느 파라미터를 얼리나"가 아니라 "무엇을/어디서 학습하나"의 축이라 PEFT와 결이 다름(아래 주의 참조).

| 용어(원어/한글) | 검증 출처(arXiv, 전수 확인) | 학습/동결 | 흔한 오해 |
|---|---|---|---|
| **LoRA / 로라** | Hu et al. **2106.09685**(2021) | 저계수 행렬 A·B 학습 / 원본 가중치 전부 동결. 추론지연 없음(병합 가능) | prompt·prefix tuning과 혼동(LoRA는 가중치에 델타 가산) |
| **QLoRA / 큐로라** | Dettmers et al. **2305.14314**(2023) | LoRA 어댑터 학습 / 4-bit(NF4) 양자화 베이스 동결 | 새 알고리즘 아님(=양자화베이스+LoRA). 베이스를 4-bit로 "학습" 아님 |
| **Prefix Tuning / 프리픽스 튜닝** | Li & Liang **2101.00190**(2021) | 전 층 어텐션 prefix 벡터 학습 / LM 전부 동결 | Prompt tuning과 다름(prefix=전 층, prompt=입력층 1곳) |
| **Adapter Tuning / 어댑터 튜닝** | Houlsby et al. **1902.00751**(2019) | 층 사이 병목 모듈 학습 / 원본 전부 동결 | LoRA와 달리 **추론 지연 발생**(모듈 실제 삽입) |
| **Instruction Tuning / 인스트럭션 튜닝** | FLAN: Wei et al. **2109.01652**(2021) · T0: Sanh et al. **2110.08207**(2021) | **완전 미세조정 계열**(대개 전체 파라미터 갱신) | ⚠️PEFT 아님. "무엇을 학습하나(지시형 데이터)"의 문제지 동결의 문제가 아님 |
| **P-Tuning / 피튜닝** | Liu et al. "GPT Understands, Too" **2103.10385**(2021) | 연속 프롬프트 임베딩 학습 / LM은 frozen·tuned 선택 | v1과 v2 혼동 금지 |
| **P-Tuning v2** | Liu et al. **2110.07602**(2021) | 전 층 연속 프롬프트(deep prompt tuning) / LM 동결 | v1(입력층)과 별개 논문·별개 방법 |
| **BitFit / 비트핏** | Ben-Zaken et al. **2106.10199**(2021) | bias 항만 학습 / 모든 가중치 행렬 동결 | 새 파라미터 추가 아님(기존 bias만 갱신) |
| **Soft Prompts / Prompt Tuning** | Lester et al. **2104.08691**(2021) | 입력층 soft prompt만 학습 / LM 전부 동결. 규모 클수록 완전미세조정과 격차 소멸 | soft prompt=사람이 못 읽는 학습된 벡터(텍스트 프롬프트 아님) |
| **Multi-Task Fine-Tuning / 멀티태스크 미세조정** | 개괄: Ruder **1706.05098**(2017) · LLM맥락 T0/FLAN 병용 | 대개 공유 백본+태스크head 갱신(방법이 동결 규정 안 함) | ⚠️단일 창시논문 없음(개괄 채택). instruction tuning은 그 특수사례 |
| **Federated Fine-Tuning / 연합 미세조정** | FedAvg: McMahan et al. **1602.05629**(⚠️arXiv 2016 / AISTATS 2017) · LLM적용 FedIT: Zhang et al. **2305.05644**(2023) | 원본 데이터는 로컬 잔류, 로컬 업데이트만 취합(model averaging) | 그 자체가 PEFT 아님(FL×PEFT 직교, 흔히 결합). ⚠️개념창시+LLM적용 2건 병기 |

## 카테고리 5 — 정렬 · 강화학습

| 용어(원어/한글) | 검증 출처(arXiv, 전수 확인) | 한 줄 정의 / 차이축 | 흔한 오해 |
|---|---|---|---|
| **RLHF / 인간 피드백 기반 강화학습** | 기원 Christiano et al. **1706.03741**(2017) · LLM표준 InstructGPT: Ouyang et al. **2203.02155**(2022) | 인간 선호(순위)로 보상모델 학습→PPO로 미세조정 | 인간이 정답을 직접 쓰는 것 아님(선호 비교만 함) |
| **RLAIF / AI 피드백 기반 강화학습** | 개념 Constitutional AI: Bai et al. **2212.08073**(2022) · 명명·비교 Lee et al. **2309.00267**(2023) | 선호 라벨 주체를 인간→다른 LLM으로 교체 | 인간 완전배제 아님(원칙·헌법은 인간이 설계). ※사용자 원목록의 RLAIF 중복(#10/#13)은 1개로 통합 |
| **DPO / 직접 선호 최적화** | Rafailov et al. **2305.18290**(2023) | 보상모델·RL루프 없이 선호데이터→단순 분류손실로 최적정책 직접 유도 | **RL 아님**(보상모델도 롤아웃도 없음) |
| **GRPO / 그룹 상대 정책 최적화** | DeepSeekMath: Shao et al. **2402.03300**(2024) | PPO 변형, critic 없이 그룹 내 여러 출력의 상대우열로 어드밴티지 추정 | ⚠️원출처는 DeepSeek-R1 아님(2024 DeepSeekMath). R1은 후속 적용사례 |

### 사례 — OpenPipe ART (github.com/openpipe/art)
- **ART = Agent Reinforcement Trainer** · **Apache-2.0** · 핵심 알고리즘 **GRPO** 사용(위 §5와 직결). LLM 에이전트를 다단계 과제에서 RL 훈련.
- **RULER = Relative Universal LLM-Elicited Rewards**(풀네임 검증, openpipe.ai/blog/ruler): LLM-as-judge가 한 프롬프트의 여러 궤적을 나란히 상대순위→0~1 점수→GRPO 보상신호. **라벨·수작업 보상함수·인간피드백 없이** 개선(4과제 중 3개서 수작업보상 상회 주장).
- ⚠️ **사용자 언급 "2026 기사" 본문 미수신.** RULER 개념("자동 LLM채점 보상으로 수작업 보상설계 생략")과 부합하나, 그 기사가 실제 ART/RULER를 지목했는지·저자·매체·발행일은 **본문 없이 검증 불가**. 사례로 확정 인용 전 **기사 본문 확보 필수**.

## 카테고리 6 — 바이브코딩 워크플로우

> ⚠️ 대부분 단일 기원논문 없는 업계 은어. 정본 확실 3개(Refactoring=Fowler, Wrapper=GoF, 아래 MVP=Ries)만 강함. 나머지는 "대표 권위 레퍼런스 + 대중화 출처"로 앵커.

| 용어(원어/한글) | 검증 출처 | 한 줄 정의 | 흔한 오해 |
|---|---|---|---|
| **Vendoring / 벤더링** | Go 공식 모듈 레퍼런스(go.dev/ref/mod) | 외부 의존성 복사본을 프로젝트 안(`vendor/`)에 넣어 재현·독립성 고정 | 설치(install)와 다름(소스 복사본을 저장소에 커밋). ⚠️Go 전용 아님(PHP Composer 등도)—"Go가 대중화"로 |
| **Porting / 포팅** | ⚠️위키백과(Porting) — 최종본은 IEEE 용어집으로 격상 권장 | 다른 환경(OS·CPU·언어·FW)에서 돌게 고쳐 옮김 | 복붙실행 아님(동작 보존+환경차 코드수정) |
| **Refactoring / 리팩터링** | Martin Fowler, refactoring.com(정본) | 겉보기 동작 불변, 내부구조만 이해·수정 쉽게 개선 | 기능추가·버그수정 아님. observable behavior 바뀌면 리팩터링 아님 |
| **Scaffolding / 스캐폴딩** | Ruby on Rails 공식 가이드 | 기본 뼈대 파일·보일러플레이트를 명령 한 번에 자동생성 | 완성품 아님(출발용 뼈대). ⚠️Rails가 대중화(범용 개념) |
| **Wrapping / 래핑 (Adapter)** | GoF *Design Patterns*(1994), Adapter 별칭 "Wrapper" | 기존 코드를 껍데기로 감싸 다른 인터페이스처럼/쉽게 사용 | 원본 고치는 것 아님(바깥 껍데기만). ⚠️GoF 원서 페이지 미열람(refactoring.guru로 검증) |
| **Modularization / 모듈화** | ⚠️위키백과 — 정본 Parnas(1972) CACM 논문으로 격상 권장 | 큰 프로그램을 책임별 독립 모듈로 쪼개고 인터페이스로만 소통 | "파일 쪼개기" 아님(핵심=관심사 분리·경계) |

## 카테고리 7 — 개발 단계 · 품질

| 용어(원어/한글) | 검증 출처 | 한 줄 정의 | 흔한 오해 |
|---|---|---|---|
| **PoC / 개념 증명** | ⚠️단일 정본 없음(업계·학술 통용어). 위키백과 + 표준 정의로 서술 | 기술적 실현가능성만 빠르게 확인하는 일회용 검증실험 | 제품 첫 버전 아님(feasibility만 답). ❌에이전트가 준 `arXiv:2604.05835`는 **존재하지 않는 ID → 폐기** |
| **MVP / 최소 기능 제품** | Eric Ries *The Lean Startup*(startuplessonslearned.com) | 최소 노력으로 검증된 학습을 최대로 얻는 출시 가능한 최소 제품 | 조잡한 반쪽 제품 아님(작아도 실제 작동·가치). 초기 개념자 Frank Robinson 각주 권장 |
| **Production Ready / 프로덕션 레디** | Google SRE Book, Production Readiness Review(sre.google/sre-book) | 실제 트래픽 운영에 안전히 올릴 수 있는 상태(모니터링·장애대응·확장·보안) | "로컬에서 잘 돎"=프로덕션 레디 아님(관측성·복구·용량·보안 필요) |

---

## 카테고리 8 — 모델 형식·양자화·경량화 (2차 확장)

| 용어 | 검증 출처 | 한 줄 정의 | 흔한 오해 |
|---|---|---|---|
| **Quantization / 양자화** | LLM.int8(): Dettmers **arXiv:2208.07339**(2022) · GPTQ: Frantar **arXiv:2210.17323**(2022) | 가중치를 저비트(16→8/4bit)로 표현해 크기·메모리 압축 | 파인튜닝 아님; "무조건 성능 급락" 아님. ⚠️단일 창시 아님(대표 논문) |
| **GGUF** | ggml-org GGUF 명세 [ggml/docs/gguf.md] · llama.cpp | llama.cpp/GGML 생태계의 (주로 양자화) 모델 단일 파일 포맷 | 양자화 "방법"이 아니라 "그릇(포맷)". ⚠️원전 논문 없음 |
| **MLX** | Apple ml-explore [github.com/ml-explore/mlx] · ml-explore.github.io/mlx | 애플 실리콘용 NumPy 유사 ML 프레임워크(통합메모리·로컬) | 특정 모델/포맷 아님(프레임워크). ⚠️원전 논문 없음 |
| **safetensors** | Hugging Face [github.com/huggingface/safetensors] | 안전(코드실행 불가)·고속(≈제로카피, README 예: BLOOM 8GPU 로딩 10분→45초) 텐서 직렬화 포맷, pickle 대체 | 압축(양자화) 아님(직렬화). "완전 0복사"도 아님. ⚠️원전 논문 없음 |
| **MoE / 전문가 혼합** | Sparsely-Gated MoE: Shazeer **arXiv:1701.06538**(2017) · Switch: Fedus **arXiv:2101.03961**(2021) | 입력마다 게이팅이 고른 일부 전문가 서브넷만 활성화(희소 활성화) | "매 입력에 전체 파라미터 다 씀" 아님. ⚠️대표 출처(유일 창시 아님) |
| **Distillation / 증류** | Hinton, Vinyals & Dean **arXiv:1503.02531**(2015) | 큰 교사 모델 지식(soft targets)을 작은 학생 모델로 이전·압축 | 양자화와 다름(별개 작은 모델 생성) |

## 카테고리 9 — 로컬 실행·셀프호스팅 (2차 확장)

| 용어 | 검증 출처 | 한 줄 정의 | 흔한 오해 |
|---|---|---|---|
| **llama.cpp** | [github.com/ggml-org/llama.cpp] | C/C++ 경량 LLM 추론 엔진(CPU·GPU, GGUF 사용, GGML 기반) | 학습 도구 아님(추론); 유일 엔진 아님. ⚠️원전 논문 없음 |
| **Ollama** | [github.com/ollama/ollama]·ollama.com (MIT) | 로컬에서 오픈 모델을 손쉽게 받아 실행하는 도구(llama.cpp 백엔드) | 밑바닥부터의 엔진 아님(래핑). ⚠️원전 논문 없음 |
| **LM Studio** | [lmstudio.ai]·lmstudio.ai/docs | 로컬 LLM 실행·서버 GUI 앱(엔진 llama.cpp+MLX, OpenAI 호환 API) | "무료≠오픈소스"(앱은 독점무료); OpenAI 호환=응답 형식만. ⚠️제품 |
| **Self-hosting / 셀프호스팅** | Wikipedia *Self-hosting (web services)* | 클라우드 대신 내 서버/PC에서 직접 운영 | 자동으로 싸거나 쉬운 것 아님(운영·보안 부담). ⚠️단일 정본 없음 |

## 카테고리 10 — 개발·운영 인프라 (2차 확장)

| 용어 | 검증 출처 | 한 줄 정의 | 흔한 오해 |
|---|---|---|---|
| **SSH** | **RFC 4251** *SSH Protocol Architecture*(2006) | 신뢰 못 할 망 위에서 원격 로그인·서비스를 안전하게(인증·암호화) | 파일전송 도구 아님(scp/sftp가 위에 얹힘); 개인키=비밀번호 취급 |
| **CLI / 커맨드라인** | POSIX *Shell Command Language* IEEE Std 1003.1-2017 | GUI 대신 텍스트 명령으로 프로그램·OS 조작 | "전문가 전용·더 위험"은 절반만 맞음. ⚠️단일 정본 없음 |
| **cron / 크론** | POSIX `crontab` IEEE Std 1003.1-2017 · man cron | 지정 시각·주기에 명령 자동 반복 실행(5칸) | 꺼진 동안 못 돈 작업 몰아 실행 안 함; cron 실행환경(PATH) 다름 |
| **Docker/Container / 컨테이너** | docs.docker.com · OCI Runtime/Image Spec | 앱+의존성을 격리 단위로 묶어 호스트 무관 동일 실행 | VM과 다름(커널 공유→경량); Docker=컨테이너 아님(OCI 표준·Podman 등) |
| **API** | 일반 개념 · REST: Fielding(2000) 5장 · HTTP: **RFC 9110**(2022) | 프로그램끼리 정해진 규칙으로 기능·데이터를 요청·수신하는 창구 | 웹 API만 API 아님(함수규격도 API). ⚠️단일 창시 아님 |
| **Port / localhost** | 포트 **RFC 6335**(2011) · localhost **RFC 6761**(2013) · 루프백 **RFC 1122** §3.2.1.3 | 포트=서비스 구분번호(0–65535); localhost=자기자신(127.0.0.1, 루프백) | localhost는 기본 내 기기서만 접근; 1024 미만은 관리자 권한 |
| **Environment variable / 환경변수(.env)** | POSIX Base Defs ch.8 IEEE Std 1003.1-2017 · Twelve-Factor III(12factor.net) | 코드 밖에서 주입되는 이름–값 설정(환경마다 코드 수정 없이 교체) | **비밀키는 코드·Git·.env 커밋 금지**; .env는 관례이지 표준 아님. ⚠️단일 정본 없음 |

## 카테고리 2·3 추가 (분산 편입)

| 용어(카테고리) | 검증 출처 | 한 줄 정의 | 흔한 오해 |
|---|---|---|---|
| **Reasoning/Thinking mode (2)** | OpenAI *Reasoning models* docs · Anthropic *Extended thinking* docs | 답 전에 내부적으로 더 길게 '생각'하는 내장 모드 | CoT(프롬프트 기법)와 다름(내장 기능). ⚠️벤더별 구현·유일 창시 아님 |
| **Context engineering (2)** | Anthropic *Effective context engineering for AI agents* | 컨텍스트 윈도우에 무엇을 어떻게 채울지 설계(프롬프트 넘어) | 프롬프트 엔지니어링의 확장. ⚠️신생·단일 정본 없음 |
| **Embedding model (3)** | Sentence-BERT: Reimers & Gurevych **arXiv:1908.10084**(2019) | 텍스트를 임베딩 벡터로 변환하는 전용 모델(생성 LLM과 구별) | 생성 LLM이 다 하지 않음(전용 모델); 다른 모델 벡터 섞기 금지 |
| **Chunking (3)** | Pinecone *Chunking Strategies* 등 실무 가이드 | 긴 문서를 검색·임베딩용 작은 조각으로 나눔(RAG 전처리) | ⚠️단일 정본 없음(RAG 실무 개념) |

## 3차 확장 — 개념 6종 (분산 편입)

| 용어(카테고리) | 검증 출처 | 한 줄 정의 | 흔한 오해 |
|---|---|---|---|
| **Prompt injection / Jailbreak (2)** | Willison(2022) 명명 · OWASP *Top 10 for LLM* LLM01 · Wei et al. **arXiv:2307.02483**(2023) | injection=신뢰 못 할 입력의 숨은 지시가 모델을 탈취 / jailbreak=안전장치 우회 | 둘은 다름(injection=출처 탈취, jailbreak=정책 우회); 시스템 프롬프트가 방어벽 아님 |
| **Evals / 평가·벤치마크 (7)** | MMLU: Hendrycks **arXiv:2009.03300**(2020) · HELM: Liang **arXiv:2211.09110**(2022) | 정해진 과제·지표로 성능 측정(표준 묶음=벤치마크) | 벤치마크 점수≠실사용(오염·과적합·도메인 불일치). ⚠️단일 정본 없음 |
| **Multimodal / 멀티모달 (1)** | CLIP: Radford **arXiv:2103.00020**(2021) | 텍스트+이미지·음성 등 여러 모달리티를 함께 처리 | "진짜로 본다" 아님(환각 가능)·모달별 편차. ⚠️대표 사례(유일 창시 아님) |
| **Knowledge cutoff / 지식 컷오프 (1)** | 제공사 모델 문서(OpenAI/Anthropic model cards의 training/knowledge cutoff) | 학습 데이터가 특정 시점까지만→이후는 도구 없이 모름 | 실시간 인터넷 연결 아님; 이후 정보엔 RAG/검색 필요. (날짜 아닌 개념 인용→노후화 방지) |
| **Reranking / 리랭킹 (3)** | Nogueira & Cho **arXiv:1901.04085**(2019) · Cohere Rerank docs | 1차 검색 후보를 정밀 모델(cross-encoder)로 재정렬하는 RAG 2단계 | 검색 대체 아님(위에 얹는 2단계); 느림→소수 후보에만 |
| **Open weights vs Open source (9)** | OSI *Open Source AI Definition* v1.0(2024) opensource.org/ai | 가중치 공개≠오픈소스; 사용·재배포 범위는 라이선스가 정함 | "무료=마음대로" 아님; "open"이 곧 OSI 오픈소스 아님. Llama는 논쟁 사례로만(판정 아님) |

## 카테고리 11 — 버전 관리·협업

| 용어 | 검증 출처 | 한 줄 정의 | 흔한 오해 |
|---|---|---|---|
| **Git vs GitHub** | *Pro Git*(git-scm.com/book) · git-scm.com/docs · MS 인수(2018-06-04) | Git=분산 버전관리 도구(Torvalds 2005, 리눅스 커널용); GitHub=git 저장소 호스팅·협업 서비스(MS 소유) | Git≠GitHub; git은 로컬 완결·GitHub은 호스팅 다수 중 하나(GitLab·Bitbucket) |
| **Repository (repo)** | *Pro Git* "Getting a Git Repository" | 프로젝트 파일+전체 변경 이력을 담는 저장 단위 | "현재 폴더"만 아님(이력 포함); 온라인 전용 아님 |
| **Commit** | *Pro Git* "Recording Changes" · git-scm.com/docs/git-commit | 저장소 상태를 메시지와 함께 남기는 스냅샷 | 커밋만으론 공유 안 됨(push 필요) |
| **Branch / Merge** | *Pro Git* "Branches in a Nutshell"·"Basic Branching and Merging" | 브랜치=본류 밖 독립 작업 갈래; 머지=본류에 합침 | 브랜치=복사폴더 아님; 머지 자동 아님(충돌은 사람이) |
| **Clone / Pull / Push** | git-scm.com/docs/git-{clone,pull,push} · *Pro Git* "Working with Remotes" | 클론=최초 복제, 풀=원격 변경 받기, 푸시=내 커밋 올리기 | 풀↔푸시 방향 반대; 커밋만으론 전달 안 됨 |
| **Fork** | GitHub Docs *About forks* | 남의 저장소를 내 계정으로 통째 복제해 독립 갈래 | ⚠️core git 명령 아님(플랫폼 기능); branch와 다름(별개 저장소) |
| **Pull request (PR)** | GitHub Docs *About pull requests* · GitLab *Merge requests* | 내 변경을 원본에 합쳐달라 제안·리뷰·병합하는 절차 | ⚠️core git 아님(플랫폼 기능); GitLab=merge request; `git pull`과 별개 |

## 카테고리 12 — 안전·거버넌스

| 용어 | 검증 출처 | 한 줄 정의 | 흔한 오해 |
|---|---|---|---|
| **Guardrails / 가드레일** | NVIDIA NeMo Guardrails docs·github | 모델 입출력을 규칙·필터로 통제하는 바깥 안전 통제층 | 모델 자체를 안전하게 만드는 게 아님(바깥 통제층); 우회 가능. ⚠️단일 정본 없음 |
| **Red-teaming / 레드팀** | OpenAI·Anthropic red-teaming 문서 | 배포 전 일부러 공격·유도해 안전 취약점을 찾는 적대적 시험 | 합격 도장 아님·반복 과정; 취약점 부재 증명 못 함. ⚠️단일 정본 없음 |
| **Model card / 모델 카드** | Mitchell et al. **arXiv:1810.03993**(2019) | 모델 용도·성능·한계·평가·윤리를 표준 서식으로 문서화(투명성) | 성능 자랑 아님 — 한계·부적합 용도 명시가 핵심 |
| **Watermarking / 워터마킹** | Kirchenbauer et al. **arXiv:2301.10226**(2023) | AI 생성물 식별 위해 출력에 통계적 신호를 삽입 | 재작성·번역·편집으로 제거·우회 가능; 표식 없음≠비AI. ⚠️대표 방법 |
| **EU AI Act 위험등급** | Regulation (EU) **2024/1689**(EUR-Lex CELEX:32024R1689)·EU집행위 요약 | AI를 위험도 4단계(금지/고위험/투명성/최소)+GPAI로 분류 | 금지(Art.5)≠고위험(Art.6·Annex III); 투명성=Art.50; GPAI=Ch.V(분류 Art.51·기본의무 Art.53·systemic Art.55). 조문 축자 확인 완료(2026-07-12) |

## 4차 확장 — 토대·분산 (9종)

| 용어(카테고리) | 검증 출처 | 한 줄 정의 | 흔한 오해 |
|---|---|---|---|
| **LLM / Foundation model (1)** | Foundation model 명명: Bommasani **arXiv:2108.07258**(2021, Stanford CRFM); LLM=우산용어 | 방대한 텍스트로 학습해 언어 이해·생성하는 대형 신경망 / 다양한 과제에 적응 가능한 범용 사전학습 모델 | LLM≠Foundation model(후자 더 넓음); LLM≠정답 DB. ⚠️LLM 단일 정본 없음 |
| **Transformer / Attention (1)** | Vaswani et al. **arXiv:1706.03762**(2017) | 어텐션으로 문맥을 파악하는, 현대 거의 모든 LLM의 뼈대 아키텍처 | 어텐션≠사람의 집중력(수학적 가중치) |
| **Generative AI (1)** | Google Cloud·IBM 문서(우산 용어) | 학습 패턴으로 새 콘텐츠(텍스트·이미지·코드 등)를 생성하는 AI 총칭 | GenAI≠LLM(더 넓음). ⚠️단일 창시 없음 |
| **Pretraining / 사전학습 (1)** | BERT: Devlin **arXiv:1810.04805**(2018) / GPT 계열이 대중화 | 대규모 데이터로 기본 언어·표현 능력을 먼저 학습(이후 파인튜닝·정렬) | 사전학습≠파인튜닝. ⚠️대표 계보(단일 창안 아님) |
| **Diffusion model / 디퓨전 (1)** | Ho et al. **arXiv:2006.11239**(2020); 원류 Sohl-Dickstein **arXiv:1503.03585**(2015) | 노이즈를 점진 제거해 이미지·영상을 생성(미드저니·SD·Sora류) | LLM(텍스트)과 다른 생성 방식 |
| **Alignment / 정렬 (5)** | Wikipedia *AI alignment* · Anthropic(HHH) | AI를 인간 의도·가치·선호에 맞게 행동시키는 것(RLHF·RLAIF·DPO·GRPO가 그 방법) | 정렬≠성능(별개 축). ⚠️단일 정본 없음 |
| **Overfitting / 과적합 (4)** | Google *ML Crash Course — Overfitting* · Wikipedia | 학습 데이터에 과하게 맞춰져 새 데이터·실제서 성능이 떨어지는 현상 | 벤치마크 과적합→[Evals] 연결. ⚠️단일 창시 없음 |
| **Structured output / JSON mode (3)** | OpenAI *Structured Outputs* · Anthropic 문서 | 모델이 정해진 스키마(JSON)에 맞춰 답하도록 강제하는 기능 | 구조화출력(스키마 보장)≠단순 JSON mode. ⚠️벤더별 구현 |
| **Agent memory / 에이전트 메모리 (3)** | LangChain LangMem · Anthropic Agent memory 문서 | 세션을 넘어 정보를 저장·회상해 맥락 유지(단기=컨텍스트, 장기=외부저장) | ⚠️프레임워크·제공사별, 단일 정본 없음 |

## 정확성 결정사항 (집필 시 반드시 반영)

1. **폐기:** PoC 근거 `arXiv:2604.05835`는 존재하지 않는 ID 형식 → 인용 금지. PoC는 업계·학술 통용 정의로만 서술.
2. **연도 병기:** FedAvg(McMahan) = "arXiv 2016 / AISTATS 2017"로 병기(흔한 연도 오인).
3. **위키백과 앵커 격상(bible 품질):** Porting→IEEE 용어집, Modularization→Parnas(1972), PoC→학술 정의 확보 시 교체.
4. **단일 창시논문 없음 명시:** Embedding·Hallucination·Agent·Tool use·Vector DB·Multi-Task·Federated·PoC·Production Ready·MVP(개념자 구분) — "특정 벤더/논문 출처이며 유일 창시가 아님"을 본문에 명시(로펌 명의 중립성).
5. **축이 다른 항목 구분:** Instruction Tuning·Multi-Task·Federated는 PEFT(동결) 축이 아니라 "무엇을/어디서 학습" 축 — 카테고리 4 안에서 별도 주석.
6. **RLAIF 중복 제거:** 사용자 원목록 #10/#13 → 1개 항목.
7. **DPO는 RL 아님 / GRPO 원출처는 DeepSeekMath(R1 아님):** 오해 방지 문구 필수.
8. **미확정 verbatim 재확인:** System prompt 한줄 정의, Inference 개별페이지 — Phase 2 착수 시 1회 재fetch.
9. **⚠️블로커:** OpenPipe ART 사례(카테고리 5)는 사용자의 2026 기사 본문 수신 전 확정 집필 보류.

## 다음 단계 (Phase 1 진입 조건)
- [ ] 이 SOURCES.md 검토·승인
- [ ] 파인튜닝 기사 본문 수신 → §5 ART 사례 확정
- [ ] 위키백과 앵커 격상 여부 결정(격상 vs 현행 유지)
- [ ] 리포명·라이선스 확정(제안: `dillettante/ai-glossary`, CC BY 4.0)
