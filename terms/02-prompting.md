# 2. 프롬프트 & 상호작용 · Prompting & Interaction

> AI에게 무엇을, 어떻게 말하느냐 — 결과를 좌우하는 상호작용의 언어.
> 출처 근거: [research/SOURCES.md](../research/SOURCES.md) 카테고리 2. 서식: [STYLE.md](../STYLE.md).

---

### Prompt · 프롬프트

> **한 줄 요약:** 모델에게 답을 끌어내려고 건네는 자연어 요청 — 질문·지시가 적힌 쪽지다.

**정의 (Definition)**
- KO: 원하는 응답을 끌어내기 위해 모델에 보내는 자연어 요청(질문·지시·맥락).
- EN: A natural-language request sent to a model to elicit a desired response.

**비유 (쉽게):** 모델에게 건네는 **쪽지**. "이걸 이렇게 해줘"라고 적어 넘긴다. 쪽지가 구체적이고 좋을수록 돌아오는 답도 좋아진다.

**왜 중요한가 / 언제 쓰나:**
- 모델과의 **유일한 조작판**이다. 같은 모델이라도 프롬프트를 어떻게 쓰느냐에 따라 결과가 크게 갈린다.
- 역할·맥락·예시·출력형식을 프롬프트 안에 담아 결과를 원하는 쪽으로 유도한다.

**실무 예시 / AI에게 이렇게 말한다:**
- "아래 계약서에서 손해배상 조항만 뽑아, 조항번호와 함께 표로 정리해줘."
- "초등학생도 이해하게, 3문장으로 요약해줘."

**흔한 오해:**
- **"짧을수록 좋다"** — 아니다. 필요한 맥락·제약·예시를 담을수록 결과가 안정된다. 관건은 길이가 아니라 명확성이다.
- **"모델이 내 지시를 사람처럼 '이해'한다"** — 과신 금지. 모델은 통계적으로 다음 토큰을 이어붙일 뿐, 의도를 진짜로 파악한다는 보장은 없다.

