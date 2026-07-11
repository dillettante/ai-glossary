# 3. AI 서비스 구축 · Building with AI

> AI를 실제 서비스로 엮을 때 만나는 부품들. 검색을 붙이고(RAG), 도구를 연결하고(MCP·Tool use), 스스로 일하게(Agent) 만든다.
> 출처 근거: [research/SOURCES.md](../research/SOURCES.md) 카테고리 3. 서식: [STYLE.md](../STYLE.md).

---

### RAG · 검색 증강 생성 (Retrieval-Augmented Generation)

> **한 줄 요약:** AI가 답하기 전에 외부 문서를 검색해, 그 내용을 근거로 답을 만들게 하는 방식.

**정의 (Definition)**
- KO: 사전학습 모델의 내부 지식(파라메트릭 메모리)에, 외부 문서를 실시간 검색해 가져온 지식(비파라메트릭 메모리)을 결합해 답을 생성하는 방식.
- EN: Combining a model's parametric memory with non-parametric memory retrieved from an external corpus at generation time.

**비유 (쉽게):** 닫힌 책 시험(외운 것만으로 답)이 아니라 **오픈북 시험**. 답을 쓰기 전에 관련 페이지를 먼저 찾아 펼쳐 보고, 그 내용을 근거로 답을 쓴다.

**왜 중요한가 / 언제 쓰나:**
- 사내 문서·최신 정보·특정 지식을 모델에 주입하고, **답변에 출처를 달 수 있다**.
- 지식이 바뀌면 문서만 갈아 끼우면 되므로 갱신이 쉽다.
- 원 논문의 문제의식 자체가 "파인튜닝이 잘 못 푸는 지식 접근·출처 제시·갱신" 문제였다.

**실무 예시 / AI에게 이렇게 말한다:**
- "사내 규정 문서를 벡터DB에 넣고 RAG로 답하게 해줘."
- "답변에 근거가 된 문서 출처를 함께 달아줘."

**흔한 오해:**
- **"RAG = 파인튜닝의 대체"** — 아니다. 파인튜닝은 모델의 *행동·문체·능력*을 바꾸고, RAG는 *최신·특정 지식*을 주입한다. 상호 배타가 아니라 보완재.
- **"문서만 넣으면 환각이 사라진다"** — 검색 품질이 낮으면(엉뚱한 문서 회수) 오히려 그럴듯한 오답을 만든다. RAG는 검색 정확도에 종속된다.

**함께 보기:** [Embedding](01-llm-basics.md), [Vector DB / Embedding search](03-building.md), [Hallucination](01-llm-basics.md), [Agent](03-building.md)

