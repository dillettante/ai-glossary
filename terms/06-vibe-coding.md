# 6. 바이브코딩 워크플로우 · Vibe-Coding Workflow

> AI 코딩 도구와 협업할 때 쓰는 작업 동사들. 이 말들을 알면 AI에게 원하는 작업을 정확히 시킬 수 있다.
> 출처 근거: [research/SOURCES.md](../research/SOURCES.md) 카테고리 6. 서식: [STYLE.md](../STYLE.md).

---

### Vendoring · 벤더링

> **한 줄 요약:** 외부 라이브러리의 소스 복사본을 내 프로젝트 폴더 안에 넣어두고, 그대로 함께 관리하는 것.

**정의 (Definition)**
- KO: 외부 의존성의 소스 복사본을 프로젝트 안(보통 `vendor/` 폴더)에 넣어 저장소에 함께 커밋함으로써, 빌드 재현성과 외부 인터넷·패키지 서버에 대한 독립성을 고정하는 방식.
- EN: Copying a project's external dependencies into a `vendor/` directory checked into the repository, so builds are reproducible and independent of external package servers.

**비유 (쉽게):** 요리할 때마다 옆집에서 소금을 빌려오는 대신, 소금 한 봉지를 통째로 내 부엌 서랍에 복사해 둔다. 옆집이 이사를 가도(패키지 서버가 내려가도) 내 요리는 그대로 된다.

**왜 중요한가 / 언제 쓰나:**
- 외부 패키지 서버가 사라지거나 버전이 바뀌어도 빌드가 깨지지 않게 하고 싶을 때.
- 보안 심사·오프라인 빌드처럼, 의존성 소스가 저장소 안에 물리적으로 있어야 할 때.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 프로젝트의 의존성을 `vendor/` 폴더에 벤더링해줘. 소스를 저장소에 커밋해서 오프라인에서도 빌드되게."
- "이 라이브러리는 설치만 하지 말고 벤더링해서, 업스트림이 내려가도 재현되게 고정해줘."

**흔한 오해:** 벤더링은 그냥 설치(install)와 다르다. 설치는 별도 캐시·시스템 폴더에 두지만, 벤더링은 **소스 복사본 자체를 저장소에 커밋**한다. 또 Go 전용 기법이 아니다 — Go 모듈 레퍼런스가 이 용어를 널리 알린 대표 사례이지만, 그 이전부터 Ruby(Bundler)·PHP(Composer) 등 여러 생태계에 같은 개념이 있었다.

