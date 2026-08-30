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

**함께 보기:** [Embedding](01-llm-basics.md#embedding--임베딩), [Vector DB / Embedding search](03-building.md#vector-db--embedding-search--벡터db--임베딩-검색), [Hallucination](01-llm-basics.md#hallucination--환각할루시네이션), [Agent](03-building.md#agent--에이전트-ai-agent)

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

**함께 보기:** [Agent](03-building.md#agent--에이전트-ai-agent), [Tool use / Function calling](03-building.md#tool-use--function-calling--도구-사용--함수-호출), [RAG](03-building.md#rag--검색-증강-생성-retrieval-augmented-generation)

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

**함께 보기:** [MCP](03-building.md#mcp--모델-컨텍스트-프로토콜-model-context-protocol), [Tool use / Function calling](03-building.md#tool-use--function-calling--도구-사용--함수-호출), [RAG](03-building.md#rag--검색-증강-생성-retrieval-augmented-generation)

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

**함께 보기:** [Agent](03-building.md#agent--에이전트-ai-agent), [MCP](03-building.md#mcp--모델-컨텍스트-프로토콜-model-context-protocol), [RAG](03-building.md#rag--검색-증강-생성-retrieval-augmented-generation)

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
- **대표 제품:** Qdrant · Pinecone · Chroma · Weaviate · Milvus · pgvector · FAISS 등 — 개념은 같고 제품을 골라 쓴다(예시이며 특정 제품 추천이 아님).

**실무 예시 / AI에게 이렇게 말한다:**
- "문서를 임베딩해서 벡터DB에 넣고, 질문과 유사한 문단을 찾아줘."

**흔한 오해:**
- **"정확 검색이다"** — 아니다. 대규모에선 근사(ANN, 예: HNSW)로 찾으므로 100% 정확한 최근접을 보장하지 않는다(속도와의 맞바꿈).
- **"키워드 검색이 필요 없어진다"** — 아니다. 의미 검색과 키워드 검색을 섞은 **하이브리드**가 흔히 더 낫다.
- **단일 표준 정의 없음** — 특정 벤더에 묶이지 않는 개념. 아래는 대표 논문 출처이며 유일 창시가 아니다.

**함께 보기:** [Embedding](01-llm-basics.md#embedding--임베딩), [RAG](03-building.md#rag--검색-증강-생성-retrieval-augmented-generation)

**출처:** word2vec: Mikolov et al. (2013), [arXiv:1301.3781](https://arxiv.org/abs/1301.3781) · HNSW: Malkov & Yashunin (2016), [arXiv:1603.09320](https://arxiv.org/abs/1603.09320). 정의 보조: Elastic/Google/IBM. (벤더 중립 — 유일 창시 아님)

---

### Embedding model · 임베딩 모델

> **한 줄 요약:** 문장·문서 같은 데이터를 '의미 좌표'인 임베딩 벡터로 바꿔주는 전용 모델. 답을 쓰는 생성 모델과는 역할이 다르다.

**정의 (Definition)**
- KO: 텍스트(또는 이미지 등)를 의미를 담은 고정 크기의 임베딩 벡터로 변환하도록 특화된 모델. 문장을 생성하는 LLM이 아니라, 유사도 비교·검색에 쓸 벡터를 만드는 것이 목적이다.
- EN: A model specialized to convert text (or other data) into fixed-size embedding vectors that capture meaning — distinct from a generative LLM, its output is a vector for similarity and search, not text.

**비유 (쉽게):** 글의 뜻을 읽고 **지도 위 좌표 하나를 찍어주는 측량사.** 뜻이 비슷한 문장은 가까운 좌표에, 다른 문장은 먼 좌표에 찍힌다. 측량사는 글을 새로 쓰지 않는다 — 위치만 매긴다.

**왜 중요한가 / 언제 쓰나:**
- RAG·의미검색의 **입구**다. 문서와 질문을 같은 임베딩 모델로 벡터화해야 서로 거리를 잴 수 있다.
- 검색 품질이 임베딩 모델 성능에 직접 달려 있다 — 뜻을 잘 못 담으면 엉뚱한 문서가 회수된다.

**실무 예시 / AI에게 이렇게 말한다:**
- "문서와 질문을 같은 임베딩 모델로 벡터화한 뒤 벡터DB에 넣어줘."
- "한국어 성능이 좋은 임베딩 모델을 골라 문단 단위로 임베딩해줘."

**흔한 오해:**
- **"GPT 같은 생성 모델이 임베딩도 다 한다"** — 겹치기도 하지만, 임베딩은 보통 **별도의 전용 모델**(또는 전용 엔드포인트)로 만든다. 생성용 LLM과 임베딩 모델은 목적·출력이 다르다(하나는 텍스트, 하나는 벡터).
- **"임베딩 모델이 다르면 벡터를 섞어 써도 된다"** — 아니다. 모델마다 좌표계(벡터 공간)가 달라, 문서와 질문은 **같은 모델**로 임베딩해야 비교가 성립한다.

**함께 보기:** [Embedding](01-llm-basics.md#embedding--임베딩), [Vector DB / Embedding search](03-building.md#vector-db--embedding-search--벡터db--임베딩-검색), [RAG](03-building.md#rag--검색-증강-생성-retrieval-augmented-generation), [Chunking](03-building.md#chunking--청킹)

**출처:** Reimers & Gurevych (2019), *Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks*, [arXiv:1908.10084](https://arxiv.org/abs/1908.10084) (EMNLP 2019). 보조: OpenAI, *Embeddings* 문서, [platform.openai.com](https://platform.openai.com/docs/guides/embeddings). (대표 문헌 — 유일 창시 아님)

---

### Chunking · 청킹

> **한 줄 요약:** 긴 문서를 검색·임베딩하기 좋게 작은 조각으로 잘라두는 전처리 작업.

**정의 (Definition)**
- KO: 긴 텍스트를 임베딩·검색에 알맞은 작은 조각(chunk)으로 나누는 전처리. RAG에서 벡터DB에 넣기 전에 거치는 필수 단계다.
- EN: The preprocessing step of breaking long text into smaller segments (chunks) suited to embedding and retrieval — a standard step before indexing in RAG.

**비유 (쉽게):** 두꺼운 책을 통째로 복사기에 넣는 대신 **한 장씩 뜯어 정리해 두는 것.** 나중에 필요한 대목을 찾을 때, 책 전체가 아니라 딱 그 한 장만 빠르게 꺼낼 수 있다.

**왜 중요한가 / 언제 쓰나:**
- 임베딩 모델은 한 번에 넣을 수 있는 토큰(컨텍스트) 한도가 있어, 긴 문서는 잘라야 벡터로 만들 수 있다.
- 조각 크기가 검색 품질을 좌우한다 — 너무 크면 잡음이 섞이고, 너무 작으면 맥락이 끊겨 엉뚱하게 회수된다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 규정 문서를 문단·조항 단위로 청킹한 뒤 임베딩해줘."
- "조각이 너무 잘게 끊기지 않게, 겹침(overlap)을 조금 줘서 나눠줘."

**흔한 오해:**
- **"조각은 작을수록 좋다"** — 아니다. 지나치게 작으면 맥락이 사라져, 그 자체로는 쓸모없는 조각이 되어 검색에서 안 떠오를 수 있다. 크기·겹침에는 균형점이 있다.
- **"정답 청킹 규칙이 있다"** — 아니다. 문서 종류·질의 성격에 따라 달라지는 **실무 튜닝 영역**이며, 단일 정본 방식은 없다.

**함께 보기:** [RAG](03-building.md#rag--검색-증강-생성-retrieval-augmented-generation), [Embedding model](03-building.md#embedding-model--임베딩-모델), [Vector DB / Embedding search](03-building.md#vector-db--embedding-search--벡터db--임베딩-검색), [Embedding](01-llm-basics.md#embedding--임베딩)

**출처:** Pinecone, *Chunking Strategies for LLM Applications*, [pinecone.io](https://www.pinecone.io/learn/chunking-strategies/) ("chunking is the process of breaking down large text into smaller segments called chunks"). (RAG 실무 개념 — 단일 정본·유일 창시 없음)

---

### Reranking · 리랭킹(리랭커)

> **한 줄 요약:** 1차 검색으로 대충 추린 후보 문서들을, 더 정밀한 모델로 질문과의 관련도를 다시 매겨 상위만 남기는 2단계 정제.

**정의 (Definition)**
- KO: 벡터·키워드 검색으로 빠르게 뽑은 다수의 후보 문서를, 질문과 문서를 함께 입력받는 더 정밀한 모델(주로 cross-encoder)로 관련도를 다시 채점해 순위를 재정렬하고, 상위 소수만 남기는 RAG의 2단계 정제 과정.
- EN: A second-stage step that re-scores the candidates returned by a first-stage retriever using a more precise model (typically a cross-encoder that reads the query and each document together), reorders them by relevance, and keeps only the top few.

**비유 (쉽게):** 1차 검색은 이력서만 훑어 지원자를 100명에서 20명으로 걸러내는 **서류심사**, 리랭킹은 그 20명을 한 명씩 앉혀 놓고 직접 면접해 최종 5명을 고르는 **면접**. 정밀하지만 느려서 소수에게만 한다.

**왜 중요한가 / 언제 쓰나:**
- 1차 검색(임베딩·키워드)은 빠르지만 거칠다 → 리랭커가 상위 결과의 정확도를 끌어올린다.
- cross-encoder는 질문과 문서를 **한꺼번에 읽어** 둘의 상호작용을 보므로, 각자 따로 벡터로 만들어 거리만 재는 1차 검색보다 정밀하다.
- 대표 논문인 BERT 리랭커는 MS MARCO 패시지 검색에서 이전 최고 성능 대비 MRR@10을 상대 27% 끌어올렸다(Nogueira & Cho, 2019).

**실무 예시 / AI에게 이렇게 말한다:**
- "벡터DB에서 후보 30개를 뽑은 다음, cross-encoder 리랭커로 다시 매겨 상위 5개만 컨텍스트에 넣어줘."
- "1차 검색 결과가 어수선하니, 리랭킹 단계를 붙여 관련도 순으로 재정렬해줘."

**흔한 오해:**
- **"리랭커가 검색을 대체한다"** — 아니다. 1차 검색 **위에 얹는 2단계**이지, 전체 코퍼스를 리랭커로 훑는 게 아니다. 먼저 후보를 좁힌 뒤에만 쓴다.
- **"많이 리랭킹할수록 좋다"** — 아니다. cross-encoder는 후보마다 모델을 한 번씩 돌려야 해 **느리고 비싸므로**, 후보 소수(보통 수십 개)에만 적용한다.

**함께 보기:** [RAG](03-building.md#rag--검색-증강-생성-retrieval-augmented-generation), [Vector DB / Embedding search](03-building.md#vector-db--embedding-search--벡터db--임베딩-검색), [Embedding model](03-building.md#embedding-model--임베딩-모델)

**출처:** Nogueira & Cho (2019), *Passage Re-ranking with BERT*, [arXiv:1901.04085](https://arxiv.org/abs/1901.04085) (확인 2026-07-12). 보조(상용 예): Cohere, *Rerank Overview*, [docs.cohere.com](https://docs.cohere.com/docs/rerank-overview) ("Given a query and a list of documents, Rerank indexes the documents from most to least semantically relevant to the query"). (대표 문헌 — 유일 창시 아님)

---

### Structured output / JSON mode · 구조화 출력

> **한 줄 요약:** 모델이 자유로운 문장이 아니라, 미리 정한 **틀(예: JSON 스키마)**에 딱 맞춰 답하도록 강제하는 기능.

**정의 (Definition)**
- KO: 모델의 출력을 자유 텍스트가 아니라 정해진 스키마(대개 JSON)에 반드시 부합하도록 제약하는 기능. 필수 키 누락이나 잘못된 값 없이 정해진 형식만 나오게 보장한다.
- EN: A feature that constrains the model's output to always conform to a supplied schema (typically JSON), so required keys aren't omitted and invalid values aren't produced.

**비유 (쉽게):** 자유 서술형 답안 대신 **칸이 정해진 서식(양식지)**을 내미는 것. "이름·날짜·금액 칸을 채워라"라고 칸을 못박아 두면, 답이 제멋대로 흩어지지 않고 항상 같은 자리에 들어온다.

**왜 중요한가 / 언제 쓰나:**
- 모델 답을 **프로그램이 곧바로 받아 쓰려면** 형식이 일정해야 한다 — 시스템 연동·자동화의 필수 관문이다.
- 자유 텍스트를 정규식으로 긁어 파싱하다 깨지는 사고를 없애, 재시도·후처리 비용을 줄인다.
- 도구 사용(function calling)과 짝을 이룬다 — 도구에 넘길 인자를 스키마에 맞춰 안정적으로 뽑을 때 쓴다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 계약서에서 당사자·금액·기일을 뽑아, {parties, amount, due_date} JSON 스키마로만 답해줘."

**흔한 오해:**
- **"JSON mode면 스키마까지 지켜진다"** — 아니다. 단순 JSON mode는 *유효한 JSON*만 보장하고, 내가 정한 *스키마 준수*까지는 보장하지 않는다. 스키마 강제는 Structured Outputs 계열의 기능이다.
- **"형식만 맞으면 내용도 맞다"** — 아니다. 틀은 맞아도 값이 틀릴 수 있다. 구조화 출력은 **형식**을 보장할 뿐 사실 정확성까지 보장하지는 않는다.

**함께 보기:** [Tool use / Function calling](03-building.md#tool-use--function-calling--도구-사용--함수-호출), [Agent](03-building.md#agent--에이전트-ai-agent)

**출처:** OpenAI, *Structured Outputs*, [developers.openai.com](https://developers.openai.com/api/docs/guides/structured-outputs) ("ensures the model will always generate responses that adhere to your supplied JSON Schema"; "only Structured Outputs ensure schema adherence"; 확인 2026-07-12). 보조: Anthropic, *Increase output consistency / Structured outputs*, [platform.claude.com](https://platform.claude.com/docs/en/build-with-claude/structured-outputs) ("guaranteed schema compliance"). (벤더별 구현 병존 — 유일 창시 아님.)

---

### Harness · 하네스 (에이전트 하네스 / 평가 하네스)

> **한 줄 요약:** 모델을 감싸 도구·권한·컨텍스트·종료 조건을 정하는 실행 껍데기. 같은 모델이라도 하네스가 다르면 결과가 달라진다.

**정의 (Definition)**
- KO: 모델 자체가 아니라 **모델 바깥에서 실행을 구성하는 층**. 어떤 도구를 노출할지, 무엇을 승인받게 할지, 컨텍스트를 어떻게 실어 나를지, 언제 멈출지를 정한다. 평가 맥락에서는 모델을 정해진 절차로 반복 실행·채점하는 **평가 프레임워크**를 뜻한다.
- EN: The layer *around* the model that determines which tools are exposed, what needs approval, how context is delivered, and when execution stops. In evaluation contexts it refers to a framework that runs and scores models under a fixed protocol.

**비유 (쉽게):** 같은 요리사라도 **주방이 다르면 다른 음식이 나온다.** 불의 세기, 손 닿는 곳의 재료, 쓸 수 있는 칼, 언제 접시를 내보내는지를 정하는 게 주방(하네스)이다. 요리사(모델)를 바꾸지 않고도 결과가 바뀐다.

**왜 중요한가 / 언제 쓰나:**
- **벤치마크 점수를 읽을 때 반드시 확인할 변수다.** 같은 모델도 하네스 설정에 따라 성적이 갈리므로, 모델 비교는 하네스를 고정한 상태에서만 성립한다.
- 실무의 통제점이 대부분 여기 있다 — 권한 승인, 도구 노출 범위, 종료 조건은 모델 파라미터가 아니라 하네스가 정한다.
- **용어가 아직 정착 중이다.** 초기에는 주로 *평가* 프레임워크를 가리켰고(EleutherAI, 2021), 최근에는 *에이전트 실행* 환경을 가리키는 쓰임이 늘었다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 벤치마크 결과를 비교하기 전에, 두 모델이 같은 하네스에서 측정된 건지 먼저 확인해줘."

**흔한 오해:**
- **"성능은 모델이 정한다"** — 도구·권한·컨텍스트 구성이 결과를 크게 바꾼다. 모델을 바꾸기 전에 하네스를 점검하는 편이 싸고 빠를 때가 많다.
- **"하네스는 하나의 뜻"** — 평가 하네스와 에이전트 하네스는 겹치지만 같지 않다. 어느 쪽인지 문맥으로 가려야 한다.
- **"표준 정의가 있다"** — **없다.** Anthropic 공식 용어집에도 항목이 없다(2026-08-27 확인). 업계 통용어 단계다.

**함께 보기:** [Agent](03-building.md#agent--에이전트-ai-agent), [Evals](07-dev-stages.md#evals--평가벤치마크-evaluation--benchmarks), [Guardrails](12-safety-governance.md#guardrails--가드레일), [MCP](03-building.md#mcp--모델-컨텍스트-프로토콜-model-context-protocol)

**출처:** 평가 하네스 — EleutherAI, *lm-evaluation-harness*, [github.com/EleutherAI/lm-evaluation-harness](https://github.com/EleutherAI/lm-evaluation-harness) ("A framework for few-shot evaluation of language models."; 확인 2026-08-27). 에이전트 하네스 — Anthropic, *A harness for every task: dynamic workflows in Claude Code*(블로그, 2026-06-02), [claude.com/blog](https://claude.com/blog/a-harness-for-every-task-dynamic-workflows-in-claude-code). ⚠ **Anthropic 공식 용어집에는 harness 항목이 없다**(2026-08-27 확인) — 표준 정의가 아직 없는 통용어이며, 유일 창시도 없다.

---

### Agent memory · 에이전트 메모리

> **한 줄 요약:** 에이전트가 대화·작업이 끝나도 정보를 저장해 두었다가 다시 꺼내 써, 여러 세션에 걸쳐 맥락을 이어 가게 하는 것.

**정의 (Definition)**
- KO: 에이전트가 상호작용·작업의 정보를 저장하고 나중에 회상해, 한 번의 대화를 넘어 맥락을 유지하는 능력. 단기 기억은 컨텍스트 윈도우에, 장기 기억은 외부 저장소(예: 벡터DB·DB)에 둔다.
- EN: An agent's ability to store and later recall information from interactions so it retains context beyond a single exchange — short-term memory in the context window, long-term memory in an external store.

**비유 (쉽게):** 사람이 회의 중에 **머릿속에 잠깐 담아 두는 것(단기)**과, 나중에 다시 보려고 **수첩에 적어 두는 것(장기)**의 차이. 컨텍스트 윈도우는 머릿속, 외부 저장소는 수첩이다. 수첩에 적어 두면 다음 회의(다음 세션)에도 이어서 볼 수 있다.

**왜 중요한가 / 언제 쓰나:**
- 컨텍스트 윈도우는 유한하고 세션이 끝나면 비워진다 — 오래 이어지는 작업·개인화에는 창 밖으로 밀려난 정보를 **다시 꺼내 올** 장기 기억이 필요하다.
- 장기 기억은 흔히 정보를 외부 저장소에 넣고 필요할 때 검색해 오는 식이라, RAG와 기법이 겹친다.
- 무엇을 기억하고 무엇을 버릴지(컨텍스트 큐레이션)는 context engineering의 핵심 주제와 맞닿는다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 에이전트가 지난 세션에서 정한 사용자 선호를 기억하게, 장기 메모리를 외부 저장소로 붙여줘."

**흔한 오해:**
- **"컨텍스트 윈도우가 곧 기억이다"** — 아니다. 그건 단기 기억일 뿐, 창 밖으로 밀려나거나 세션이 끝나면 사라진다. 장기 기억은 **별도 저장소**가 있어야 한다.
- **단일 표준 정의·정본 없음** — 아래는 대표적 프레임워크 문서 기준이며, "에이전트 메모리"의 구현·구분(단기/장기)은 표준화돼 있지 않다(제품마다 다르다).

**함께 보기:** [Context window](01-llm-basics.md#context-window--컨텍스트-윈도우), [Agent](03-building.md#agent--에이전트-ai-agent), [RAG](03-building.md#rag--검색-증강-생성-retrieval-augmented-generation), [Context engineering](02-prompting.md#context-engineering--컨텍스트-엔지니어링)

**출처:** LangChain, *LangMem — Core Concepts*, [langchain-ai.github.io/langmem](https://langchain-ai.github.io/langmem/concepts/conceptual_guide/) ("Long-term memory allows agents to remember important information across conversations"; 단기=현재 대화 컨텍스트 / 장기=대화를 넘어 지속; 확인 2026-07-12). 참고: Anthropic도 별도 *Agent memory* 문서를 둔다([platform.claude.com](https://platform.claude.com/docs/en/docs/build-with-claude/agent-memory)). (프레임워크·제공사별 구현 — 단일 정본 없음.)

---

### Prompt caching · 프롬프트 캐싱

> **한 줄 요약:** 매번 똑같이 들어가는 프롬프트 앞부분을 서버가 재사용해, 같은 내용을 다시 계산하지 않고 더 싸고 빠르게 처리하는 것.

**정의 (Definition)**
- KO: 요청마다 반복되는 프롬프트의 **앞부분(접두부)**을 제공사 서버가 저장해 두었다가 다음 요청에서 재사용하는 기능. 재사용된 부분은 일반 입력 토큰보다 싸게 과금되고 처리도 빨라진다.
- EN: A feature where the provider caches a repeated **prefix** of the prompt and reuses it on later requests, so the reused portion is billed at a lower rate and processed faster than ordinary input tokens.

**비유 (쉽게):** 매번 같은 서류 뭉치를 들고 창구에 가는 것과 같다. 원래는 갈 때마다 직원이 **처음부터 다시 읽어야** 하는데, 캐싱은 "앞의 50장은 지난번 그대로입니다"라고 말해 두는 것이다. 직원은 그 부분을 다시 읽지 않고 **바뀐 뒷장부터** 본다. 다만 서류를 맡겨 두는 데도 비용이 든다.

**왜 중요한가 / 언제 쓰나:**
- **비용이 실제로 정해지는 지점이다.** 긴 시스템 프롬프트·지시문·참고 문서를 매 요청에 넣는 구조라면, 캐싱 여부가 청구서를 좌우한다.
- 벤더마다 **설계가 다르다.** Anthropic은 어디까지 캐시할지 요청에서 명시하고(`cache_control`), OpenAI는 일정 길이를 넘으면 **자동으로** 적용된다.
- 캐시는 **앞에서부터 연속으로만** 맞는다. 그래서 자주 바뀌는 내용을 앞에 두면 뒤의 고정 부분까지 캐시가 깨진다 — **고정된 것을 앞에, 바뀌는 것을 뒤에** 두는 것이 설계 원칙이 된다.

**실무 예시 / AI에게 이렇게 말한다:**
- "시스템 프롬프트와 참고 문서는 앞에 고정하고 사용자 질문만 뒤에 붙이도록 요청 구조를 바꿔줘 — 캐시가 깨지지 않게."
- "응답에서 캐시 읽기·쓰기 토큰이 각각 얼마나 잡히는지 찍어 줘."

**흔한 오해:**
- **"캐싱은 언제나 이득"** — 아니다. Anthropic 문서는 **캐시 쓰기가 일반 입력 토큰보다 비싸다**고 명시한다. 저장해 둔 것을 충분히 재사용하지 못하면 오히려 손해다.
- **"짧은 프롬프트도 반복하면 할인된다"** — OpenAI 쪽은 **1,024 토큰을 넘는 프롬프트**에만 적용되며, 그보다 짧으면 아무리 반복해도 캐시 할인이 없다고 문서가 못 박는다.
- **[KV 캐시](01-llm-basics.md#kv-cache--kv-캐시)와 같은 것이 아니다** — KV 캐시는 답 한 번을 생성하는 **모델 내부**의 재사용이고, 프롬프트 캐싱은 **요청과 요청 사이**에 걸친 서비스 기능이다.

**함께 보기:** [Token](01-llm-basics.md#token--토큰), [Context window](01-llm-basics.md#context-window--컨텍스트-윈도우), [KV cache](01-llm-basics.md#kv-cache--kv-캐시), [System prompt](02-prompting.md#system-prompt--시스템-프롬프트), [Context engineering](02-prompting.md#context-engineering--컨텍스트-엔지니어링)

**출처:** 제공사 공식 문서(벤더별 구현이 병존하는 기능 — 유일 창시 없음). Anthropic, *Prompt caching*, [docs.anthropic.com](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching) (`cache_control`로 캐시 지점을 지정, 기본 수명 5분·1시간 옵션, 응답에 `cache_creation_input_tokens`·`cache_read_input_tokens` 표시, **"Cache writes cost more than normal input tokens"**). OpenAI, *Prompt Caching in the API*, [openai.com/index/api-prompt-caching](https://openai.com/index/api-prompt-caching/) ("No code changes or opt-in are required", **1,024 토큰 초과** 프롬프트에 자동 적용, 가장 긴 기존 접두부를 128토큰 단위로 확장). ⚠ **할인율·수명·최소 길이는 제공사가 수시로 바꾼다** — 금액을 계산에 넣기 전 원문을 다시 확인할 것(확인 2026-08-30).

