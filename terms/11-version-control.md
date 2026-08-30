# 11. 버전 관리·협업 · Version Control & Collaboration

> 코드를 여럿이 함께 고치고 이력을 관리하는 도구·개념. AI 코딩·바이브코딩을 하다 보면 매일 만나는 말들이다. 개발자가 아니어도 이 말들만 알면 "새 브랜치에 커밋하고 PR 올려줘" 같은 AI의 지시를 그대로 따라갈 수 있다.
> 출처 근거: 각 항목 하단 출처 및 [research/SOURCES.md](../research/SOURCES.md). 서식: [STYLE.md](../STYLE.md).

---

### CommonMark / GFM · 마크다운 표준과 GitHub 확장

> **한 줄 요약:** 마크다운에는 표준(CommonMark)이 있고, GitHub이 표·체크박스·취소선을 더한 확장(GFM)이 있다. 렌더러마다 결과가 달라지는 이유가 여기 있다.

**정의 (Definition)**
- KO: **CommonMark**는 마크다운의 동작을 엄밀히 규정한 명세. **GFM(GitHub Flavored Markdown)**은 GitHub.com의 사용자 콘텐츠에서 지원되는 마크다운 방언으로, **CommonMark의 엄격한 상위집합(strict superset)**이다.
- EN: "GitHub Flavored Markdown, often shortened as GFM, is the dialect of Markdown that is currently supported for user content on GitHub.com and GitHub Enterprise." — "GFM is a strict superset of CommonMark."

**비유 (쉽게):** **표준어와 지역 방언**이다. 방언을 쓰는 사람은 표준어를 다 알아듣지만, 표준어만 아는 사람은 방언의 일부 표현을 못 알아듣는다. 표(table)와 체크박스가 그 방언 표현이다.

**왜 중요한가 / 언제 쓰나:**
- **한 곳에서 잘 보이던 문서가 다른 곳에서 깨지는 원인**이다. 표·체크박스·취소선은 CommonMark 표준이 아니라 GFM 확장이다.
- GFM이 CommonMark에 더한 확장은 다섯 가지 — **표, 취소선, 작업 목록(체크박스), 자동 링크 확장, 위험한 원시 HTML 차단**.
- 볼트·문서를 마크다운으로 운영한다면, **어느 방언을 전제하는지**가 곧 이식 가능성이다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 문서를 표준 CommonMark만 쓰도록 고쳐줘. 표는 유지해야 하니 어디가 GFM 확장인지 표시해줘."

**흔한 오해:**
- **"마크다운은 하나다"** — 아니다. 표준(CommonMark)과 여러 방언이 있고, 렌더러마다 지원 범위가 다르다.
- **"GFM은 마크다운을 바꾼 것"** — 상위집합이라 CommonMark 문서는 GFM에서 그대로 동작한다. 반대 방향이 안 될 뿐이다.
- **"표는 마크다운 기본 기능"** — GFM 확장이다.

