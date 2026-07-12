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

---

### LLM · 대규모 언어 모델 (Large Language Model) / Foundation model · 파운데이션 모델

> **한 줄 요약:** 방대한 글로 훈련해 "다음에 올 말"을 예측하며 언어를 이해·생성하는 거대한 신경망. 이렇게 넓게 훈련돼 온갖 일에 갖다 쓸 수 있는 범용 모델을 파운데이션 모델이라 부르고, LLM은 그 대표 사례다.

**정의 (Definition)**
- KO: **LLM(대규모 언어 모델)** = 방대한 텍스트로 학습해 다음 토큰을 예측하며 언어를 이해·생성하는 대형 신경망. **파운데이션 모델** = 대량의 데이터로 사전학습되어 다양한 하위 과제(번역·요약·질의응답 등)에 적응할 수 있는 범용 모델이며, LLM은 그 대표적 사례다.
- EN: An **LLM (Large Language Model)** is a large neural network trained on vast text to predict the next token, thereby understanding and generating language. A **foundation model** is a general-purpose model pretrained on broad data that can be adapted to many downstream tasks; the LLM is its most prominent instance.

**비유 (쉽게):** 수많은 책을 읽고 **문장을 이어 쓰는 감각을 몸에 익힌 사람**과 같다. "옛날 옛적에 …" 다음에 무슨 말이 자연스러운지 감으로 아는 것이다. 그리고 파운데이션 모델은 **밑간을 넓게 해 둔 육수**에 비유할 수 있다 — 한 번 깊게 우려 두면, 여기서 국·찌개·전골 어느 요리로도 갈라 쓸 수 있다. LLM은 그 육수로 만든 대표 요리다.

**왜 중요한가 / 언제 쓰나:**
- 요즘 쓰는 챗봇·글쓰기·코딩 도구의 **엔진**이 대부분 LLM이다 — 이 글의 거의 모든 다른 항목이 결국 LLM을 다루기 위한 개념이다.
- "파운데이션 모델"이라는 명명은 **하나를 크게 훈련해 여러 과제에 재사용한다**는 패러다임 전환을 가리킨다 — 과제마다 모델을 새로 만들던 시대와 대비된다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 작업에 굳이 최상위 LLM이 필요한지, 작은 모델로 충분한지 먼저 따져줘."
- "범용 파운데이션 모델을 우리 업무 데이터로 [파인튜닝](04-finetuning.md)하면 뭐가 좋아지는지 설명해줘."

**흔한 오해:**
- **LLM은 "정답 데이터베이스"가 아니다** — 사실을 찾아 꺼내는 게 아니라 다음 토큰을 확률적으로 이어 붙이는 것이라, 그럴듯한 [환각](01-llm-basics.md)이 섞일 수 있다.
- **"LLM = 파운데이션 모델"로 등치하지 말 것** — 파운데이션 모델은 이미지·음성 등 언어가 아닌 것도 포함하는 더 넓은 개념이고, LLM은 그중 언어를 다루는 대표 갈래다.

**함께 보기:** [Token](01-llm-basics.md), [Parameter / Weight](01-llm-basics.md), [Inference](01-llm-basics.md), [Multimodal](01-llm-basics.md), [Pretraining](01-llm-basics.md), [Transformer / Attention](01-llm-basics.md), [Fine-tuning](04-finetuning.md)

