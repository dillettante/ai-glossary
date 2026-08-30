# 12. 안전·거버넌스 · Safety & Governance

> AI를 책임 있게 쓰고 규제에 대응하기 위한 안전 통제·문서화·규제 개념 — 특히 도입·규제 검토에서 만난다.
> 출처 근거: 각 항목 하단 출처 및 [research/SOURCES.md](../research/SOURCES.md). 서식: [STYLE.md](../STYLE.md).

---

### Guardrails · 가드레일

> **한 줄 요약:** AI가 하면 안 되는 말·형식을 벗어난 답을 내놓지 못하게, 입력과 출력 앞뒤에 세워둔 안전 울타리.

**정의 (Definition)**
- KO: 모델의 입력과 출력을 미리 정한 규칙·필터·정책으로 통제해, 유해·금지·형식 이탈 출력을 차단하거나 교정하는 안전 통제 장치(들)를 통칭한다. 모델 자체를 바꾸는 게 아니라 모델 주위에 얹는 통제층이다.
- EN: A collective term for controls placed around a model's inputs and outputs — rules, filters, and policies — that block or correct harmful, disallowed, or off-format responses. They wrap the model rather than retraining it.

**비유 (쉽게):** 볼링장 초보 레인의 양옆 범퍼와 같다. 공(모델의 답)이 도랑(유해·금지 영역)으로 빠지려 하면 범퍼가 다시 안쪽으로 밀어준다. 볼링 실력(모델 자체)을 바꾸는 게 아니라, 빗나가도 큰 사고가 안 나게 가장자리를 막아두는 것이다.

**왜 중요한가 / 언제 쓰나:**
- 챗봇·업무 자동화를 대중이나 고객에게 열 때, 욕설·개인정보 유출·경쟁사 비방·법적 위험 발언 등을 사전에 막아야 할 때.
- 출력을 정해진 형식(JSON·특정 항목만)으로 강제하거나, 특정 주제(의료·투자 조언 등)를 회피시키는 정책을 걸 때.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 챗봇에 가드레일을 걸어줘 — 개인정보(주민번호·카드번호)가 답변에 나오면 마스킹하고, 정치·투자 조언은 거절하도록."
- "출력이 반드시 아래 JSON 형식만 나오게 가드레일을 붙이고, 벗어나면 다시 생성하게 해줘."

**흔한 오해:** 가드레일은 모델을 "안전하게 만드는" 것이 아니라 **모델 바깥에 덧대는 통제층**이다 — 우회(탈옥)될 수 있고, 완벽한 차단을 보장하지 않는다. 또 "가드레일"은 한 회사·한 제품의 고유 기능이 아니라 여러 도구·기법을 아우르는 일반 용어다(단일 정본 없음).

