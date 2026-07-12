# 1. LLM 기초 · LLM Basics

> 대규모 언어모델을 이해하는 데 꼭 필요한 밑바탕 개념들.
> 출처 근거: [research/SOURCES.md](../research/SOURCES.md) 카테고리 1. 서식: [STYLE.md](../STYLE.md).

---

### Token · 토큰

> **한 줄 요약:** 모델이 글을 읽고 쓸 때 쓰는 최소 단위. 글자도 단어도 아닌, 그 중간쯤의 조각이다.

**정의 (Definition)**
- KO: 모델이 처리하는 텍스트의 최소 단위. 단어·부분단어(subword)·문자·바이트일 수 있으며, Claude 기준 1토큰은 영어로 약 3.5자에 해당한다.
- EN: The smallest unit of text a model processes — a word, subword, character, or byte. For Claude, one token corresponds to roughly 3.5 English characters.

**비유 (쉽게):** 문장을 **레고 블록으로 쪼갠 것**. 모델은 글자를 하나씩 읽는 게 아니라 이 블록 단위로 읽고, 답도 블록을 하나씩 쌓아 만든다. "말하기"는 한 블록, "말하다가"는 다른 블록이 될 수 있다.

**왜 중요한가 / 언제 쓰나:**
- API 요금·속도·길이 제한이 모두 **토큰 수**로 매겨진다(글자 수나 단어 수가 아니다).
- 같은 뜻이라도 한국어는 영어보다 토큰을 더 많이 쓰는 경향이 있어, 비용·길이를 가늠할 때 감안해야 한다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 문서가 대략 몇 토큰인지 알려줘 — 컨텍스트 윈도우에 들어갈지 보게."
- "답변을 500토큰 안쪽으로 짧게 해줘."

**흔한 오해:** **"토큰 = 단어"가 아니다.** 긴 단어 하나가 여러 토큰으로 쪼개지기도 하고, 공백·문장부호도 토큰을 차지한다. 또 한국어는 같은 의미를 담는 데 영어보다 토큰을 더 쓰는 경우가 많다.

**함께 보기:** [Context window](01-llm-basics.md), [Temperature](01-llm-basics.md)

