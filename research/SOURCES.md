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