**출처:** Lewis et al. (2020), *Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks*, [arXiv:2005.11401](https://arxiv.org/abs/2005.11401) (NeurIPS 2020).

---

### MCP · 모델 컨텍스트 프로토콜 (Model Context Protocol)

> **한 줄 요약:** AI 앱을 외부 데이터·도구에 연결하는 개방형 표준. 표준 단자 하나로 무엇이든 꽂는다.

**정의 (Definition)**
- KO: AI 애플리케이션과 외부 데이터·도구를 잇는 개방형 표준. N개 앱을 M개 도구에 각각 붙이던 N×M 통합 문제를, 표준 규격 하나로 대체한다.
- EN: An open standard that connects AI applications to external data sources and tools through a single, shared interface.

**비유 (쉽게):** **AI를 위한 USB-C 포트.** 예전엔 기기마다 다른 케이블이 필요했지만, 표준 단자 하나면 어떤 기기든 그대로 꽂힌다.

**왜 중요한가 / 언제 쓰나:**
- 도구마다 따로 만들던 연결을 표준 하나로 대체 → N×M 통합 폭발을 해소한다.
- 개방형 표준이라 특정 벤더 도구에 종속되지 않는다.

**실무 예시 / AI에게 이렇게 말한다:**
- "MCP 서버로 사내 문서 저장소를 Claude에 연결해줘."
- "이 데이터베이스를 MCP로 붙여서 질의할 수 있게 해줘."

**흔한 오해:**
- **"Anthropic/Claude 전용이다"** — 아니다. 2024-11 Anthropic이 처음 공개했지만 **개방형 표준**이며, 2025-12 Linux Foundation 산하 **Agentic AI Foundation(AAIF)**에 기부되어(OpenAI·Block 공동 창립) 벤더 중립 거버넌스로 이관됐다.
- **"MCP = 에이전트다"** — 아니다. MCP는 앱과 도구를 잇는 **연결 규격(프로토콜)**일 뿐, 스스로 판단·행동하는 주체가 아니다.

**함께 보기:** [Agent](03-building.md), [Tool use / Function calling](03-building.md), [RAG](03-building.md)

**출처:** Anthropic, *Introducing the Model Context Protocol* (2024-11-25), [modelcontextprotocol.io](https://modelcontextprotocol.io). 기부 사실: Anthropic, *Donating the Model Context Protocol…* (2025-12-09), [anthropic.com](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation).

---

### Agent · 에이전트 (AI Agent)

> **한 줄 요약:** 목표만 주면 스스로 필요한 도구를 골라, 여러 단계를 거쳐 일을 완수하는 시스템.

**정의 (Definition)**
- KO: LLM이 자신의 처리 과정과 도구 사용을 동적으로 지휘하며, 과제를 어떻게 수행할지 스스로 결정하고 통제하는 시스템.
- EN: A system where an LLM dynamically directs its own processes and tool usage, maintaining control over how it accomplishes tasks.

**비유 (쉽게):** 레시피를 한 줄씩 그대로 따라 하는 사람(=워크플로우)이 아니라, **"손님을 만족시켜라"**라는 목표만 받고 냉장고를 열어 재료를 확인하고 메뉴를 스스로 정하는 요리사.

**왜 중요한가 / 언제 쓰나:**
- 경로를 미리 다 짤 수 없는, 열린 문제에 쓴다.
- 필요한 단계 수·도구를 사전에 못 박기 어려울 때 유용하다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 버그를 에이전트로 조사해서 원인을 찾고 고쳐줘."

**흔한 오해:**
- **"챗봇이다"** — 아니다. 단순 대화 응답이 아니라, 도구를 써서 스스로 행동을 이어간다.
- **"항상 에이전트가 낫다"** — 아니다. 경로가 고정된 일은 예측 가능한 **워크플로우**가 더 싸고 안정적이다.
- **단일 표준 정의 없음** — 아래 정의는 Anthropic(한 회사)의 정의이며, 업계 공통 표준 정의가 아니다.

**함께 보기:** [MCP](03-building.md), [Tool use / Function calling](03-building.md), [RAG](03-building.md)

**출처:** Anthropic, *Building Effective Agents* (2024-12-19), [anthropic.com](https://www.anthropic.com). (특정 벤더 정의 — 유일 창시 아님)

---

### Tool use / Function calling · 도구 사용 · 함수 호출

> **한 줄 요약:** 모델이 필요하다고 판단하면 계산기·검색 같은 외부 도구를 스스로 불러 쓰게 하는 기능.

**정의 (Definition)**
- KO: 모델이 스스로 판단해 미리 정의된 도구를 호출하는 기능. 모델은 "이 도구를 이렇게 쓰겠다"는 호출 의도(JSON)만 내고, 실제 실행은 앱이 한다.
- EN: The model decides when to invoke predefined tools and emits a structured (JSON) call; the application executes it and returns the result.

**비유 (쉽게):** 암산하다 막히면 **"잠깐 계산기 좀 쓸게"** 하고 꺼내 쓰는 것. 단, 계산기를 실제로 두드리는 건 모델이 아니라 옆에 있는 앱이다.

**왜 중요한가 / 언제 쓰나:**
- 모델이 혼자 못 하는 일(실시간 검색·계산·DB 조회·외부 API 호출)을 도구로 메운다.
- 에이전트·MCP가 작동하는 기본 동작 원리다.

**실무 예시 / AI에게 이렇게 말한다:**
- "날씨 API를 도구로 등록하고, 필요할 때 호출해서 답하게 해줘."

**흔한 오해:**
- **"모델이 직접 실행한다"** — 아니다. 모델은 호출 의도(JSON)만 내고, 실제 실행·결과 반환은 애플리케이션 몫이다.
- **"도구가 많을수록 좋다"** — 아니다. 도구의 설명과 스키마 품질이 성패를 가른다.
- **단일 표준 정의 없음** — 아래는 Anthropic 문서 기준이며, OpenAI의 2023년 function calling 등 벤더별 구현이 병존한다.

**함께 보기:** [Agent](03-building.md), [MCP](03-building.md), [RAG](03-building.md)

**출처:** Anthropic, *Tool use (function calling)* 문서, [platform.claude.com](https://platform.claude.com). (OpenAI function calling 2023 등 병존 — 유일 창시 아님)

---

### Vector DB / Embedding search · 벡터DB · 임베딩 검색

> **한 줄 요약:** 뜻이 비슷한 데이터끼리 가까이 저장해두고, 질문과 의미가 가까운 것을 근처에서 빠르게 찾아주는 검색.

**정의 (Definition)**
- KO: 데이터를 고차원 임베딩 벡터로 저장·색인하고, 질의 벡터와 가장 가까운(최근접) 항목을 의미 유사도 순으로 돌려주는 저장소·검색 방식. 대규모에서는 정확 최근접 대신 근사 최근접(ANN)으로 찾는다.
- EN: Stores and indexes high-dimensional embeddings and returns the nearest neighbors to a query vector by semantic similarity (approximate nearest neighbor at scale).

**비유 (쉽게):** 도서관을 **내용이 비슷한 책끼리 가까운 서가에 꽂아둔 것.** "이거랑 비슷한 거 줘" 하면 근처 서가에서 꺼내준다.

**왜 중요한가 / 언제 쓰나:**
- RAG의 검색 엔진 역할 — 문서를 임베딩해 넣고, 질문과 뜻이 가까운 조각을 회수한다.
- 키워드가 겹치지 않아도 의미가 비슷하면 찾아낸다.

**실무 예시 / AI에게 이렇게 말한다:**
- "문서를 임베딩해서 벡터DB에 넣고, 질문과 유사한 문단을 찾아줘."

**흔한 오해:**
- **"정확 검색이다"** — 아니다. 대규모에선 근사(ANN, 예: HNSW)로 찾으므로 100% 정확한 최근접을 보장하지 않는다(속도와의 맞바꿈).
- **"키워드 검색이 필요 없어진다"** — 아니다. 의미 검색과 키워드 검색을 섞은 **하이브리드**가 흔히 더 낫다.
- **단일 표준 정의 없음** — 특정 벤더에 묶이지 않는 개념. 아래는 대표 논문 출처이며 유일 창시가 아니다.

**함께 보기:** [Embedding](01-llm-basics.md), [RAG](03-building.md)

**출처:** word2vec: Mikolov et al. (2013), [arXiv:1301.3781](https://arxiv.org/abs/1301.3781) · HNSW: Malkov & Yashunin (2016), [arXiv:1603.09320](https://arxiv.org/abs/1603.09320). 정의 보조: Elastic/Google/IBM. (벤더 중립 — 유일 창시 아님)
