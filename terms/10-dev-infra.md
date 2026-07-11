# 10. 개발·인프라 기초 · Dev & Infra Basics

> AI를 내 컴퓨터에서 직접 돌리거나(로컬 실행) 서버에 올릴 때(셀프호스팅) 마주치는 일반 개발·운영 용어들. 개발자가 아니어도 이 말들만 알면 설치 안내문과 AI의 지시를 그대로 따라갈 수 있다.
> 출처 근거: 각 항목 하단 출처 및 [research/SOURCES.md](../research/SOURCES.md). 서식: [STYLE.md](../STYLE.md).

---

### SSH

> **한 줄 요약:** 멀리 있는 컴퓨터에 암호로 잠긴 안전한 통로를 뚫고 접속해, 내 컴퓨터 앞에 앉은 것처럼 명령을 내리는 방식.

**정의 (Definition)**
- KO: 신뢰할 수 없는 네트워크(예: 인터넷) 위에서도 원격 로그인과 그 밖의 네트워크 서비스를 안전하게 하도록 만든 프로토콜. 서버 인증·암호화·무결성 보호를 제공한다.
- EN: A protocol for secure remote login and other secure network services over an insecure network, providing server authentication, confidentiality, and integrity protection.

**비유 (쉽게):** 멀리 있는 사무실 컴퓨터에 전화로 지시하는데, 그 통화가 아무도 엿듣지 못하게 암호화된 전용 회선이라고 보면 된다. 회선 양끝은 서로가 진짜인지 열쇠(키)로 먼저 확인한 뒤에야 말을 주고받는다.

**왜 중요한가 / 언제 쓰나:**
- AI 모델을 돌리는 원격 서버(집의 다른 컴퓨터, 클라우드 임대 서버)에 접속해 명령을 내릴 때 표준으로 쓴다.
- 비밀번호 대신 **SSH 키**(공개키/개인키 한 쌍)로 로그인하면 더 안전하다 — 개인키는 절대 남에게 넘기지 않는다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 클라우드 서버에 SSH로 접속하는 명령을 알려줘 — 키 기반 로그인으로."
- "내 SSH 공개키를 서버에 등록해서, 다음부터 비밀번호 없이 접속되게 하는 절차를 설명해줘."

**흔한 오해:** SSH는 파일을 옮기는 도구가 "아니라" 안전한 접속·명령 통로다(그 통로 위에서 파일 전송 `scp`·`sftp`가 얹혀 돌 뿐이다). 또 개인키(private key)는 비밀번호처럼 다뤄야 한다 — 공개키(public key)만 서버에 올리고, 개인키를 넘겨달라는 요구는 정상적인 접속에 필요 없다.

