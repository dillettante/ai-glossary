# 7. 개발 단계 · 품질 · Development Stages & Quality

> 아이디어가 실제 서비스가 되기까지의 단계 이름들. "되는지 확인(PoC) → 최소로 내놓기(MVP) → 운영급으로 다듬기(Production Ready)".
> 출처 근거: [research/SOURCES.md](../research/SOURCES.md) 카테고리 7. 서식: [STYLE.md](../STYLE.md).

---

### MVP · 최소 기능 제품 (Minimum Viable Product)

> **한 줄 요약:** 가장 적은 노력으로 "사람들이 이걸 원하나"를 배우게 해주는, 출시 가능한 최소한의 제품.

**정의 (Definition)**
- KO: 가장 적은 노력으로 고객에 대한 검증된 학습(validated learning)을 최대한 얻어낼 수 있는, 출시 가능한 최소한의 제품 버전.
- EN: The version of a new product which allows a team to collect the maximum amount of validated learning about customers with the least effort. (Ries)

**비유 (쉽게):** 목적이 "이동"이라면, 완성된 자동차를 만들기 전에 먼저 **킥보드**부터 내놓아 "사람들이 이걸로 이동하려 하나"를 배운다. 바퀴 하나만 덜렁 주는 게 아니라, 작아도 **실제로 굴러가는** 것을 준다.

**왜 중요한가 / 언제 쓰나:**
- 완제품을 다 만들기 전에, 실제 사용자로 아이디어·시장 가설을 빠르게 검증해 낭비를 줄인다.
- 핵심 가치 하나에 집중하고 부가 기능은 미룬다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 아이디어의 MVP를 만들어줘 — 핵심 기능 하나만, 실제 사용자가 쓸 수 있게."
- "부가 기능은 빼고, 가입 → 핵심 가치 경험까지만 되는 MVP로 좁혀줘."

**흔한 오해:** MVP는 조잡하게 대충 만든 반쪽 제품이 아니다. 최소지만 **실제로 가치를 주고 학습을 얻을 만큼은 '작동(viable)'해야** 한다. 또 [PoC](07-dev-stages.md)와 달리 **고객에게 실제로 내놓는다**(PoC는 기술 검증용 실험으로 대개 검증 뒤 폐기한다).

**함께 보기:** [PoC](07-dev-stages.md), [Production Ready](07-dev-stages.md)

**출처:** Eric Ries, *The Lean Startup* — 원문 정의: [startuplessonslearned.com (2009)](https://www.startuplessonslearned.com/2009/08/minimum-viable-product-guide.html). (용어 대중화는 Ries, 초기 개념은 Frank Robinson(2001) — 유일 창시 아님.)

---

### PoC · 개념 증명 (Proof of Concept)

> **한 줄 요약:** "이게 기술적으로 되긴 하나?"만 딱 확인하는, 버리기 위해 만드는 소규모 검증 실험.

**정의 (Definition)**
- KO: 어떤 아이디어·기술이 실제로 구현 가능한지(기술적 실현가능성)만 빠르게 확인하려고 만드는 소규모 검증 실험(대개 검증 뒤에는 폐기한다).
- EN: A small, throwaway experiment built to verify only whether an idea or technology is technically feasible — not to ship a product.

**비유 (쉽게):** 새 요리를 손님상에 정식으로 내기 전에, 딱 **한 숟갈만** 만들어보고 "맛이 나긴 하나"를 맛본다. 손님에게 내놓으려는 게 아니라, **되는지만** 확인하고 그 한 숟갈은 버린다.

**왜 중요한가 / 언제 쓰나:**
- 큰 비용을 들이기 전에 "이 접근이 아예 불가능한 건 아닌지"를 최소 노력으로 가려낸다.
- 기술 리스크가 큰 아이디어에서, 본격 개발 착수 여부를 결정하는 근거로 쓴다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 방식이 되는지만 확인하는 PoC를 짜줘 — 예쁠 필요 없고, 핵심 동작 하나만 증명되면 돼."
- "이 API로 원하는 데이터가 실제로 나오는지 PoC로 빠르게 검증해줘."

**흔한 오해:** PoC는 **제품의 첫 버전이 아니다.** 오직 "기술적으로 실현 가능한가"라는 질문에만 답하는 실험이며, [MVP](07-dev-stages.md)와 달리 **고객에게 내놓지 않고** 검증이 끝나면 대개 폐기한다.

**함께 보기:** [MVP](07-dev-stages.md), [Production Ready](07-dev-stages.md)

**출처:** 단일 창시 정본이 없는 업계·학술 통용어로, 대표 서술은 [Wikipedia, *Proof of concept*](https://en.wikipedia.org/wiki/Proof_of_concept) 등 표준 정의를 따른다(특정 논문·벤더의 유일 창시 아님).

---

### Production Ready · 프로덕션 레디 (Production Readiness)

> **한 줄 요약:** "내 컴퓨터에선 잘 돌아요"를 넘어, 실제 사용자 트래픽에 안전하게 올릴 수 있는 상태.

**정의 (Definition)**
- KO: 실제 운영 트래픽을 받아도 안전하게 서비스할 수 있는 상태 — 모니터링(관측성), 장애 대응·복구, 용량(확장), 보안, 변경 관리가 갖춰진 수준.
- EN: The state in which a system can be safely operated under real production traffic — with monitoring, incident response, capacity, security, and change management in place.

**비유 (쉽게):** 놀이공원이 새 롤러코스터에 **손님을 태우기 전**에 거치는 안전 점검을 통과한 상태. 시운전으로 한 번 굴러갔다고 태우는 게 아니라, 제동·하중·비상정지까지 점검 목록을 모두 통과해야 손님을 태운다.

**왜 중요한가 / 언제 쓰나:**
- 로컬에서 동작하는 것과, 수많은 사용자·장애·트래픽 급증을 견디는 것은 전혀 다른 문제다.
- 출시(런칭) 승인 게이트로 쓴다 — 무엇이 준비됐고 무엇이 안 됐는지 점검 목록으로 확인한다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 서비스를 프로덕션에 올리기 전 체크리스트를 만들어줘 — 모니터링·장애 복구·용량·보안·변경 관리 관점으로."
- "지금 코드가 프로덕션 레디인지, 관측성과 장애 대응 관점에서 빠진 걸 짚어줘."

**흔한 오해:** **"로컬에서 잘 돎"은 프로덕션 레디가 아니다.** 관측성·장애 복구·용량·보안·변경 관리가 함께 갖춰져야 한다. 또한 단 하나의 "정의 문장"이 있는 게 아니라, 구글 SRE의 **프로덕션 준비도 검토(PRR, Production Readiness Review) 체크리스트**가 사실상의 표준 역할을 한다(특정 논문의 유일 창시 개념이 아님).

**함께 보기:** [PoC](07-dev-stages.md), [MVP](07-dev-stages.md)

**출처:** Google, *Site Reliability Engineering* — Production Readiness Review: [sre.google/sre-book](https://sre.google/sre-book/evolving-sre-engagement-model/). (특정 벤더의 실무 표준이며 유일 창시 정의가 아님.)