**출처:** Anthropic, *Glossary* (platform.claude.com/docs), [https://platform.claude.com/docs](https://platform.claude.com/docs); 보조: Google Cloud, *Generative AI glossary*.

---

### Context window · 컨텍스트 윈도우

> **한 줄 요약:** 모델이 답을 만들 때 한 번에 "볼 수 있는" 텍스트의 총량. 모델의 작업 기억이다.

**정의 (Definition)**
- KO: 모델이 응답을 생성할 때 되돌아볼 수 있는 텍스트의 양. 프롬프트와 지금까지의 대화를 담는 모델의 작업기억(working memory)에 해당한다.
- EN: The amount of text a model can look back over when generating a response — its working memory, holding the prompt and the conversation so far.

**비유 (쉽게):** 모델의 **책상 크기**. 책상 위에 올려둔 종이만 보면서 일하는데, 책상이 꽉 찼을 때 새 종이를 올리면 **가장 오래된 종이가 밀려 떨어진다**. 떨어진 종이(창 밖으로 나간 초반 대화)는 더 이상 저절로 참고되지 않는다.

**왜 중요한가 / 언제 쓰나:**
- 긴 문서·긴 대화를 다룰 때, 한 번에 넣을 수 있는 분량의 상한이 곧 컨텍스트 윈도우다.
- 대화가 길어지면 초반에 준 지시가 창 밖으로 밀려나 "잊힌 것처럼" 보일 수 있다.

**실무 예시 / AI에게 이렇게 말한다:**
- "대화가 길어졌으니 앞부분 핵심 지시를 다시 정리해서 넣을게."
- "이 긴 보고서를 한 번에 다 넣지 말고, 나눠서 요약하며 진행하자."

**흔한 오해:** 컨텍스트 윈도우는 모델이 **학습한 데이터의 양**과 다르다(그건 훈련 때 이미 굳은 지식이다). 또 창 밖으로 밀려난 초반 대화는 **자동으로 기억되지 않는다** — 필요하면 다시 넣어줘야 한다.

**함께 보기:** [Token](01-llm-basics.md), [Inference](01-llm-basics.md)

**출처:** Anthropic, *Glossary* (platform.claude.com/docs), [https://platform.claude.com/docs](https://platform.claude.com/docs); 보조: Google Cloud, *Generative AI glossary*.

---

### Embedding · 임베딩

> **한 줄 요약:** 단어나 문장의 "뜻"을 숫자 좌표로 바꾼 것. 뜻이 비슷하면 좌표도 가깝다.

**정의 (Definition)**
- KO: 데이터(단어·문장·이미지 등)를, 의미적 관계를 담은 수치 벡터로 변환한 표현.
- EN: A representation that converts data into numerical vectors capturing its semantic relationships.

**비유 (쉽게):** 모든 단어에 **지도 좌표를 붙이는 것**. 뜻이 비슷한 단어는 지도에서 가까이 모인다 — '왕'과 '여왕'이 이웃이 되는 식이다. 컴퓨터는 이 좌표 사이의 거리를 재서 "의미가 얼마나 가까운가"를 계산한다.

**왜 중요한가 / 언제 쓰나:**
- 의미 기반 검색·추천·[RAG](03-building.md)의 바탕이 된다 — 키워드가 달라도 뜻이 가까우면 찾아낸다.
- 텍스트를 숫자로 바꿔야 기계가 "유사도"를 계산할 수 있는데, 그 다리 역할을 한다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 문서들을 임베딩해서 의미가 비슷한 것끼리 묶어줘."
- "질문과 뜻이 가장 가까운 문단을 임베딩으로 찾아줘."

**흔한 오해:** **임베딩 자체가 검색(RAG)은 아니다** — 임베딩은 "의미를 좌표로 바꾸는 표현"이고, 그 좌표로 가까운 것을 찾아 답에 쓰는 것이 검색·RAG다. 임베딩은 그 재료일 뿐이다.

**함께 보기:** [Vector DB / Embedding search](03-building.md), [RAG](03-building.md), [Token](01-llm-basics.md)

**출처:** Mikolov et al. (2013), *Efficient Estimation of Word Representations in Vector Space* (word2vec), [arXiv:1301.3781](https://arxiv.org/abs/1301.3781); 보조 정의: Google Cloud, *Generative AI glossary*. (word2vec은 임베딩을 대표하는 출처이며 이 용어의 **유일한 창시 논문은 아니다** — 임베딩 개념은 그 이전·이후 여러 계보를 가진다.)

---

### Hallucination · 환각(할루시네이션)

> **한 줄 요약:** 모델이 사실이 아닌 내용을, 사실인 것처럼 그럴듯하게 지어내는 현상.

**정의 (Definition)**
- KO: 모델이 사실과 다르거나 근거 없는 내용을 생성하는 현상.
- EN: The phenomenon where a model generates content that is factually incorrect or unsupported.

**비유 (쉽게):** 모르는 문제를 만난 학생이 **"모르겠다"고 하지 않고, 자신 있게 그럴듯한 답을 지어내는** 것과 같다. 문장은 매끄럽고 확신에 차 있지만, 내용은 사실이 아닐 수 있다.

**왜 중요한가 / 언제 쓰나:**
- 법률·의료·수치처럼 **정확성이 생명인 영역**에서 특히 위험하다 — 출력이 유창할수록 오히려 속기 쉽다.
- 그래서 중요한 답은 [RAG](03-building.md)로 근거 문서를 붙이거나, 사람이 출처를 검증하는 절차가 필요하다.

**실무 예시 / AI에게 이렇게 말한다:**
- "확실하지 않으면 지어내지 말고 '모른다'고 해줘."
- "각 주장 옆에 근거 출처를 달고, 없으면 '확인 필요'로 표시해줘."

**흔한 오해:** 환각을 **"거짓말"로 의인화하지 말 것** — 모델이 속이려는 의도가 있는 게 아니라, 다음 토큰을 확률적으로 잇는 과정에서 나오는 부산물이다. 또 [Temperature](01-llm-basics.md)를 0으로 낮춰도 환각은 **완전히 사라지지 않는다.**

**함께 보기:** [RAG](03-building.md), [Temperature](01-llm-basics.md), [Inference](01-llm-basics.md)

**출처:** Ji et al. (2022), *Survey of Hallucination in Natural Language Generation*, [arXiv:2202.03629](https://arxiv.org/abs/2202.03629) (ACM Computing Surveys); 보조 정의: Google Cloud, *Generative AI glossary*. (이는 현상을 정리한 **서베이(2차 문헌)**로, 환각을 처음 규정한 단일 창시 논문을 특정하기는 어렵다.)

---

### Inference · 추론(실행단계)

> **한 줄 요약:** 학습이 끝난 모델을 실제로 써서, 입력을 넣고 출력을 받아내는 단계.

**정의 (Definition)**
- KO: 학습이 끝난 모델을 실제로 사용해, 입력을 받아 출력을 생성하는 단계.
- EN: The stage of using a trained model to take an input and produce an output.

**비유 (쉽게):** **훈련이 끝난 뒤 실제 시합에서 뛰는 것**. 연습(학습)으로 실력을 이미 다졌고, 이제 경기장(실사용)에서 그 실력을 그대로 쓴다. 경기 중에 실력 자체가 바뀌지는 않는다.

**왜 중요한가 / 언제 쓰나:**
- 우리가 챗봇에 질문하고 답을 받는 순간이 바로 추론이다 — 서비스 운영 비용·속도가 여기서 결정된다.
- 학습(training)은 한 번 크게 하고, 추론(inference)은 사용자마다 매번 일어난다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 모델을 배포해서 실시간 추론으로 답하게 해줘."
- "추론 속도가 느린데, 응답 길이를 줄여서 빠르게 해줘."

**흔한 오해:**
- **추론(inference)은 학습(training)과 다르다** — 추론 중에는 모델의 [가중치](01-llm-basics.md)가 바뀌지 않는다(실력이 고정된 채로 쓰기만 한다).
- 한글 "추론"이 겹쳐 **reasoning(사고력·단계적 사고)과 혼동되기 쉽다.** 여기서 inference는 "모델을 실행하는 단계"를 뜻하고, reasoning은 "문제를 단계적으로 풀어내는 능력"을 뜻하는 별개 개념이다.

**함께 보기:** [Parameter / Weight](01-llm-basics.md), [Context window](01-llm-basics.md)

**출처:** Google, *Machine Learning Glossary*, [https://developers.google.com/machine-learning/glossary](https://developers.google.com/machine-learning/glossary).

---

### Parameter / Weight · 파라미터·가중치

> **한 줄 요약:** 모델 안에 든 수백억 개의 조절 손잡이. 학습이란 이 손잡이들을 돌려 맞추는 일이다.

**정의 (Definition)**
- KO: 입력 처리와 출력을 결정하는 모델 내부의 변수. 학습으로 조정되는 대표적인 값이 가중치(weight)와 편향(bias)이다.
- EN: The internal variables that determine how a model processes input and produces output; the values adjusted during training are chiefly the weights and biases.

**비유 (쉽게):** 거대한 믹싱 콘솔에 달린 **수백억 개의 조절 손잡이**. 학습이란 정답에 가까운 소리가 나도록 이 손잡이들을 조금씩 돌려 맞추는 과정이고, 다 맞춰진 손잡이 값들의 집합이 곧 "학습된 모델"이다.

**왜 중요한가 / 언제 쓰나:**
- "70억(7B)·700억(70B) 파라미터 모델" 같은 표현이 모델의 크기를 가리킬 때 쓰인다.
- 파인튜닝·[LoRA](04-finetuning.md) 같은 커스터마이징은 결국 "어떤 손잡이를, 얼마나 건드리느냐"의 문제다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 작업엔 굳이 큰 모델이 필요 없으니, 작은 파라미터 모델로 충분한지 보자."
- "파인튜닝으로 어떤 가중치가 바뀌는지 설명해줘."

**흔한 오해:**
- **"파라미터가 많을수록 항상 더 똑똑"한 것은 아니다** — 데이터 품질·학습 방법·용도 적합성이 크기만큼 중요하다.
- 학습으로 조정되는 파라미터(weight·bias)와, 사람이 실행 전에 손으로 정하는 **하이퍼파라미터([온도](01-llm-basics.md) 등)를 혼동하지 말 것.**

**함께 보기:** [Inference](01-llm-basics.md), [Temperature](01-llm-basics.md)

**출처:** Google Cloud, *Generative AI glossary*; IBM, *What are LLM parameters?*, [https://www.ibm.com/think/topics/llm-parameters](https://www.ibm.com/think/topics/llm-parameters).

---

### Temperature · 온도

> **한 줄 요약:** 답을 고를 때 "주사위를 얼마나 세게 흔드느냐"를 정하는 값. 높으면 뜻밖의 말, 낮으면 무난한 말.

**정의 (Definition)**
- KO: 생성의 무작위성을 조절하는 값. 높이면 창의적·다양한 출력, 낮추면 보수적·결정론에 가까운 출력이 나온다.
- EN: A value that controls the randomness of generation — higher yields more creative and varied output, lower yields more conservative, near-deterministic output.

**비유 (쉽게):** 다음 단어를 고를 때 **주사위를 흔드는 세기**. 세게 흔들면(높은 온도) 확률이 낮은 뜻밖의 단어까지 튀어나오고, 살살 흔들면(낮은 온도) 가장 무난한 단어로 얌전히 수렴한다.

**왜 중요한가 / 언제 쓰나:**
- 브레인스토밍·카피 초안처럼 다양성이 필요하면 높이고, 요약·추출·코드처럼 일관성이 필요하면 낮춘다.
- 같은 프롬프트라도 온도에 따라 결과의 결이 크게 달라진다.

**실무 예시 / AI에게 이렇게 말한다:**
- "아이디어를 다양하게 뽑고 싶으니 온도를 높여줘."
- "정확한 추출 작업이니 온도를 최대한 낮춰 일관되게 해줘."

**흔한 오해:**
- **온도 0이라고 매번 완전히 똑같은 출력이 나오는 것은 아니다** — Anthropic도 온도 0이 완전한 결정론을 보장하지는 않는다고 명시한다.
- 온도가 높다고 해서 답이 **"더 똑똑"해지는 것도 아니다** — 다양해질 뿐, 정확도가 올라가는 게 아니다.

**함께 보기:** [Token](01-llm-basics.md), [Hallucination](01-llm-basics.md), [Parameter / Weight](01-llm-basics.md)

**출처:** Anthropic, *Glossary* (platform.claude.com/docs), [https://platform.claude.com/docs](https://platform.claude.com/docs); 보조: Google Cloud, *Generative AI glossary*.

---

### Multimodal · 멀티모달

> **한 줄 요약:** 글만이 아니라 이미지·음성·영상까지 한 모델이 함께 다루는 것. 눈·귀·입을 여럿 가진 셈이다.

**정의 (Definition)**
- KO: 텍스트뿐 아니라 이미지·음성·영상 등 여러 형식(모달리티, modality)을 함께 입력·출력으로 다루는 모델이나 시스템.
- EN: A model or system that handles multiple modalities — not just text but also images, audio, or video — as input and/or output.

**비유 (쉽게):** 글만 읽던 사람이 **눈과 귀까지 갖게 된 것**. 편지(텍스트)만 받던 상대가 이제 사진도 보고 목소리도 들을 수 있게 되어, "이 사진 속 표지판 뭐라고 쓰여 있어?" 같은 부탁을 한 번에 알아듣는다. 다만 감각마다 밝기가 달라, 눈은 밝은데 귀는 어두운 식으로 형식별 실력 차가 난다.

**왜 중요한가 / 언제 쓰나:**
- 문서 스캔본·사진·도표·음성 녹취처럼 **글이 아닌 자료**를 그대로 넣고 물어볼 수 있다.
- 하나의 모델로 "이미지 보고 설명", "표 읽어 요약", "음성 받아쓰기"를 처리해, 형식마다 별도 도구를 붙이던 수고를 줄인다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 계약서 스캔 이미지를 읽고 핵심 조항을 정리해줘."
- "이 도표 사진에서 숫자를 뽑아 표로 만들어줘."

**흔한 오해:** **"이미지도 되니 모델이 진짜로 본다"는 뜻이 아니다** — 사람처럼 이해하는 게 아니라 여전히 확률적으로 처리하므로, 사진 속 글자를 잘못 읽거나 없는 것을 지어내는 [환각](01-llm-basics.md)이 얼마든지 생길 수 있다. 또 **형식마다 성능이 고르지 않다** — 텍스트는 잘해도 음성·영상은 약할 수 있다.

**함께 보기:** [Hallucination](01-llm-basics.md), [Token](01-llm-basics.md), [Embedding](01-llm-basics.md)

**출처:** Radford et al. (2021), *Learning Transferable Visual Models From Natural Language Supervision* (CLIP), [arXiv:2103.00020](https://arxiv.org/abs/2103.00020); 보조 정의: Google Cloud, *Generative AI glossary*. (CLIP은 이미지·텍스트를 잇는 **대표적 멀티모달 학습 사례**일 뿐, 멀티모달이라는 넓은 개념의 **유일한 창시 논문은 아니다** — 음성·영상 등 다른 계보가 병존한다.)

---

### Knowledge cutoff · 지식 컷오프(학습 컷오프)

> **한 줄 요약:** 모델의 지식이 멈춰 있는 시점. 그 이후에 벌어진 일은 (검색·도구 없이는) 모른다.

**정의 (Definition)**
- KO: 모델의 학습 데이터가 특정 시점까지만 포함되어, 그 이후의 사건·정보는 (검색·도구 없이는) 알지 못한다는 한계이자 그 기준 시점.
- EN: The point up to which a model's training data extends — after which it does not know later events or information without external tools or search.

**비유 (쉽게):** **특정 날짜에 인쇄돼 멈춘 백과사전**. 그 날까지의 내용은 담겨 있지만, 인쇄 뒤에 일어난 일은 페이지에 없다. 아무리 최신 사건을 물어도, 책 자체가 그 날짜에서 멈춰 있으니 답할 재료가 없는 것이다.

**왜 중요한가 / 언제 쓰나:**
- 최신 뉴스·법령 개정·시세처럼 **컷오프 이후의 사실**을 물으면, 모델이 모르거나 옛 정보로 답할 수 있어 반드시 별도 확인이 필요하다.
- 그래서 최신성이 중요한 작업은 [RAG](03-building.md)·검색·도구로 **바깥의 최신 자료를 붙여** 컷오프의 빈틈을 메운다.

**실무 예시 / AI에게 이렇게 말한다:**
- "네 지식 컷오프가 언제인지 먼저 알려주고, 그 이후 내용은 추측하지 말아줘."
- "최근 개정된 조문이 필요하니, 기억에 의존하지 말고 검색해서 출처와 함께 확인해줘."

**흔한 오해:** **"AI가 인터넷에 실시간으로 연결돼 있다"는 뜻이 아니다** — 기본 상태의 모델은 컷오프까지의 고정된 지식만 가지며, 실시간으로 웹을 뒤지지 않는다. 컷오프 이후를 알려면 검색·[RAG](03-building.md)·도구를 따로 붙여야 한다. 또 컷오프는 "그 날짜 이후 전부 완벽히 안다"는 뜻도 아니다 — 제공사 문서도 지식이 **가장 두텁고 신뢰할 만한 시점**과 학습 데이터의 넓은 범위를 구분해 표기한다.

**함께 보기:** [Hallucination](01-llm-basics.md), [Inference](01-llm-basics.md), [RAG](03-building.md)

**출처:** 제공사 모델 문서(검증) — Anthropic, *Models overview*, [https://platform.claude.com/docs/en/about-claude/models/overview](https://platform.claude.com/docs/en/about-claude/models/overview) (모델별 "Reliable knowledge cutoff"·"Training data cutoff" 명시); OpenAI, *Models*, [https://developers.openai.com/api/docs/models](https://developers.openai.com/api/docs/models) (모델별 "Knowledge cutoff" 명시). 단일 논문이 아니라 제공사 모델 문서에 근거한 개념이다.