**함께 보기:** [Red-teaming · 레드팀](#red-teaming--레드팀), [프롬프트 인젝션·탈옥](02-prompting.md#prompt-injection--jailbreak--프롬프트-인젝션--탈옥)

**출처:** 단일 정본 없는 일반 개념(유일 창시 아님). 권위 있는 대표 문서로 NVIDIA, *NeMo Guardrails* 공식 문서, [docs.nvidia.com/nemo/guardrails](https://docs.nvidia.com/nemo/guardrails/latest/index.html) 및 GitHub [github.com/NVIDIA/NeMo-Guardrails](https://github.com/NVIDIA/NeMo-Guardrails). (특정 벤더 구현이며 개념의 유일 창시가 아님 — 안전 통제층을 부르는 여러 도구·관행의 통칭.)

---

### Red-teaming · 레드팀

> **한 줄 요약:** 배포 전에 일부러 AI를 공격하고 꾀어내, 나쁜 답·탈옥·편향 같은 약점을 먼저 찾아내는 모의 침투 시험.

**정의 (Definition)**
- KO: 모델을 배포하기 전에 전문가·도구가 의도적으로 적대적 입력을 던져(유도·공격) 안전 취약점(유해 출력·탈옥·편향·정보 유출)을 체계적으로 찾아내는 적대적 시험 방법. 원래 국방·보안 분야의 '공격자 역할(레드팀)' 개념에서 왔다.
- EN: An adversarial testing method in which experts (and tools) deliberately probe and attack a model before deployment to surface safety weaknesses — harmful outputs, jailbreaks, bias, leakage. The term originates from the "attacker role" in defense and security.

**비유 (쉽게):** 새 자물쇠를 팔기 전에 자물쇠 회사가 도둑 역할을 맡은 사람을 고용해 온갖 방법으로 따보게 하는 것과 같다. 실제 도둑이 오기 전에 우리 편이 먼저 뚫어봐야, 어디가 약한지 알고 고칠 수 있다.

**왜 중요한가 / 언제 쓰나:**
- 챗봇·AI 서비스를 대중에 공개하기 전, "이걸 악용하면 어떤 나쁜 답이 나오나"를 미리 확인해야 할 때.
- 규제·거버넌스 관점에서 "출시 전에 안전성을 시험했다"는 근거(문서)를 남겨야 할 때.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 챗봇을 레드팀 관점에서 공격해봐 — 탈옥·유해 정보 유도·개인정보 추출을 시도하고, 뚫린 사례를 정리해줘."
- "우리 프롬프트의 약점을 찾도록 적대적 테스트 케이스 20개를 만들어줘."

**흔한 오해:** 레드팀은 한 번 통과하면 끝나는 '합격 도장'이 아니다 — 새로운 공격 기법이 계속 나오므로 **반복해야 하는 과정**이고, 취약점이 없음을 증명하지 못한다(있음을 찾아낼 뿐). 또 AI 고유 발명이 아니라 보안 분야에서 온 개념이며, 단일 정본이 없다.

**함께 보기:** [Guardrails · 가드레일](#guardrails--가드레일), [프롬프트 인젝션·탈옥](02-prompting.md#prompt-injection--jailbreak--프롬프트-인젝션--탈옥), [Evals](07-dev-stages.md#evals--평가벤치마크-evaluation--benchmarks)

**출처:** 단일 정본 없는 일반 개념(보안 분야 유래, 유일 창시 아님). 대표 문서: OpenAI, *Red teaming*(안전 접근법 문서), [openai.com/index/red-teaming-network](https://openai.com/index/red-teaming-network/); Anthropic, *Challenges in red teaming AI systems*, [anthropic.com/news/challenges-in-red-teaming-ai-systems](https://www.anthropic.com/news/challenges-in-red-teaming-ai-systems). (두 곳 모두 실제 열람 — 개념의 대표 권위 문서.)

---

### Model card · 모델 카드

> **한 줄 요약:** 이 AI 모델이 뭘 하고, 어디까지 잘하고 어디서 못하는지를 정해진 서식으로 적어둔 '제품 설명서 겸 성분표'.

**정의 (Definition)**
- KO: 학습된 모델의 용도(의도된/부적합한 사용처)·성능·한계·학습데이터·평가 조건·윤리적 고려 사항을 표준 서식으로 문서화한 것. 투명성과 거버넌스의 기본 장치로, 조건(문화·인구집단 등)별 평가 결과를 함께 밝히는 것을 권장한다.
- EN: A short standardized document accompanying a trained model that discloses its intended (and out-of-scope) uses, performance, limitations, training data, evaluation conditions, and ethical considerations — including benchmarked results across different groups/conditions. A basic transparency and governance artifact.

**비유 (쉽게):** 식품 포장의 성분표·영양성분 라벨과 같다. 안에 뭐가 들었고(학습데이터), 누구에게 맞고 누구는 조심해야 하며(용도·한계), 어떤 조건에서 시험했는지(평가)를 사기 전에 읽을 수 있게 해준다. 라벨이 있어야 소비자가 책임 있게 고를 수 있다.

**왜 중요한가 / 언제 쓰나:**
- 외부 모델을 도입할 때, 그 모델의 한계·평가·데이터 출처를 확인해 위험을 가늠할 때(거버넌스·컴플라이언스의 출발점).
- 우리가 만든 모델·서비스를 공개할 때, 책임 있는 사용을 위한 최소한의 문서로 첨부할 때.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 모델의 모델 카드를 만들어줘 — 의도된 용도, 부적합한 용도, 학습데이터 개요, 평가 조건, 알려진 한계·편향을 항목별로."
- "도입 검토용으로, 이 외부 모델의 공개 모델 카드에서 한계와 평가 조건만 뽑아 정리해줘."

**흔한 오해:** 모델 카드는 '성능 자랑 문서'가 아니라 **한계와 부적합한 용도까지 밝히는 문서**가 핵심이다. 또 법으로 강제된 단일 표준 양식이 있는 게 아니라 — 2019년 제안된 관행이 널리 채택된 것이며, 조직마다 항목이 조금씩 다르다.

**함께 보기:** [EU AI Act 위험등급 · EU AI 법 위험 분류](#eu-ai-act-위험등급--eu-ai-법-위험-분류), [Evals](07-dev-stages.md#evals--평가벤치마크-evaluation--benchmarks)

**출처(검증):** Mitchell, Wu, Zaldivar, Barnes, Vasserman, Hutchinson, Spitzer, Raji & Gebru (2019), *Model Cards for Model Reporting*, [arXiv:1810.03993](https://arxiv.org/abs/1810.03993). (초록 실제 열람 — 2018-10 제출·2019-01 개정. 제안 관행이며 법정 단일 양식은 아님.)

---

### Watermarking · 워터마킹

> **한 줄 요약:** AI가 만든 글·이미지임을 나중에 알아볼 수 있게, 사람 눈엔 안 보이는 통계적 표식을 출력에 몰래 심는 기법.

**정의 (Definition)**
- KO: AI 생성물임을 식별할 수 있도록, 사람은 알아채기 어렵지만 알고리즘으로는 검출 가능한 신호(통계적 표식)를 출력에 심는 기법. 텍스트의 경우, 생성 시 특정 토큰군('초록 목록')의 사용을 미세하게 유도하고, 짧은 구간만 봐도 통계적으로(p-값) 검출하도록 설계할 수 있다.
- EN: A technique that embeds a signal into AI outputs — hard for humans to notice but algorithmically detectable — so the content can be identified as AI-generated. For text, one approach softly promotes a set of "green" tokens during generation and detects it statistically (with p-values) from a short span.

**비유 (쉽게):** 지폐에 빛에 비춰야 보이는 숨은 그림(워터마크)을 넣는 것과 같다. 평소엔 안 보이지만, 아는 방법으로 확인하면 "이건 진짜 발행처가 찍은 것"임을 알 수 있다. AI 워터마킹은 글자에 그런 '숨은 통계 무늬'를 넣는다.

**왜 중요한가 / 언제 쓰나:**
- AI 생성 콘텐츠의 출처 표시·식별이 문제 될 때(가짜뉴스·표절·규제상 라벨링 의무 검토 등).
- "이 텍스트가 우리 모델에서 나온 것인지" 사후에 판정해야 할 때의 한 가지 기술적 수단.

**실무 예시 / AI에게 이렇게 말한다:**
- "AI 생성 텍스트 워터마킹의 원리와 한계를 정리해줘 — 어떤 경우에 검출이 실패하는지 포함해서."
- "AI 생성물 표시 의무를 검토 중인데, 워터마킹으로 어디까지 보장되고 무엇이 보장 안 되는지 알려줘."

**흔한 오해:** 워터마킹은 '완벽한 위조 방지'가 아니다 — **다시 쓰기(패러프레이즈)·번역·편집으로 신호가 약해지거나 제거·우회될 수 있다**. 따라서 '워터마크가 없음 = AI가 안 만듦'을 뜻하지 않고, 표시가 있어야 검출되는 방식이라 강한 규제적 보증 수단으로 과신하면 안 된다.

**함께 보기:** [EU AI Act 위험등급 · EU AI 법 위험 분류](#eu-ai-act-위험등급--eu-ai-법-위험-분류), [Model card · 모델 카드](#model-card--모델-카드)

**출처(검증):** J. Kirchenbauer, J. Geiping, Y. Wen, J. Katz, I. Miers, T. Goldstein (2023), *A Watermark for Large Language Models*, [arXiv:2301.10226](https://arxiv.org/abs/2301.10226). (초록 실제 열람 — 2023-01 제출. 논문은 견고성·보안을 논의하며, 패러프레이즈·편집에 의한 제거·우회 한계는 후속 견고성 연구에서 더 상세히 다뤄진다.)

---

### EU AI Act 위험등급 · EU AI 법 위험 분류

> **한 줄 요약:** 유럽연합의 AI 법이 AI를 위험도에 따라 4단계로 나누고(+ 범용 AI는 별도), 단계가 높을수록 더 무거운 의무를 지우는 규제 틀.

**정의 (Definition)**
- KO: EU 인공지능법(AI Act, **Regulation (EU) 2024/1689**)은 AI 시스템을 위험도에 따라 네 단계로 분류한다 — ①**허용 불가 위험(unacceptable risk)**: 금지, ②**고위험(high-risk)**: 엄격한 의무, ③**투명성 위험(transparency risk, 흔히 '제한적 위험')**: 투명성 의무, ④**최소·무위험(minimal or no risk)**: 별도 규제 없음. 여기에 **범용 AI 모델(GPAI)**을 별도 규율하며, 그중 '시스템적 위험(systemic risk)'이 있는 모델에는 추가 의무를 둔다.
- EN: The EU AI Act (**Regulation (EU) 2024/1689**) classifies AI systems by risk into four tiers — ① **unacceptable risk** (banned), ② **high-risk** (strict obligations), ③ **transparency risk** (often called "limited risk"; transparency duties), ④ **minimal or no risk** (no new rules). It separately regulates **general-purpose AI (GPAI) models**, with extra duties for those posing **systemic risk**.

**비유 (쉽게):** 의약품 규제와 비슷하다. 아예 판매 금지 물질(허용 불가), 처방·엄격 관리 대상(고위험), "이건 이런 성분입니다"라고 알릴 의무만 있는 일반약(투명성), 규제가 사실상 없는 생활용품(최소 위험)으로 나누고, 위험이 클수록 지켜야 할 절차가 많아진다.

**왜 중요한가 / 언제 쓰나:**
- EU 시장을 상대하거나 EU 이용자에게 AI 서비스를 제공할 때, 우리 시스템이 어느 등급인지에 따라 의무(문서화·인적 감독·투명성 표시 등)가 크게 달라진다.
- 도입·계약 단계에서 "이 AI가 고위험에 해당하는가"를 먼저 가려야 리스크·비용을 가늠할 수 있다.

**등급별 요지(공식 요약 기준):**
- **허용 불가 위험 (Article 5):** 사람의 안전·권리를 명백히 위협하는 관행을 **금지**(예: 조작적 시스템, 사회적 점수화, 일정한 실시간 원격 생체인식 등). 금지 규정은 Chapter I·II와 함께 **2025-02-02부터 적용 중**이다. 다만 개정 Article 113은 **옴니버스가 Article 5에 신설한 일부 금지 조항에 한해 2026-12-02 적용**을 정한다(해당 호의 정확한 번호는 **확인 필요**).
- **고위험 (Article 6 및 부속서/Annex III 등):** 건강·안전·기본권에 중대한 위험 → **엄격한 의무**(위험관리·양질의 데이터·기록(로깅)·문서화·인적 감독·견고성·보안 등, 시장 출시 전 충족). **2026년 7월 발효된 이른바 'AI 옴니버스'(Regulation (EU) 2026/1744)로 고위험 의무의 적용 시점이 뒤로 미뤄졌다** — 아래 표 참조. 다만 이는 **Article 113(적용일) 개정**이지 Article 6의 분류 기준 자체를 바꾼 것이 아니다.
- **투명성 위험(제한적 위험) — Article 50:** 사람이 AI와 상호작용함을 알리고, AI 생성·조작 콘텐츠(예: 딥페이크)를 식별 가능하게 표시하는 **투명성 의무**(제공자·배포자). 적용 시점은 단계적이다(아래 표).
- **최소·무위험:** 대부분의 AI(예: 스팸 필터, 게임) — **새로운 의무 없음**.
- **범용 AI(GPAI) — Chapter V, Article 51–56:** 모든 GPAI 제공자에 기본 의무(기술문서·다운스트림 정보·저작권 정책·학습데이터 요약, **Article 53**); '시스템적 위험'으로 분류되는 모델(**Article 51**)에는 추가 의무(모델 평가·적대적 테스트·리스크 완화·중대사고 보고·사이버보안, **Article 55**). **Chapter V는 2025-08-02부터 이미 적용 중**이다(고위험과 달리 연기되지 않았다). 2025-08-02 이전에 시장에 출시된 GPAI 모델에 대한 경과규정(2027-08-02까지 유예)이 있다고 알려져 있으나 조항·문언은 **확인 필요**.

**적용 일정(개정 Article 113 기준):**

| 트랙 | 조문 | 적용일 |
|---|---|---|
| 금지 관행 | Article 5 (Ch. I–II) | **2025-02-02** 적용 중 (단 옴니버스가 신설한 일부 Art. 5 조항은 2026-12-02 — 호 번호 확인 필요) |
| 범용 AI(GPAI) | Chapter V (Art. 51·53·55) | **2025-08-02** 적용 중 |
| 투명성 | Article 50 | **2026-08-02** (2026-08-02 이전 출시 시스템의 표시 의무에 경과 유예가 있다고 알려져 있으나 조항·문언은 **확인 필요**) |
| 고위험 — Annex III 유형 | Article 6(2) + Annex III (Ch. III §1–3, Art. 6(5) 제외) | **2027-12-02** (종전 2026-08-02에서 연기) |
| 고위험 — 제품 안전요소 유형 | Article 6(1) + Annex I (Ch. III §1–3, Art. 6(5) 제외) | **2028-08-02** |

연기의 근거는 **Regulation (EU) 2026/1744**(이른바 'Digital Omnibus on AI')로, **2026-07-27 발효**되면서 AI Act의 Article 113(적용일)과 Article 111(경과규정)을 개정했다. **분류 기준(4단계·GPAI 축)이나 의무의 내용이 폐지된 것이 아니라 적용 시점만 미뤄진 것**이다. 규정 전체의 일반 적용일은 여전히 2026-08-02이다.

**실무 예시 / AI에게 이렇게 말한다:**
- "우리 서비스가 EU AI Act에서 고위험에 해당하는지 판단하는 데 필요한 질문 목록을 만들어줘 — 다만 최종 판단은 공식 조문·법률자문으로 확인한다는 전제로."
- "허용 불가·고위험·투명성·최소 위험 4단계의 의무 차이를 표로 정리해줘, 각 항목은 공식 출처 확인 필요 표시와 함께."

**흔한 오해:** ⚠️ **이 항목은 개괄 안내이며 법률자문이 아니다.** 정확한 등급·의무·조문은 반드시 공식 원문(EUR-Lex)과 법률 검토로 확정해야 한다. 흔한 오해로 (1) '4단계' 외에 **범용 AI(GPAI)라는 별도 축**이 있음을 놓치는 것, (2) 세 번째 단계를 '제한적 위험'이라 부르지만 **유럽위원회 공식 요약은 'transparency risk'로 표기**한다는 점, (3) **금지(Article 5)와 고위험(Article 6)은 서로 다른 단계**라는 점(금지는 아예 못 쓰고, 고위험은 의무를 지키면 쓸 수 있음), (4) **"연기됐으니 GPAI도 아직 유예"라는 오해** — 2026년 연기는 **고위험(Ch. III §1–3)에 한정**되고 GPAI(Chapter V)는 2025-08-02부터, 금지(Article 5)는 2025-02-02부터 이미 적용 중이다, (5) **"고위험 연기 = 의무 소멸"이라는 오해** — 개정은 적용일만 미뤘고 위험관리·데이터 거버넌스·문서화·인적 감독·적합성 평가 등 의무 자체는 그대로다, (6) **"기존 생성형 서비스는 표시 의무를 한동안 안 지켜도 된다"는 오해** — 경과 유예가 있더라도 그것은 **기존 출시 시스템의 일부 표시 의무에 한정**되고, Article 50의 나머지 의무는 2026-08-02부터 적용된다(유예의 조항·범위는 **확인 필요**).

**함께 보기:** [Model card · 모델 카드](#model-card--모델-카드), [Watermarking · 워터마킹](#watermarking--워터마킹), [Red-teaming · 레드팀](#red-teaming--레드팀)

**출처(검증):** 유럽연합, *Regulation (EU) 2024/1689 … (Artificial Intelligence Act)*, OJ L, 2024-07-12, EUR-Lex [eur-lex.europa.eu/eli/reg/2024/1689/oj](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng) (CELEX: 32024R1689); 개정: *Regulation (EU) 2026/1744* (Digital Omnibus on AI), 채택 2026-07-08 · OJ 게재 2026-07-24 · 발효 2026-07-27, [eur-lex.europa.eu/legal-content/EN/ALL/?uri=CELEX:32026R1744](https://eur-lex.europa.eu/legal-content/EN/ALL/?uri=CELEX%3A32026R1744) (OJ L_202601744); 개정 반영 통합본 [eur-lex.europa.eu/eli/reg/2024/1689/2026-07-27/eng](https://eur-lex.europa.eu/eli/reg/2024/1689/2026-07-27/eng) (CELEX: 02024R1689-20260727); European Commission, *AI Act — Regulatory framework for AI*, [digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai) 및 *AI Omnibus enters into force*, [digital-strategy.ec.europa.eu/en/news/ai-omnibus-enters-force](https://digital-strategy.ec.europa.eu/en/news/ai-omnibus-enters-force).

> **확인 범위(2026-08-30).** 확인된 것 — 옴니버스 규정의 실재(CELEX 32026R1744)·발효일, 개정 Article 113의 **고위험 적용일(Annex III형 2027-12-02, Annex I형 2028-08-02)**, Chapter I·II의 2025-02-02 적용, 규정 전체 일반 적용일 2026-08-02. **미확인(확인 필요)** — ① Article 5 신설 조항의 2026-12-02 적용은 "specified new Article 5 provisions" 수준까지만 확인되었고 **개별 호 번호는 미확인**, ② 기존 출시 시스템의 Article 50 표시의무 유예의 **조항·문언**, ③ Article 50 적용일을 명시한 원문, ④ Chapter V가 개정되지 않았다는 점(확인된 개정 목록에 없다는 정황 근거만 있음), ⑤ 2025-08-02 이전 출시 GPAI 모델의 2027-08-02 경과규정(**원 AI Act Article 111(3)로 추정되나 미확인**). 위 미확인 항목은 EUR-Lex 통합본의 Article 111·113 전문을 직접 열람해 확정해야 한다.