**함께 보기:** [Modularization](#modularization--모듈화), [Scaffolding](#scaffolding--스캐폴딩)

**출처:** Go 공식 모듈 레퍼런스, [go.dev/ref/mod](https://go.dev/ref/mod). (Go 전용 개념이 아니며 Ruby·PHP 등 선례가 있음 — 유일 창시 아님.)

---

### Porting · 포팅

> **한 줄 요약:** 한 환경(OS·CPU·언어·프레임워크)에서 돌던 프로그램을 다른 환경에서도 돌게 고쳐 옮기는 일.

**정의 (Definition)**
- KO: 어떤 실행 환경(운영체제·CPU 아키텍처·프로그래밍 언어·프레임워크)을 전제로 만든 소프트웨어를, 동작을 보존하면서 다른 환경에서도 돌아가도록 코드를 수정해 옮기는 작업.
- EN: Adapting software built for one environment (OS, CPU architecture, language, or framework) so that it runs in a different one, preserving its behavior while changing the environment-dependent code.

**비유 (쉽게):** 콘솔 전용으로 나온 게임을 휴대폰에서도 되게 고치는 일. 게임 내용(동작)은 같지만, 조작 방식·화면 크기처럼 환경이 다른 부분은 다시 손봐야 한다.

**왜 중요한가 / 언제 쓰나:**
- 잘 돌던 코드를 새 플랫폼(다른 OS, 다른 언어, 웹→모바일 등)으로 옮겨야 할 때.
- 오래된 환경에 묶인 레거시 코드를 최신 환경에서 살려야 할 때.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 Python 스크립트를 동작 그대로 유지하면서 Go로 포팅해줘."
- "이 리눅스 전용 코드를 macOS에서도 빌드되게 포팅해줘 — 경로·시스템 콜만 환경에 맞게 고치고."

**흔한 오해:** 포팅은 파일을 복붙해서 그냥 실행하는 게 아니다. **동작은 보존하되, 환경이 다른 부분의 코드를 실제로 수정**해야 옮겨진다.

**함께 보기:** [Refactoring](#refactoring--리팩터링), [Wrapping](#wrapping--래핑)

**출처:** ⚠️ 위키백과 "Porting" 항목(현재 앵커). 바이블 품질 기준상 **IEEE 소프트웨어 공학 용어집으로 격상 권장** — 현 단계에서는 정본 미확정 상태임을 밝혀 둔다.

---

### Refactoring · 리팩터링

> **한 줄 요약:** 겉으로 드러나는 동작은 그대로 두고, 내부 구조만 이해·수정하기 쉽게 다듬는 것.

**정의 (Definition)**
- KO: 소프트웨어의 겉보기 동작(observable behavior)은 바꾸지 않으면서, 내부 구조를 더 이해하고 고치기 쉽게 개선하는 작업.
- EN: A change made to the internal structure of software to make it easier to understand and cheaper to modify without changing its observable behavior. (Fowler)

**비유 (쉽게):** 어질러진 방을 청소·정리하는 일. 방 안의 물건(기능)은 그대로 있지만, 어디에 뭐가 있는지 찾기 쉽게 배치를 바꾼다. 물건을 새로 사거나(기능 추가) 버리는(동작 변경) 게 아니다.

**왜 중요한가 / 언제 쓰나:**
- 코드가 돌긴 하는데 지저분해서 다음 수정이 두려울 때, 손대기 쉬운 상태로 먼저 정리한다.
- 기능 추가 전에 바닥을 고르게 다져, 이후 변경 비용을 낮춘다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 함수의 동작은 그대로 두고, 중복을 없애고 이름을 명확히 리팩터링해줘."
- "이 클래스를 리팩터링해줘 — 겉보기 동작은 절대 바꾸지 말고, 테스트가 계속 통과하게."

**흔한 오해:** 리팩터링은 기능 추가나 버그 수정이 아니다. **겉보기 동작(observable behavior)이 바뀌면 그건 이미 리팩터링이 아니다.** "고치는 김에 기능도 조금 바꿨다"면 그건 다른 작업이 섞인 것이다.

**함께 보기:** [Porting](#porting--포팅), [Modularization](#modularization--모듈화)

**출처:** Martin Fowler, *Refactoring* — [refactoring.com](https://refactoring.com/). (이 용어의 정본.)

---

### Scaffolding · 스캐폴딩

> **한 줄 요약:** 명령 한 번으로 기본 뼈대 파일·보일러플레이트를 자동 생성해, 개발의 출발점을 깔아주는 것.

**정의 (Definition)**
- KO: 애플리케이션의 기본 구조 파일과 보일러플레이트 코드를 명령 한 번으로 자동 생성해, 곧바로 개발을 시작할 수 있는 출발용 뼈대를 만들어 주는 방식.
- EN: Automatically generating the basic structural files and boilerplate of an application with a single command, giving developers a starting skeleton to build on.

**비유 (쉽게):** 건물 공사장의 비계(scaffold) — 완성된 건물이 아니라, 그 위에 서서 진짜 건물을 올리기 위한 임시 발판이다. 색칠공부의 밑그림처럼, 채워 넣을 자리를 미리 잡아준다.

**왜 중요한가 / 언제 쓰나:**
- 새 프로젝트·새 기능을 맨땅에서 시작할 때, 반복되는 기본 파일 구조를 손으로 짜는 수고를 던다.
- 팀이 같은 뼈대에서 출발하게 해 구조 일관성을 확보한다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 리소스에 대한 CRUD 스캐폴딩을 만들어줘 — 모델·컨트롤러·기본 뷰까지 뼈대만."
- "새 API 프로젝트의 폴더 구조와 보일러플레이트를 스캐폴딩해줘. 로직은 비워두고 뼈대만."

**흔한 오해:** 스캐폴딩 결과물은 완성품이 아니다. **출발용 뼈대일 뿐**, 실제 로직은 그 위에 직접 채워야 한다. 또 Rails 전용 기능이 아니다 — Ruby on Rails가 이 말을 대중화했지만 여러 프레임워크에 있는 범용 개념이다.

**함께 보기:** [Vendoring](#vendoring--벤더링), [PoC / MVP](07-dev-stages.md)

**출처:** Ruby on Rails 공식 가이드, [guides.rubyonrails.org](https://guides.rubyonrails.org/). (Rails가 대중화한 범용 개념 — 유일 창시 아님.)

---

### Wrapping · 래핑

> **한 줄 요약:** 기존 코드를 바깥 껍데기로 감싸, 원본은 그대로 둔 채 다른 인터페이스처럼 쓰거나 더 쉽게 쓰게 하는 것.

**정의 (Definition)**
- KO: 기존 코드·객체를 새로운 껍데기(wrapper)로 감싸, 원본을 고치지 않고도 다른 인터페이스로 보이게 하거나 더 편하게 쓸 수 있게 하는 방식. GoF 디자인 패턴의 Adapter는 별칭이 "Wrapper"다.
- EN: Enclosing existing code or an object in a new outer layer (a "wrapper") so it can be used through a different interface — without modifying the original. GoF's Adapter pattern is also known as "Wrapper".

**비유 (쉽게):** 해외여행용 전원 어댑터. 벽 콘센트(원본)를 뜯어고치지 않고, 그 위에 끼우는 껍데기 하나로 내 플러그가 맞게 만든다. 안쪽 전기는 그대로다.

**왜 중요한가 / 언제 쓰나:**
- 고칠 수 없는(혹은 고치고 싶지 않은) 외부 라이브러리·레거시 코드를, 내 코드가 원하는 형태로 쓰고 싶을 때.
- 서로 안 맞는 두 인터페이스를 껍데기 하나로 이어붙일 때.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 외부 SDK를 우리 인터페이스에 맞게 래핑해줘 — 원본은 건드리지 말고 어댑터만 씌워서."
- "이 옛날 함수를 감싸는 래퍼를 만들어줘. 호출부는 새 시그니처로 쓰고, 안에서 원본을 그대로 호출하게."

**흔한 오해:** 래핑은 원본을 고치는 게 아니다. **바깥 껍데기만 새로 씌우고, 안쪽 원본은 그대로 둔다.** 그래서 원본을 수정할 수 없는 상황에서도 쓸 수 있다.

**함께 보기:** [Modularization](#modularization--모듈화), [Porting](#porting--포팅)

**출처:** Gamma et al. (GoF), *Design Patterns* (1994) — Adapter 패턴, 별칭 "Wrapper". ⚠️ GoF 원서 해당 페이지는 직접 미열람, refactoring.guru로 개념 검증함(정본 페이지 대조는 후속 과제).

---

### Modularization · 모듈화

> **한 줄 요약:** 큰 프로그램을 책임(관심사)별 독립 모듈로 쪼개고, 정해진 인터페이스로만 서로 소통하게 하는 것.

**정의 (Definition)**
- KO: 하나의 큰 프로그램을 각자 맡은 책임이 분명한 독립 모듈들로 나누고, 모듈끼리는 내부를 숨긴 채 정해진 인터페이스로만 소통하게 설계하는 방식(관심사 분리).
- EN: Dividing a large program into independent modules, each with a clear responsibility, that communicate only through defined interfaces while hiding their internals (separation of concerns).

**비유 (쉽게):** 레고. 표준 규격의 블록들로 나눠 두면, 블록 하나를 다른 것으로 갈아 끼우거나 다른 작품에 재사용할 수 있다. 중요한 건 "몇 조각으로 나눴나"가 아니라 **끼우는 자리(인터페이스)가 표준화돼 있다**는 점이다.

**왜 중요한가 / 언제 쓰나:**
- 코드가 커져서 한 곳을 고치면 엉뚱한 데가 깨질 때, 책임 경계를 그어 서로 영향을 줄인다.
- 부분을 독립적으로 개발·테스트·교체·재사용하고 싶을 때.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 덩어리 코드를 관심사별로 모듈화해줘 — 데이터 접근·비즈니스 로직·표시를 경계로 나눠서."
- "이 모듈이 다른 모듈의 내부에 직접 의존하지 않게, 인터페이스로만 소통하도록 모듈화해줘."

**흔한 오해:** 모듈화는 그냥 "파일 여러 개로 쪼개기"가 아니다. **핵심은 관심사 분리와 경계 설정** — 파일만 나누고 서로의 속을 훤히 들여다보며 얽혀 있으면 모듈화가 된 게 아니다.

**함께 보기:** [Wrapping](#wrapping--래핑), [Vendoring](#vendoring--벤더링), [Refactoring](#refactoring--리팩터링)

**출처:** ⚠️ 위키백과 "Modular programming" 항목(현재 앵커). 바이블 품질 기준상 정본인 **Parnas (1972), CACM "On the Criteria To Be Used in Decomposing Systems into Modules"로 격상 권장** — 현 단계에서는 정본 미확정 상태임을 밝혀 둔다.