**함께 보기:** [CLI](#cli--커맨드라인-인터페이스), [Port / localhost](#port--localhost--포트로컬호스트), [09-local-run.md](09-local-run.md)

**출처:** T. Ylonen & C. Lonvick (Ed.), *The Secure Shell (SSH) Protocol Architecture*, IETF **RFC 4251** (2006-01), [rfc-editor.org/rfc/rfc4251](https://www.rfc-editor.org/rfc/rfc4251). (프로토콜의 정본.)

---

### CLI · 커맨드라인 인터페이스

> **한 줄 요약:** 버튼을 누르는 대신, 글자로 된 명령을 타이핑해서 프로그램을 다루는 방식.

**정의 (Definition)**
- KO: 그래픽 화면(버튼·아이콘)이 아니라, 텍스트 명령을 한 줄씩 입력해 프로그램·운영체제를 조작하는 상호작용 방식. 명령을 해석·실행하는 프로그램(셸)이 그 창구가 된다.
- EN: A way of operating a program or operating system by typing text commands line by line, rather than through a graphical interface; a shell interprets and runs those commands.

**비유 (쉽게):** 식당에서 손가락으로 메뉴판 사진을 가리키는 대신(GUI), 종업원에게 "된장찌개 하나, 밥 곱빼기로" 하고 말로 정확히 주문하는 것과 같다. 익히는 데 조금 품이 들지만, 한 문장으로 복잡하고 정밀한 주문을 정확히 전달할 수 있다.

**왜 중요한가 / 언제 쓰나:**
- AI 코딩 도구(Claude Code, Codex, Gemini CLI 등)와 로컬 AI 실행 안내는 대부분 CLI 명령으로 진행된다 — "이 명령을 붙여넣으세요" 형태.
- 같은 작업을 반복·자동화(예: cron 예약 실행)하려면, 클릭보다 텍스트 명령이 그대로 재사용·기록되기 때문에 유리하다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 작업을 GUI 말고 CLI 명령으로 하는 방법을 한 줄씩 알려줘 — 각 명령이 무슨 일을 하는지 주석으로."
- "내가 붙여넣을 수 있게, 이 폴더의 파일 목록을 보는 커맨드라인 명령을 알려줘."

**흔한 오해:** CLI가 GUI보다 "더 어렵고 위험한 전문가 전용"이라는 인식은 절반만 맞다 — 진입 장벽은 있지만, 명령이 **글자로 남으므로 복사·검토·재현이 쉽다**는 장점이 있다. 다만 검증 없이 받은 명령을 그대로 붙여넣는 것은 위험하다(무슨 일을 하는지 먼저 확인).

**함께 보기:** [SSH](#ssh), [cron](#cron--크론), [Environment variable](#environment-variable--환경변수-env)

**출처:** The Open Group / IEEE, POSIX *Shell Command Language & Utilities*, **IEEE Std 1003.1-2017**, [pubs.opengroup.org](https://pubs.opengroup.org/onlinepubs/9699919799/). (단일 정본 없는 일반 개념 — 명령행 유틸리티의 권위 있는 표준 레퍼런스로 인용.)

---

### cron · 크론

> **한 줄 요약:** "매일 새벽 3시에 이 작업 실행"처럼, 정해둔 시각에 컴퓨터가 알아서 작업을 자동 실행하게 해주는 예약 장치.

**정의 (Definition)**
- KO: 유닉스 계열 운영체제에서, 지정한 시각·주기에 명령(작업)을 자동으로 반복 실행하도록 등록·관리하는 스케줄러. 등록표를 `crontab`이라 하고, 분·시·일·월·요일 다섯 칸으로 실행 시점을 지정한다.
- EN: A Unix scheduler that runs commands automatically at specified times or intervals; the schedule table (`crontab`) uses five time fields — minute, hour, day-of-month, month, day-of-week.

**비유 (쉽게):** 벽에 걸린 자동 타이머 콘센트와 같다. "저녁 7시에 전등 켜기"를 한 번 맞춰두면, 내가 없어도 매일 그 시각에 알아서 켜진다. cron은 전등 대신 "명령"을 그 시각에 켠다.

**왜 중요한가 / 언제 쓰나:**
- 매일·매주 반복되는 AI 작업(예: 뉴스 요약, 데이터 수집, 백업)을 사람이 지키고 앉지 않아도 자동으로 돌리고 싶을 때.
- 로컬·서버에서 돌아가는 스크립트를 정기적으로 깨워야 할 때의 표준 도구.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 스크립트를 매일 오전 9시에 자동 실행하는 crontab 항목을 만들어줘 — 다섯 칸 시간 표기가 뭘 뜻하는지 설명도."
- "매주 월요일 자정에만 도는 cron 작업으로 바꿔줘."

**흔한 오해:** cron은 "컴퓨터가 꺼져 있어도 나중에 몰아서 실행"해주지 않는다 — 예정 시각에 **기기가 켜져 있고 cron 서비스가 돌고 있어야** 실행된다(잠자기·종료 중 놓친 작업은 그냥 지나간다). 또 cron이 실행하는 작업의 실행 환경(PATH·환경변수)은 내 평소 터미널과 다를 수 있어, "손으로는 되는데 cron에선 안 되는" 일이 흔하다.

**함께 보기:** [CLI](#cli--커맨드라인-인터페이스), [Environment variable](#environment-variable--환경변수-env), [SSH](#ssh)

**출처:** The Open Group / IEEE, POSIX `crontab` 유틸리티, **IEEE Std 1003.1-2017**, [pubs.opengroup.org …/crontab.html](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/crontab.html); 보조: 시스템 매뉴얼 `man 5 crontab` / `man 8 cron`. (확인 — POSIX 표준 정의.)

---

### Docker / Container · 컨테이너

> **한 줄 요약:** 프로그램과 그게 돌아가는 데 필요한 모든 것을 하나의 밀봉 상자로 묶어, 어느 컴퓨터에서든 똑같이 실행되게 하는 방식.

**정의 (Definition)**
- KO: 애플리케이션과 그 실행에 필요한 라이브러리·설정을 격리된 하나의 단위(컨테이너)로 묶어, 호스트 환경 차이와 상관없이 동일하게 실행되게 하는 방식. Docker는 이를 대중화한 대표 도구이고, 컨테이너의 형식·실행 규격 자체는 개방형 표준(OCI)으로 정해져 있다.
- EN: Packaging an application together with its libraries and configuration into an isolated, runnable unit (a container) so it runs the same regardless of the host. Docker is the tool that popularized this; the container image and runtime formats themselves are defined by an open standard (OCI).

**비유 (쉽게):** 해외로 이사할 때 쓰는 규격 컨테이너 박스와 같다. 짐(앱)과 완충재·설명서(의존성·설정)를 한 상자에 밀봉해 두면, 배든 트럭이든(어느 컴퓨터든) 상자를 그대로 올리기만 하면 내용물이 똑같이 도착한다. "내 컴퓨터에선 되는데 네 컴퓨터에선 안 돼" 문제를 줄인다.

**왜 중요한가 / 언제 쓰나:**
- AI 모델·서버(예: 로컬 LLM, 벡터DB, MCP 서버)를 복잡한 설치 과정 없이 "이미지 하나 내려받아 실행"으로 띄우고 싶을 때.
- 내 컴퓨터·동료 컴퓨터·클라우드에서 **똑같은 환경**을 재현해야 할 때.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 AI 서버를 Docker 컨테이너로 띄우는 명령을 알려줘 — 포트는 8000번으로 열고."
- "이 프로젝트를 어디서든 똑같이 돌게 컨테이너로 묶는 설정을 만들어줘, 필요한 의존성까지 포함해서."

**흔한 오해:** 컨테이너는 가상머신(VM)과 다르다. VM은 가상의 컴퓨터 한 대를 통째로(각자 별도 운영체제까지) 흉내 내지만, **컨테이너는 호스트의 운영체제 커널을 공유**하고 앱과 그 의존성만 격리한다 — 그래서 훨씬 가볍고 빠르게 뜬다. 또 "Docker = 컨테이너"가 아니다: Docker는 대표 구현일 뿐, 규격은 개방형 표준 **OCI**라 다른 런타임(containerd, Podman 등)도 같은 컨테이너를 실행한다.

**함께 보기:** [Port / localhost](#port--localhost--포트로컬호스트), [Environment variable](#environment-variable--환경변수-env), [09-local-run.md](09-local-run.md), [03-building.md](03-building.md)

**출처:** Docker 공식 문서, *Docker overview*, [docs.docker.com](https://docs.docker.com/get-started/docker-overview/); 규격 표준: Open Container Initiative, *Runtime/Image Specification*, [opencontainers.org](https://opencontainers.org/). (확인 — Docker는 대표 도구, 컨테이너 규격의 정본은 OCI. 특정 벤더 전용 개념 아님.)

---

### API

> **한 줄 요약:** 프로그램끼리 "이렇게 요청하면 이런 답을 준다"는 약속으로 소통하는 창구.

**정의 (Definition)**
- KO: 한 프로그램이 다른 프로그램의 기능·데이터를 정해진 규칙에 따라 요청하고 받을 수 있게 해주는 인터페이스(창구). 특히 웹에서는 HTTP로 요청을 주고받는 REST API 형태가 흔하다.
- EN: An interface that lets one program request functionality or data from another according to an agreed set of rules; on the web this is often a REST API exchanging requests over HTTP.

**비유 (쉽게):** 식당의 주문 창구와 같다. 손님(내 프로그램)은 주방(상대 프로그램)에 직접 들어가지 않고, 정해진 메뉴판과 주문 방식(API)에 따라 "이걸 주세요" 하고 요청하면 정해진 형태로 결과를 받는다. 주방 내부가 어떻게 돌아가는지는 몰라도 된다.

**왜 중요한가 / 언제 쓰나:**
- AI 모델을 코드에서 부를 때(예: 클라우드 LLM 호출), 또는 내가 만든 서비스가 외부 데이터·도구와 연결될 때 그 통로가 곧 API다.
- 로컬에서 띄운 AI 서버도 보통 API(예: `http://localhost:8000`)로 접근한다 — AI에게 도구를 쥐여주는 방식(도구 사용·MCP)의 바탕이 된다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 서비스의 REST API를 호출해서 데이터를 가져오는 예제 코드를 만들어줘 — 인증 키는 코드에 박지 말고 환경변수로."
- "내가 만든 이 함수를 외부에서 부를 수 있게 간단한 API로 감싸줘."

**흔한 오해:** API는 특정 언어·회사의 물건이 아니라 "소통 규약(약속)"이다. 또 웹 API(REST/HTTP)만 API인 것도 아니다 — 라이브러리 함수 호출 규격도 API다. AI 맥락에서 흔히 "API"라고만 하면 대개 **웹(HTTP) API**를 가리키지만, 개념 자체는 더 넓다.

**함께 보기:** [Port / localhost](#port--localhost--포트로컬호스트), [03-building.md](03-building.md)

**출처:** 단일 정본 없는 일반 개념. 권위 있는 레퍼런스로 REST: R. Fielding, *Architectural Styles and the Design of Network-based Software Architectures* (2000, 박사학위논문 5장), [ics.uci.edu/~fielding/pubs/dissertation](https://ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm); HTTP 시맨틱스: IETF **RFC 9110** (2022), [rfc-editor.org/rfc/rfc9110](https://www.rfc-editor.org/rfc/rfc9110). (유일 창시 아님 — 대표 권위 출처.)

---

### Port / localhost · 포트/로컬호스트

> **한 줄 요약:** 한 컴퓨터 안에서 여러 서비스를 구분하는 번호(포트)와, "바로 이 컴퓨터 자신"을 가리키는 주소(localhost).

**정의 (Definition)**
- KO: **포트**는 한 기기의 IP 주소로 들어온 통신을 어느 서비스로 보낼지 구분하는 16비트 번호(0–65535)다. **localhost**(및 `127.0.0.1`)는 지금 이 컴퓨터 자기 자신을 가리키는 특수 주소(루프백)로, 바깥 네트워크로 나가지 않는다.
- EN: A **port** is a 16-bit number (0–65535) that distinguishes which service on a host incoming traffic goes to; **localhost** (and `127.0.0.1`) is a special loopback address meaning "this same machine," which never leaves the local host.

**비유 (쉽게):** 큰 건물(컴퓨터)에 주소(IP)는 하나지만, 안에는 수많은 사무실이 있다. **포트 번호는 그 건물 안 호실 번호**여서, 우편물(통신)이 정확한 사무실로 배달되게 한다. **localhost는 "이 건물 안에서만 도는 내부 우편"** — 편지가 건물 밖으로 안 나가고 나 자신에게 돌아온다.

**왜 중요한가 / 언제 쓰나:**
- 로컬에서 AI 서버를 띄우면 보통 `http://localhost:8000`처럼 접속한다 — 8000이 포트, localhost가 "내 컴퓨터"라는 뜻.
- 서비스 두 개를 같은 포트에 띄우면 충돌하므로, 포트가 겹치는지(이미 쓰는 중인지) 확인해야 한다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 모델 서버를 localhost의 다른 포트(예: 8080)로 띄워줘 — 8000번이 이미 쓰이고 있어."
- "지금 어떤 프로그램이 8000번 포트를 점유하고 있는지 확인하는 명령을 알려줘."

**흔한 오해:** `localhost`에 띄운 서비스는 기본적으로 **내 컴퓨터에서만 접근**된다 — 같은 네트워크의 다른 기기에서 그 주소로는 못 들어온다(그래서 로컬 테스트엔 안전하지만, 남에게 열어주려면 별도 설정이 필요하다). 또 1024번 미만의 낮은 포트(시스템 포트)는 보통 관리자 권한이 있어야 열 수 있다.

**함께 보기:** [Docker / Container](#docker--container--컨테이너), [API](#api), [09-local-run.md](09-local-run.md)

**출처:** 포트: M. Cotton et al., *IANA Procedures for … Port Number Registry*, IETF **RFC 6335** (2011-08), [rfc-editor.org/rfc/rfc6335](https://www.rfc-editor.org/rfc/rfc6335); localhost 특수도메인: **RFC 6761** (2013-02); 루프백 `127.0.0.0/8`: **RFC 1122** §3.2.1.3. (일반 네트워킹 개념 — 해당 RFC가 정본.)

---

### Environment variable · 환경변수 (.env)

> **한 줄 요약:** 코드 바깥에서 프로그램에 설정값·비밀키를 넣어주는 "값 주머니". 흔히 `.env` 파일에 적어둔다.

**정의 (Definition)**
- KO: 프로그램이 실행될 때 운영체제·실행 환경으로부터 전달받는, 이름과 값의 쌍으로 된 설정 변수. 코드에 직접 박지 않고 바깥에서 주입하므로, 환경(내 컴퓨터·서버)마다 다른 값을 코드 수정 없이 바꿀 수 있다.
- EN: A name–value setting passed to a program from its operating system / runtime environment, injected from outside rather than hard-coded, so each environment can supply different values without changing the code.

**비유 (쉽게):** 가전제품을 나라마다 다른 전압에 맞추는 것과 같다. 제품(코드) 자체를 뜯어고치지 않고, 그 나라 콘센트(환경)가 주는 전압(값)을 꽂아 쓴다. 비밀번호 같은 민감한 값도 제품 설명서에 인쇄하지 않고, 쓸 때 바깥에서 꽂아 넣는다.

**왜 중요한가 / 언제 쓰나:**
- AI 서비스의 **API 키·토큰·비밀번호**를 코드에 적지 않고 환경변수(대개 `.env` 파일)로 주입할 때 — 보안의 기본.
- 같은 코드를 로컬·서버에서 서로 다른 설정으로 돌려야 할 때, 값만 환경변수로 갈아 끼운다.

**실무 예시 / AI에게 이렇게 말한다:**
- "API 키를 코드에 하드코딩하지 말고 환경변수(`.env`)에서 읽어오게 바꿔줘."
- "이 프로젝트의 `.env` 예시 파일(`.env.example`)을 만들어줘 — 실제 키는 비우고 항목 이름만."

**흔한 오해 / 모범관행:** 비밀키는 코드나 저장소(Git)에 절대 넣지 말고 `.env` 같은 환경변수에 두며, `.env` 파일 자체도 저장소에 커밋하지 않는다(`.gitignore`로 제외). Twelve-Factor App의 기준처럼 "코드베이스를 언제 공개해도 자격증명이 새지 않아야" 한다. 다만 `.env`는 하나의 관례일 뿐 표준 규격이 아니며, 값은 실행 환경으로 로드되어야 실제 환경변수가 된다. (Michael 운영지침과도 일치: API 키는 문서·Git이 아니라 `.env`·MCP 설정에만.)

**함께 보기:** [cron](#cron--크론), [Docker / Container](#docker--container--컨테이너), [SSH](#ssh)

**출처:** POSIX *Environment Variables*, **IEEE Std 1003.1-2017** (Base Definitions ch. 8), [pubs.opengroup.org](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap08.html); 모범관행: *The Twelve-Factor App* — III. Config, [12factor.net/config](https://12factor.net/config). (`.env`는 관례이며 단일 정본 없음 — 개념은 POSIX, 비밀키 분리 관행은 Twelve-Factor.)