**함께 보기:** [System prompt](02-prompting.md#system-prompt--시스템-프롬프트), [Few-shot / In-context learning](02-prompting.md#few-shot--in-context-learning--퓨샷인컨텍스트-러닝), [Context window](01-llm-basics.md)

**출처:** Google Cloud, *Generative AI glossary*, [cloud.google.com](https://cloud.google.com/discover/what-is-prompt-engineering). 보조: OpenAI, *Prompting guide*, [platform.openai.com](https://platform.openai.com/docs/guides/prompt-engineering).

---

### System prompt · 시스템 프롬프트

> **한 줄 요약:** 대화 앞단에서 모델의 역할·어조·경계를 미리 정해두는 지시문.

**정의 (Definition)**
- KO: 대화 첫머리에 놓여 모델의 역할·어조·행동 경계를 규정하는 'system' 역할의 지시문.
- EN: A 'system'-role instruction placed ahead of the conversation that sets the model's role, tone, and behavioral boundaries.

**비유 (쉽게):** 배우에게 무대에 오르기 전 미리 쥐여주는 **배역 설명서**. "너는 친절한 사서야, 존댓말을 쓰고, 모르면 모른다고 해" — 이후 대사(사용자 입력)는 그 배역 위에서 나온다.

**왜 중요한가 / 언제 쓰나:**
- 매 대화마다 반복할 규칙(말투·전문분야·금지사항)을 한 번에 고정한다.
- 챗봇·에이전트의 **기본 성격과 안전 경계**를 설계하는 자리다.

**실무 예시 / AI에게 이렇게 말한다:**
- "System: 너는 한국 법률에 정통한 변호사다. 확실치 않으면 '확인 필요'라고 표시하고 추측하지 마라."
- "System: 항상 한국어 존댓말로, 결론을 먼저 말한 뒤 근거를 대라."

**흔한 오해:**
- **"시스템 프롬프트는 절대 방어벽"** — 아니다. **프롬프트 인젝션**으로 무력화될 수 있다. 사용자 입력에 "앞의 지시는 무시하라" 같은 문구가 섞이면 경계가 뚫릴 수 있어, 보안의 마지막 방어선으로 삼아선 안 된다.
- **"공짜로 붙는다"** — 아니다. 시스템 프롬프트도 토큰을 소모하며 컨텍스트 윈도우를 차지한다. 길수록 남는 작업 공간이 줄어든다.

**함께 보기:** [Prompt](02-prompting.md#prompt--프롬프트), [Context window](01-llm-basics.md), [Token](01-llm-basics.md)

**출처:** OpenAI, *Prompting guide*, [platform.openai.com](https://platform.openai.com/docs/guides/prompt-engineering). 보조: Anthropic, *Glossary / System prompts*, [platform.claude.com](https://platform.claude.com/docs).

---

### Few-shot / In-context learning · 퓨샷·인컨텍스트 러닝

> **한 줄 요약:** 가중치를 건드리지 않고, 프롬프트 안에 예시 몇 개를 보여줘 그 자리에서 과제를 배우게 하는 방식.

**정의 (Definition)**
- KO: 가중치 갱신 없이, 프롬프트 안에 넣은 소수의 예시(demonstration)만으로 모델이 그 자리에서 과제를 수행하는 방식.
- EN: Performing a task from a few in-prompt demonstrations, without any weight updates (in-context learning).

**비유 (쉽게):** 시험 직전, 친구가 "이런 식으로 푸는 거야" 하며 **예제 두세 개**를 보여주는 것. 새로 공부(재훈련)한 게 아니라, 눈앞의 예시를 보고 패턴을 잡아 바로 따라 푼다.

**왜 중요한가 / 언제 쓰나:**
- 예시 개수에 따라 **zero-shot(예시 0개) → one-shot(1개) → few-shot(여러 개)** 으로 나뉜다. 원하는 출력형식·말투를 예시로 못 박을 때 특히 강력하다.
- 파인튜닝 없이, 프롬프트만 바꿔 즉시 과제를 조정할 수 있어 빠르고 싸다.

**실무 예시 / AI에게 이렇게 말한다:**
- "예시) 입력: 좋아요 → 긍정 / 입력: 별로예요 → 부정. 이제 '그저 그래요'를 분류해줘."
- "아래 3개 번역 예시와 같은 어조로, 마지막 문장을 번역해줘."

**흔한 오해:**
- **"예시를 주면 모델이 재훈련된다"** — 아니다. **가중치는 그대로**이고 그 효과는 해당 대화 안에서만 유효하다. 창을 닫으면 배운 것은 사라진다.
- **"예시가 많을수록 항상 좋다"** — 아니다. 예시가 컨텍스트를 잡아먹고, 지나치면 오히려 성능이 흔들릴 수 있다.

**함께 보기:** [Prompt](02-prompting.md#prompt--프롬프트), [Chain-of-thought](02-prompting.md#chain-of-thought--생각의-사슬cot), [Context window](01-llm-basics.md), [Parameter·Weight](01-llm-basics.md)

**출처:** Brown et al. (2020), *Language Models are Few-Shot Learners*, [arXiv:2005.14165](https://arxiv.org/abs/2005.14165) (NeurIPS 2020). 보조: Google Cloud, *GenAI glossary*.

---

### Chain-of-thought · 생각의 사슬(CoT)

> **한 줄 요약:** 답만 내놓지 말고 중간 풀이 과정을 단계별로 쓰게 해, 복잡한 추론의 정확도를 높이는 방식.

**정의 (Definition)**
- KO: 최종 답 이전에 중간 추론 단계의 연쇄를 생성하도록 유도해, 복잡한 추론 과제의 성능을 끌어올리는 프롬프트 기법.
- EN: Prompting the model to produce a chain of intermediate reasoning steps before the final answer, improving performance on complex reasoning tasks.

**비유 (쉽게):** 수학 시험에서 답만 적지 말고 **풀이를 한 줄씩** 적게 하는 것. 단계를 밟아 내려가면 중간에서 실수를 잡을 여지가 생겨, 대뜸 답을 뱉을 때보다 틀릴 확률이 준다.

**왜 중요한가 / 언제 쓰나:**
- 여러 단계를 거쳐야 하는 문제(수식·논리·다단계 추론)에서 정확도를 끌어올린다.
- "단계별로 생각해줘(step by step)" 한 줄만 덧붙여도 유도할 수 있어 값싸게 적용된다.

**실무 예시 / AI에게 이렇게 말한다:**
- "결론만 말하지 말고, 판단 근거를 단계별로 하나씩 짚은 뒤 마지막에 답해줘."
- "이 계산을 단계별로 풀어서 보여줘."

**흔한 오해:**
- **"출력된 추론 = 모델의 진짜 속마음"** — 아니다. 밖으로 적히는 단계는 그럴듯한 **사후 합리화**일 수 있고, 실제 내부 연산과 일치한다는 보장은 없다.
- **"어떤 모델에서든 효과가 같다"** — 아니다. 원 논문 관찰상 이득은 주로 **충분히 큰 모델**에서 나타났고, 작은 모델에선 효과가 제한적이었다.

**함께 보기:** [Few-shot / In-context learning](02-prompting.md#few-shot--in-context-learning--퓨샷인컨텍스트-러닝), [Prompt](02-prompting.md#prompt--프롬프트), [Hallucination](01-llm-basics.md)

**출처:** Wei et al. (2022), *Chain-of-Thought Prompting Elicits Reasoning in Large Language Models*, [arXiv:2201.11903](https://arxiv.org/abs/2201.11903) (NeurIPS 2022).