**함께 보기:** [Repository (repo)](11-version-control.md#repository-repo--리포지터리저장소), [Git vs GitHub](11-version-control.md#git-vs-github--깃-vs-깃허브)

**출처:** GitHub, *GitHub Flavored Markdown Spec*, [github.github.com/gfm](https://github.github.com/gfm/) ("GitHub Flavored Markdown, often shortened as GFM, is the dialect of Markdown that is currently supported for user content on GitHub.com and GitHub Enterprise."; "GFM is a strict superset of CommonMark."; 확장 5종; 확인 2026-08-27). (공식 명세.)

---

### Git vs GitHub · 깃 vs 깃허브

> **한 줄 요약:** Git은 변경 이력을 관리하는 "도구"이고, GitHub은 그 결과물을 인터넷에 올려 남과 함께 쓰는 "장소"다 — 둘은 다른 것이다.

**정의 (Definition)**
- KO: **Git**은 빠르고 분산된(각자 컴퓨터에 전체 이력을 두는) 버전 관리 시스템으로, 2005년 리누스 토르발스가 리눅스 커널 개발을 위해 만든 도구 자체다. **GitHub**은 그 Git 저장소를 온라인에 올려두고 여럿이 협업하도록 해주는 호스팅 서비스(현재 마이크로소프트 소유)다.
- EN: **Git** is a fast, distributed version-control system — the tool itself, created by Linus Torvalds in 2005 for Linux kernel development. **GitHub** is a hosting service that stores Git repositories online and lets people collaborate on them (owned by Microsoft).

**비유 (쉽게):** Git은 문서에 "변경 내용 추적"을 켜고 이력을 남기는 워드프로세서의 기능 자체이고, GitHub은 그렇게 관리한 문서를 여럿이 함께 보고 고치도록 올려두는 공유 드라이브다. 워드가 없어도 드라이브는 존재하듯, Git과 GitHub은 별개다.

**왜 중요한가 / 언제 쓰나:**
- AI 코딩 도구가 만든 결과를 이력으로 남기고(Git), 남과 공유하거나 백업할 때(GitHub) 바탕이 된다.
- GitHub 말고도 GitLab·Bitbucket 등 같은 역할의 대안 서비스가 여럿 있다 — GitHub은 그중 가장 널리 쓰이는 하나일 뿐이다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 폴더를 git 저장소로 초기화하고, 지금 상태를 첫 커밋으로 남겨줘."
- "이 프로젝트를 GitHub에 새 저장소로 올리는 절차를 알려줘 — git 자체와 GitHub이 각각 뭘 하는지 구분해서."

**흔한 오해:** "Git = GitHub"이 가장 흔한 오해다. **Git은 인터넷 없이 내 노트북 안에서도 완전히 돌아가는 도구**이고, GitHub은 그 저장소를 온라인에 올려 공유하는 여러 서비스 중 하나다(Git ≠ GitHub). Git이 GitHub의 소유물도 아니다 — Git은 오픈소스이고, GitHub은 이를 호스팅하는 별개 회사 서비스다.

**함께 보기:** [Repository (repo)](#repository-repo--리포지터리저장소), [Clone / Pull / Push](#clone--pull--push--클론풀푸시), [CLI](10-dev-infra.md#cli--커맨드라인-인터페이스-command-line-interface)

**출처:** Scott Chacon & Ben Straub, *Pro Git* (2판) — "What is Git?" 및 "A Short History of Git", [git-scm.com/book](https://git-scm.com/book/en/v2/Getting-Started-A-Short-History-of-Git); git 매뉴얼, [git-scm.com/docs/git](https://git-scm.com/docs/git); GitHub 소유권: Microsoft, *Microsoft to acquire GitHub for $7.5 billion* (2018-06-04), [news.microsoft.com](https://news.microsoft.com/source/2018/06/04/microsoft-to-acquire-github-for-7-5-billion/). (확인 — Git은 Torvalds가 2005년 창시, 단일 도구; GitHub은 별개 상용 서비스.)

---

### Repository (repo) · 리포지터리(저장소)

> **한 줄 요약:** 프로젝트의 파일 전체와 그동안의 모든 변경 이력을 함께 담아두는 저장 단위(=프로젝트 폴더 + 그 역사).

**정의 (Definition)**
- KO: 한 프로젝트에 속한 파일들과, 그 파일들이 시간에 따라 어떻게 바뀌어 왔는지(커밋 이력) 전체를 담는 저장 단위. 흔히 "레포"라 줄여 부른다.
- EN: A storage unit holding a project's files together with the complete history of how those files have changed over time (its commits); commonly shortened to "repo."

**비유 (쉽게):** 서류철 한 권과 같다. 그 안에는 현재 문서들만 있는 게 아니라, "언제 누가 무엇을 어떻게 고쳤는지"를 적은 두툼한 변경 기록까지 함께 철해져 있다. 폴더 하나가 곧 그 프로젝트의 과거·현재를 통째로 담은 상자다.

**왜 중요한가 / 언제 쓰나:**
- AI 코딩·바이브코딩에서 작업의 기본 단위는 언제나 하나의 저장소다 — "이 레포를 봐줘", "새 레포를 만들어줘"처럼 쓴다.
- 저장소를 통째로 복제(clone)하면 파일뿐 아니라 전체 이력까지 함께 받아온다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 저장소의 폴더 구조를 훑어보고, 어떤 프로젝트인지 요약해줘."
- "새 저장소를 만들고 이 파일들을 첫 커밋으로 넣어줘."

**흔한 오해:** 저장소는 "현재 파일이 담긴 폴더"만이 아니다 — **변경 이력 전체**가 함께 들어 있는 것이 핵심이다(그래서 과거 어느 시점으로도 되돌아갈 수 있다). 또 저장소가 반드시 GitHub 같은 온라인에 있어야 하는 것도 아니다 — 내 컴퓨터 안의 로컬 저장소만으로도 완결된다.

**함께 보기:** [Git vs GitHub](#git-vs-github--깃-vs-깃허브), [Commit](#commit--커밋), [Clone / Pull / Push](#clone--pull--push--클론풀푸시)

**출처:** Scott Chacon & Ben Straub, *Pro Git* (2판) — "Getting a Git Repository", [git-scm.com/book](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository). (확인 — Git 공식 서적 정의.)

---

### Commit · 커밋

> **한 줄 요약:** 지금까지 고친 내용을 "이 시점"으로 이력에 남기는 저장 지점 — 설명 메시지를 붙인 스냅샷(사진 한 장).

**정의 (Definition)**
- KO: 저장소의 현재 상태를 하나의 스냅샷으로 이력에 기록하는 동작(및 그렇게 기록된 지점). 무엇을 왜 바꿨는지 적은 커밋 메시지가 함께 저장된다.
- EN: The act (and the resulting point) of recording the repository's current state as a snapshot in its history, saved together with a message describing what changed and why.

**비유 (쉽게):** 게임의 "세이브 지점"과 같다. 여기까지 진행한 상태를 이름표(메시지)를 붙여 저장해 두면, 나중에 무언가 잘못돼도 그 지점으로 언제든 돌아올 수 있다. 커밋을 자주 남길수록 되돌아갈 수 있는 안전지대가 촘촘해진다.

**왜 중요한가 / 언제 쓰나:**
- AI가 코드를 크게 고치기 직전에 커밋해두면, 결과가 마음에 안 들 때 깔끔히 그 지점으로 되돌릴 수 있다.
- 커밋 메시지는 나중에 "언제 왜 이렇게 바뀌었나"를 읽는 기록이 된다 — 협업과 회고의 바탕.

**실무 예시 / AI에게 이렇게 말한다:**
- "지금 변경한 내용을 '로그인 버그 수정'이라는 메시지로 커밋해줘."
- "이번 작업을 의미 단위로 나눠서 여러 개의 작은 커밋으로 남겨줘."

**흔한 오해:** 커밋은 그 자체로 남과 공유되지 않는다 — **커밋은 우선 내 저장소(로컬) 이력에만 쌓이고**, 남에게 보내려면 별도로 밀어 올리는(push) 단계가 필요하다. 또 커밋은 파일을 "덮어써 지우는" 게 아니라 시점별 스냅샷을 쌓는 것이라, 과거 커밋의 내용은 그대로 남는다.

**함께 보기:** [Repository (repo)](#repository-repo--리포지터리저장소), [Branch / Merge](#branch--merge--브랜치머지), [Clone / Pull / Push](#clone--pull--push--클론풀푸시)

**출처:** Scott Chacon & Ben Straub, *Pro Git* (2판) — "Recording Changes to the Repository", [git-scm.com/book](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository); 명령 정의: [git-scm.com/docs/git-commit](https://git-scm.com/docs/git-commit). (확인 — Git 공식 정의; 커밋=스냅샷.)

---

### Branch / Merge · 브랜치·머지

> **한 줄 요약:** 브랜치 = 본류를 건드리지 않고 갈래를 따로 내 작업하는 것; 머지 = 그 갈래를 다시 본류에 합치는 것.

**정의 (Definition)**
- KO: **브랜치**는 저장소의 이력에서 갈라져 나온 독립적인 작업 갈래로, 본류(main)를 건드리지 않고 따로 변경을 쌓을 수 있게 한다. **머지**는 그렇게 갈라진 브랜치의 변경을 다른 브랜치(대개 본류)에 합쳐 넣는 동작이다.
- EN: A **branch** is an independent line of development split off from the repository's history, letting you accumulate changes without touching the main line. **Merge** is the operation of combining the changes from one branch back into another (usually main).

**비유 (쉽게):** 문서를 "다른 이름으로 저장"해 사본에서 마음껏 고치는 것과 같다(브랜치). 원본은 그대로 두고 사본에서 실험하다가, 결과가 좋으면 그 수정을 원본에 다시 반영한다(머지). 여러 사람이 각자 사본을 따로 고친 뒤 나중에 하나로 합칠 수 있다.

**왜 중요한가 / 언제 쓰나:**
- AI에게 위험한 큰 변경을 시킬 때, 새 브랜치에서 작업하게 하면 본류(잘 돌아가는 버전)를 안전하게 지킬 수 있다.
- 여러 기능·실험을 동시에 진행하고, 각각 검증된 뒤에 본류로 합치는 협업의 기본 방식이다.

**실무 예시 / AI에게 이렇게 말한다:**
- "`main`은 그대로 두고, `feature/dark-mode`라는 새 브랜치를 만들어 거기서 작업해줘."
- "이 브랜치에서 한 변경을 검토했으니 이제 main에 머지해줘."

**흔한 오해:** 브랜치는 파일을 복사한 별도 폴더가 아니다 — 같은 저장소 안에서 이력을 갈래로 나눈 것일 뿐이라 가볍고 순식간에 만들어진다. 또 머지가 늘 자동으로 깔끔히 되는 것도 아니다: 같은 부분을 양쪽이 다르게 고쳤으면 **충돌(merge conflict)**이 나서 어느 쪽을 택할지 사람이 정해줘야 한다.

**함께 보기:** [Commit](#commit--커밋), [Fork](#fork--포크), [Pull request (PR)](#pull-request-pr--풀-리퀘스트)

**출처:** Scott Chacon & Ben Straub, *Pro Git* (2판) — "Branches in a Nutshell" 및 "Basic Branching and Merging", [git-scm.com/book](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell); 명령 정의: [git-scm.com/docs/git-merge](https://git-scm.com/docs/git-merge). (확인 — Git 공식 정의.)

---

### Clone / Pull / Push · 클론·풀·푸시

> **한 줄 요약:** 원격 저장소와 내 것을 맞추는 3동작 — 클론(처음 통째 복제) · 풀(원격의 최신 변경 받아오기) · 푸시(내 커밋 올려보내기).

**정의 (Definition)**
- KO: **클론**은 원격 저장소를 이력까지 통째로 처음 복제해 내 컴퓨터에 가져오는 동작이다. **풀**은 원격에 쌓인 최신 변경을 내 저장소로 받아와 합치는 동작이다. **푸시**는 내가 로컬에 쌓은 커밋을 원격 저장소로 올려보내는 동작이다.
- EN: **Clone** makes the first full copy of a remote repository (including its history) onto your machine. **Pull** fetches the latest changes from the remote and integrates them into your copy. **Push** sends your local commits up to the remote.

**비유 (쉽게):** 공유 문서함을 두고 일하는 것과 같다. 처음에 문서함 전체를 내 책상으로 통째 복사해 오고(클론), 남들이 바꾼 최신본을 수시로 받아 내 것에 반영하고(풀), 내가 고친 것을 문서함에 다시 올려 남과 공유한다(푸시). 풀은 "받기", 푸시는 "보내기"로 방향이 반대다.

**왜 중요한가 / 언제 쓰나:**
- GitHub 등에 있는 프로젝트를 처음 가져와 작업하려면 클론부터 한다.
- 여럿이 같은 저장소를 쓸 때, 남의 변경을 풀로 받아오고 내 변경을 푸시로 올리는 왕복이 협업의 리듬이다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 GitHub 주소의 저장소를 클론하는 명령을 알려줘."
- "원격의 최신 변경을 풀로 받아온 다음, 내 커밋을 푸시해줘."

**흔한 오해:** 풀과 푸시는 방향이 정반대다 — **풀은 원격→내 컴퓨터(받기), 푸시는 내 컴퓨터→원격(보내기)**이라 헷갈리면 안 된다. 또 커밋만 해서는 남에게 전달되지 않는다: 커밋은 로컬 이력에 남는 것이고, 실제 공유는 푸시를 해야 이뤄진다. 클론은 최초 1회 통째 복제이고, 그 뒤 갱신은 풀로 한다.

**함께 보기:** [Commit](#commit--커밋), [Repository (repo)](#repository-repo--리포지터리저장소), [Git vs GitHub](#git-vs-github--깃-vs-깃허브)

**출처:** Git 명령 정의 — [git-scm.com/docs/git-clone](https://git-scm.com/docs/git-clone) · [git-scm.com/docs/git-pull](https://git-scm.com/docs/git-pull) · [git-scm.com/docs/git-push](https://git-scm.com/docs/git-push); 보조: *Pro Git* — "Working with Remotes", [git-scm.com/book](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes). (확인 — Git 공식 명령 문서.)

---

### Fork · 포크

> **한 줄 요약:** 남의 저장소를 내 계정으로 통째 복제해, 원본과 별개로 내 마음대로 고칠 수 있게 만드는 것(주로 GitHub 등 플랫폼 기능).

**정의 (Definition)**
- KO: 다른 사람의 저장소를 내 계정 아래 독립된 새 저장소로 통째 복제하는 것. 원본과 연결은 유지하되, 내 포크에서 자유롭게 실험·수정하고 나중에 원본에 변경을 제안(pull request)할 수 있다. Git 자체의 명령이 아니라 GitHub 등 호스팅 플랫폼이 제공하는 기능이다.
- EN: Making a full, independent copy of someone else's repository under your own account. It stays linked to the original ("upstream") but lets you experiment freely and later propose changes back via a pull request. This is a platform feature (GitHub, etc.), not a core Git command.

**비유 (쉽게):** 남이 공개한 요리 레시피를 내 요리책에 통째로 베껴 와, 원본은 그대로 두고 내 사본을 내 입맛대로 뜯어고치는 것과 같다. 마음에 드는 개선을 만들면 "원저자님, 이 부분 이렇게 바꾸면 어떨까요?" 하고 되돌려 제안할 수도 있다.

**왜 중요한가 / 언제 쓰나:**
- 내가 수정 권한이 없는 남의 공개 프로젝트(오픈소스)를 가져다 고치거나 기여하고 싶을 때 표준 방식이다.
- 원본을 건드리지 않고 내 것으로 독립시켜 실험하고 싶을 때 쓴다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 오픈소스 저장소를 내 계정으로 포크한 다음, 그 포크를 내 컴퓨터로 클론하는 절차를 알려줘."
- "포크한 저장소에서 원본의 최신 변경을 따라잡으려면 어떻게 해야 하는지 설명해줘."

**흔한 오해:** 포크는 **core git 명령이 아니라 플랫폼(GitHub 등) 기능**이다 — `git fork` 같은 명령은 없다. 또 브랜치와 혼동하기 쉬운데, 브랜치는 같은 저장소 안의 갈래이고 **포크는 내 계정 밑에 통째로 생기는 별개의 저장소**다(대개 남의 프로젝트를 대상으로 함). GitLab·Bitbucket 등에도 비슷한 기능이 있다.

**함께 보기:** [Branch / Merge](#branch--merge--브랜치머지), [Pull request (PR)](#pull-request-pr--풀-리퀘스트), [Git vs GitHub](#git-vs-github--깃-vs-깃허브)

**출처:** GitHub Docs, *About forks*, [docs.github.com](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/about-forks). (확인 — GitHub 정의: 포크는 upstream과 코드를 공유하는 새 저장소; 플랫폼 기능이며 core git 명령 아님.)

---

### Pull request (PR) · 풀 리퀘스트

> **한 줄 요약:** 내가 고친 변경을 원본에 "이렇게 바꿨는데 반영해 주실래요?" 하고 제안해, 리뷰·논의를 거쳐 합치는 절차(GitHub 등 플랫폼 기능).

**정의 (Definition)**
- KO: 내 브랜치나 포크에 담긴 변경을 다른 브랜치(대개 원본의 본류)에 합쳐달라고 제안하고, 그 변경을 함께 리뷰·논의한 뒤 병합(merge)하는 절차. Git 자체가 아니라 GitHub·GitLab 등 호스팅 플랫폼이 제공하는 협업 기능이다.
- EN: A proposal to merge the changes on your branch or fork into another branch (usually the original's main line), letting people review and discuss the changes before merging them. It's a feature of hosting platforms (GitHub, GitLab), not core Git.

**비유 (쉽게):** 회사에서 문서를 고친 뒤 상사에게 바로 원본을 덮어쓰지 않고, "이렇게 수정했는데 검토 후 반영해 주시겠어요?"라고 올리는 결재·검토 요청서와 같다. 상대가 내용을 보고 의견을 달거나 수정을 요청할 수 있고, 승인되면 원본에 합쳐진다.

**왜 중요한가 / 언제 쓰나:**
- 여럿이 함께 쓰는 프로젝트에서 변경을 곧바로 본류에 밀지 않고, 검토·논의를 거쳐 안전하게 반영하는 협업의 표준 관문이다.
- 오픈소스에 기여할 때(포크 → 수정 → PR)나, 팀에서 AI가 만든 변경을 사람이 검토한 뒤 합칠 때 쓴다.

**실무 예시 / AI에게 이렇게 말한다:**
- "이 변경을 새 브랜치에 커밋하고, main으로 향하는 풀 리퀘스트를 올려줘 — 무엇을 왜 바꿨는지 설명도 붙여서."
- "이 PR에서 무엇이 바뀌었는지 요약하고, 검토자가 볼 만한 위험 지점을 짚어줘."

**흔한 오해:** 이름과 달리 **PR은 명령어 `git pull`과는 별개의 것**이다(용어만 비슷할 뿐 서로 다른 개념). 또 core git 기능이 아니라 플랫폼 기능이라, **GitLab에서는 같은 것을 "merge request(머지 리퀘스트)"라고 부른다**. PR을 연다고 바로 합쳐지는 것도 아니다 — 리뷰·승인·병합은 별도 단계다.

**함께 보기:** [Branch / Merge](#branch--merge--브랜치머지), [Fork](#fork--포크), [Git vs GitHub](#git-vs-github--깃-vs-깃허브)

**출처:** GitHub Docs, *About pull requests*, [docs.github.com](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests); GitLab의 대응 용어: GitLab Docs, *Merge requests*, [docs.gitlab.com](https://docs.gitlab.com/ee/user/project/merge_requests/). (확인 — PR은 GitHub 협업 기능; GitLab은 "merge request"로 칭함; core git 명령 아님.)