**출처:** "파운데이션 모델"이라는 명명·정식화는 Bommasani et al. (2021), *On the Opportunities and Risks of Foundation Models*, [arXiv:2108.07258](https://arxiv.org/abs/2108.07258) (Stanford CRFM)에서 검증됨. **LLM은 단일 정본 논문이 없는 넓은 용어**로, 이 항목의 정의는 대표 문헌과 제공사 문서를 종합한 것이며 유일 창시를 특정하지 않는다.

---

### Transformer / Attention · 트랜스포머 / 어텐션

> **한 줄 요약:** 오늘날 거의 모든 LLM의 뼈대가 되는 설계. 그 핵심은 문장 속 단어들이 서로 얼마나 관련되는지 "주의(attention)"를 나눠 매기며 문맥을 읽는 방식이다.

**정의 (Definition)**
- KO: **트랜스포머** = 현대 거의 모든 LLM의 바탕이 되는 신경망 아키텍처. **어텐션(주의)** = 문장 속 각 단어가 다른 단어들과 얼마나 관련되는지 가중치를 매겨, 문맥에 따라 어디에 집중할지 정하는 메커니즘.
- EN: The **Transformer** is the neural-network architecture underlying almost all modern LLMs. **Attention** is its core mechanism: it weighs how much each word relates to the others, deciding where to focus given the context.

**비유 (쉽게):** 회의에서 한 사람의 말을 이해하려고 **참석자 모두를 둘러보며 "지금 이 말과 관련 깊은 사람이 누구지?"를 가늠하는 것**과 같다. "그 계약을 그가 파기했다"에서 '그가'가 누구인지 알려면 앞 문장들을 돌아봐야 하는데, 어텐션은 바로 그 "누구를 얼마나 참고할지"의 배분이다. 트랜스포머는 이 눈길 배분을 층층이 쌓아 만든 구조물이다.

**왜 중요한가 / 언제 쓰나:**
- **왜 요즘 AI가 갑자기 잘하게 됐는지**를 설명하는 핵심 전환점이다 — 트랜스포머는 문장을 앞에서부터 차례로만 읽던 이전 방식과 달리 병렬 처리가 가능해, 훨씬 크고 빠른 학습을 열었다.
- [컨텍스트 윈도우](01-llm-basics.md)·[파라미터](01-llm-basics.md) 같은 개념이 왜 성능·비용을 좌우하는지 이해하려면 이 아키텍처가 밑그림이 된다.

**실무 예시 / AI에게 이렇게 말한다:**
- "어텐션이 긴 문서에서 왜 앞뒤 문맥을 연결할 수 있는지 쉬운 말로 설명해줘."
- "우리 모델이 트랜스포머 기반인지, 그게 처리 길이·비용에 어떤 영향을 주는지 알려줘."

**흔한 오해:**
- **어텐션은 사람의 "집중력"과 같지 않다** — 의식적으로 주의를 기울이는 게 아니라, 단어들 사이 관련도를 숫자 가중치로 계산하는 수학 연산이다.
- **트랜스포머라고 문장 전체를 무제한으로 보는 것은 아니다** — 한 번에 다룰 수 있는 양은 [컨텍스트 윈도우](01-llm-basics.md)로 제한된다.

**함께 보기:** [Parameter / Weight](01-llm-basics.md), [Context window](01-llm-basics.md), [Token](01-llm-basics.md), [Pretraining](01-llm-basics.md), [LLM](01-llm-basics.md)

**출처:** Vaswani et al. (2017), *Attention Is All You Need*, [arXiv:1706.03762](https://arxiv.org/abs/1706.03762) — 트랜스포머 아키텍처와 어텐션 메커니즘을 정식화한 원 논문(검증).

---

### Generative AI · 생성형 AI (GenAI)

> **한 줄 요약:** 배운 패턴을 바탕으로 글·이미지·음성·코드 같은 "새 콘텐츠"를 만들어 내는 AI의 총칭. 분류·예측만 하던 전통 AI와 대비된다.

**정의 (Definition)**
- KO: 학습한 패턴을 바탕으로 텍스트·이미지·음성·코드 등 새로운 콘텐츠를 생성하는 AI를 아우르는 총칭. 주어진 것을 분류·예측하는 데 초점을 둔 전통적 AI와 대비되는 개념이다.
- EN: An umbrella term for AI that produces new content — text, images, audio, code — from learned patterns, in contrast to traditional AI focused on classifying or predicting existing data.

**비유 (쉽게):** 전통 AI가 **바구니 속 과일을 "사과냐 배냐" 골라내는 감별사**라면, 생성형 AI는 **주문을 받아 새 그림을 그려 주는 화가**에 가깝다. 앞의 것은 있는 것을 판정하고, 뒤의 것은 없던 것을 만들어 낸다.

**왜 중요한가 / 언제 쓰나:**
- 요즘 화제가 되는 챗봇·이미지 생성기·코드 도우미가 모두 이 범주에 든다 — 뉴스·정책 문서에서 "생성형 AI"라고 뭉뚱그려 부르는 대상이 이것이다.
- 규제·계약서에서 대상 기술을 지칭할 때 자주 쓰이므로, **무엇을 포함하고 무엇이 아닌지**의 경계를 아는 것이 실무상 중요하다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 업무 중 생성형 AI로 자동화할 부분과, 전통적 분류·예측이 더 맞는 부분을 나눠줘."
- "'생성형 AI'라는 표현이 우리 계약서에서 무엇을 가리키는지 범위를 정의해줘."

**흔한 오해:** **"생성형 AI = LLM"이 아니다** — LLM(텍스트)은 생성형 AI의 한 갈래일 뿐, 이미지의 [디퓨전 모델](01-llm-basics.md)처럼 언어가 아닌 생성 방식도 포함한다. 또 **하나의 창시자·정본 정의가 있는 용어가 아니다** — 여러 계보가 합류한 우산 용어다.

**함께 보기:** [LLM](01-llm-basics.md), [Diffusion model](01-llm-basics.md), [Multimodal](01-llm-basics.md), [Hallucination](01-llm-basics.md)

**출처:** **단일 창시·정본 정의가 없는 우산 용어**로, 권위 있는 공식 문서의 정의를 인용한다 — Google Cloud, *Generative AI glossary*; IBM, *What is generative AI?*, [https://www.ibm.com/think/topics/generative-ai](https://www.ibm.com/think/topics/generative-ai). 어느 것도 이 용어의 유일 창시가 아니다.

---

### Pretraining · 사전학습

> **한 줄 요약:** 특정 과제를 가르치기 전에, 방대한 미분류 데이터로 모델에 언어·표현의 기초 체력을 먼저 길러 두는 단계.

**정의 (Definition)**
- KO: 특정 과제 학습에 앞서, 대규모의 라벨 없는(미분류) 데이터로 모델의 기본적인 언어·표현 능력을 먼저 학습시키는 단계. 이후 파인튜닝·정렬을 거쳐 용도에 맞게 특화된다.
- EN: The stage of first training a model on large-scale unlabeled data to build general language and representation ability, before it is specialized through fine-tuning or alignment.

**비유 (쉽게):** **의대 본과에 들어가기 전의 기초 교양·기초 의학 과정**과 같다. 아직 특정 진료과를 정하지 않았지만, 나중에 어느 과로 가든 밑바탕이 되는 폭넓은 기초를 먼저 쌓는 것이다. 전공(파인튜닝)은 그다음에 얹는다.

**왜 중요한가 / 언제 쓰나:**
- 오늘날 LLM의 힘은 대부분 이 단계에서 나온다 — 방대한 데이터로 한 번 크게 사전학습해 두면, 이후엔 비교적 적은 데이터로 여러 과제에 맞춰 쓸 수 있다.
- 사전학습은 막대한 비용이 드는 **한 번의 큰 투자**이고, 그 위에 [파인튜닝](04-finetuning.md)·[인스트럭션 튜닝](04-finetuning.md)이라는 저렴한 특화 단계가 얹힌다는 구조를 이해하면, 커스터마이징 논의가 명확해진다.

**실무 예시 / AI에게 이렇게 말한다:**
- "사전학습된 범용 모델을 우리 판례 데이터로 파인튜닝하는 게 나은지, 사전학습부터 새로 하는 건 비현실적인지 판단해줘."
- "이 모델이 사전학습에서 얻은 일반 지식과, 우리가 파인튜닝으로 더할 전문 지식을 구분해서 설명해줘."

**흔한 오해:**
- **사전학습과 [파인튜닝](04-finetuning.md)은 다른 단계다** — 사전학습은 미분류 데이터로 기초 능력을 넓게 쌓는 단계, 파인튜닝은 그 위에 특정 과제를 특화하는 단계다.
- **사전학습만으로 곧바로 유용한 비서가 되는 것은 아니다** — 지시를 잘 따르게 하려면 [인스트럭션 튜닝](04-finetuning.md)·정렬([RLHF](05-alignment-rl.md)) 같은 후속 단계가 필요하다.

**함께 보기:** [LLM](01-llm-basics.md), [Parameter / Weight](01-llm-basics.md), [Fine-tuning](04-finetuning.md), [Instruction Tuning](04-finetuning.md), [RLHF](05-alignment-rl.md)

**출처:** 대규모 미분류 데이터 기반 사전학습을 대중화한 대표 계보(검증) — Devlin et al. (2018), *BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding*, [arXiv:1810.04805](https://arxiv.org/abs/1810.04805); GPT 계열도 이 "사전학습→특화" 패러다임을 널리 확산시켰다. 사전학습은 특정 단일 논문의 창안이라기보다 여러 계보를 통해 표준이 된 개념이다.

---

### Diffusion model · 디퓨전 모델

> **한 줄 요약:** 뿌연 노이즈에서 시작해 조금씩 노이즈를 걷어내며 이미지·영상을 만들어 내는 모델. 미드저니·Stable Diffusion·Sora류가 이 원리다.

**정의 (Definition)**
- KO: 무작위 노이즈에서 출발해, 노이즈를 점진적으로 제거(denoising)하는 과정을 반복하며 이미지·영상 등을 생성하는 모델. 다음 토큰을 잇는 LLM(텍스트)과는 생성 방식이 다르다.
- EN: A model that generates images or video by starting from random noise and iteratively removing it (denoising), step by step — a generative approach distinct from the next-token prediction of text LLMs.

**비유 (쉽게):** **안개 낀 유리창을 조금씩 닦아 그림을 드러내는 것**과 같다. 처음엔 아무 형체 없는 얼룩(노이즈)뿐이지만, 여러 번에 걸쳐 조금씩 닦아 나가면 점점 또렷한 그림이 나타난다. "무엇을 그릴지"의 지시(프롬프트)가 어느 얼룩을 어떻게 닦을지를 이끈다.

**왜 중요한가 / 언제 쓰나:**
- 오늘날 이미지·영상 생성 도구의 주류 원리다 — 텍스트는 LLM, 이미지·영상은 디퓨전이라는 큰 구도를 알면 [생성형 AI](01-llm-basics.md)의 지형이 잡힌다.
- 저작권·딥페이크 같은 실무 쟁점이 자주 이 계열 모델에서 불거지므로, 작동 원리의 개요를 아는 것이 유용하다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 삽화를 디퓨전 모델로 생성할 때, 프롬프트를 어떻게 써야 원하는 구도가 나오는지 알려줘."
- "우리가 쓰는 이미지 생성 도구가 디퓨전 계열인지, 그게 결과물의 특징에 어떤 영향을 주는지 설명해줘."

**흔한 오해:** **디퓨전은 LLM과 같은 방식으로 작동하지 않는다** — LLM은 다음 토큰을 순서대로 잇지만, 디퓨전은 전체 화면의 노이즈를 여러 번 걷어내며 한꺼번에 다듬는다. 또 **"노이즈에서 그림을 뽑아낸다"고 무에서 창조하는 마법은 아니다** — 학습한 데이터의 분포를 되짚어 그럴듯한 결과를 복원하는 것이다.

**함께 보기:** [Generative AI](01-llm-basics.md), [Multimodal](01-llm-basics.md), [LLM](01-llm-basics.md)

**출처:** Ho et al. (2020), *Denoising Diffusion Probabilistic Models* (DDPM), [arXiv:2006.11239](https://arxiv.org/abs/2006.11239) — 현대 디퓨전 생성모델을 정립한 대표 논문(검증). 원류로는 Sohl-Dickstein et al. (2015), *Deep Unsupervised Learning using Nonequilibrium Thermodynamics*, [arXiv:1503.03585](https://arxiv.org/abs/1503.03585)를 병기한다(검증).
