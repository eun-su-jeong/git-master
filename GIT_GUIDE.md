# Git 마스터 가이드

## 0. 로드맵 & 디렉터리 구조
이 저장소는 기초부터 실무 고급 패턴까지 단계적으로 연습하는 커리큘럼입니다. 다음과 같이 디렉터리를 구성해 각 주제를 직접 실습해 보세요.

```
01_environment-and-setup/
02_basic-workflow/
03_branching-and-merging/
04_rewriting-history/
05_collaboration-and-remotes/
06_release-strategies/
07_debugging-and-maintenance/
08_automation-and-tooling/
```

각 디렉터리에는 다음과 같은 파일을 두면 학습 효율이 올라갑니다.
- `README.md`: 실습 절차와 핵심 개념 정리
- `exercises/`: 따라하며 배울 수 있는 시나리오, 스크립트, 더미 리포지토리
- `solutions/`: 실습 리셋 및 모범 답안
- `notes.md`: 시행착오, 발견한 팁, 팀 규칙 기록

## 1. Git 핵심 개념 잡기

### 1.1 Git은 무엇인가?
- 분산 버전 관리 시스템(DVCS)으로, 모든 개발자가 전체 히스토리를 로컬에 복제합니다.
- 인터넷이 없어도 대부분의 작업(커밋, 브랜치, 로그 확인)이 가능하며, 나중에 원격 저장소와 동기화합니다.

### 1.2 로컬과 원격의 관계
| 구분 | 위치 | 예시 | 특징 |
| --- | --- | --- | --- |
| 로컬 저장소 | 개발자 PC | `C:/projects/my-app/.git` | 개인 작업 공간, 빠른 히스토리 탐색 |
| 원격 저장소 | GitHub·GitLab·사내 Git 서버 | `git@github.com:user/my-app.git` | 팀이 공유하는 기준선, 백업 역할 |

- 로컬에서 커밋을 만들고 검증 → 원격에 `push` 하여 팀과 공유 → 팀의 변경은 `fetch/pull`로 가져옵니다.

### 1.3 작업 디렉터리 · 스테이징 영역 · 커밋 히스토리
Git은 세 가지 영역으로 작업을 관리합니다.

| 영역 | Git 용어 | 설명 | 대표 명령 |
| --- | --- | --- | --- |
| 작업 디렉터리 | Working Tree | 현재 수정 중인 파일 | `git status`, 직접 파일 수정 |
| 스테이징 영역 | Index / Stage | 커밋에 포함시킬 준비 목록 | `git add <파일>`, `git restore --staged` |
| 히스토리 | Commit History | 완료된 스냅샷 | `git commit`, `git log` |

- `git add`는 변경 사항을 스테이징 영역으로 올려 “다음 커밋에 포함”하겠다는 선언입니다.
- `git commit`은 스테이징된 내용을 하나의 버전으로 확정하며 메시지와 함께 기록합니다.
- 실수로 스테이징한 파일은 `git restore --staged <파일>`로 내릴 수 있습니다.

## 2. 환경 준비하기 (`01_environment-and-setup/`)

### 2.1 설치
- Windows: https://git-scm.com 에서 설치, “Git from the command line” 옵션 선택 권장
- macOS: `brew install git` 또는 Xcode Command Line Tools
- Linux: Debian/Ubuntu `sudo apt install git`, Fedora/RHEL `sudo dnf install git`
- 설치 후 `git --version`으로 버전 확인 (최신 기능 사용 가능 여부 확인용)

### 2.2 기본 사용자 정보 설정
```bash
git config --global user.name "홍길동"
git config --global user.email "gil@example.com"
```
- 왜 필요한가? 원격 저장소(예: GitHub)는 이름·이메일이 없으면 커밋을 거부하거나 “Unknown author”로 표시합니다.
- 팀 계정과 동일한 이메일을 사용하면 웹 서비스에서 커밋이 자동으로 연결됩니다.

### 2.3 줄바꿈(LF/CRLF) 및 기본 브랜치 설정
```bash
git config --global core.autocrlf input          # macOS/Linux
# git config --global core.autocrlf true          # Windows에서 CRLF → LF 변환 필요 시

git config --global core.safecrlf warn            # 위험한 줄바꿈 변환 경고

git config --global init.defaultBranch main       # 새 리포지토리 기본 브랜치 이름 통일

git config --global pull.ff only                  # git pull 시 Fast-forward만 허용
```
- 줄바꿈 통일은 협업 시 파일이 계속 바뀐 것으로 보이는 문제를 막습니다.
- `pull.ff only`를 통해 실수로 merge 커밋이 생기는 것을 예방할 수 있습니다.

### 2.4 자격 증명 & 인증
- **SSH 키**: `ssh-keygen -t ed25519 -C "gil@example.com"` → 공개키를 Git 호스트에 등록
- **자격 증명 관리자**: Windows/Mac은 `git config --global credential.helper manager-core`, Linux는 `cache` 또는 `libsecret`
- 왜 필요한가? 매번 비밀번호를 입력하지 않고도 안전하게 원격 저장소를 이용할 수 있습니다.

### 2.5 설정 확인
```bash
git config --list --show-origin
```
- 각 설정이 시스템/글로벌/로컬 중 어디에서 왔는지 출처를 확인할 수 있습니다.

## 3. 기초 리포지토리 시작하기 (`01_environment-and-setup/`)

### 3.1 `git init`로 새 리포지토리 만들기
```bash
mkdir my-first-repo
cd my-first-repo
git init --initial-branch main
```
- `.git/` 디렉터리가 생성되며 버전 관리가 시작됩니다.
- `.gitignore`를 바로 만들어 빌드 산출물·비밀 값이 올라가지 않도록 합니다.

### 3.2 `git remote add origin`으로 원격 연결하기
```bash
git remote add origin git@github.com:user/my-first-repo.git
git remote -v
```
- `origin`은 관례적인 기본 원격 이름이며, 다른 이름도 가능하지만 통일을 위해 권장됩니다.
- 왜 필요한가? 로컬 커밋을 팀과 공유하거나 CI/CD 파이프라인을 연동하려면 원격 연결이 필수입니다.

### 3.3 첫 커밋 만들기 흐름
1. 파일 생성/수정: 예) `echo "# My Project" > README.md`
2. 상태 확인: `git status`
3. 스테이징: `git add README.md`
4. 커밋: `git commit -m "docs: 초기 README 추가"`
5. 로그 확인: `git log --oneline --decorate`

### 3.4 원격으로 밀어 올리기
```bash
git push -u origin main
```
- `-u` 옵션은 로컬 `main` 브랜치의 upstream을 `origin/main`에 묶어, 이후 `git push`만 입력해도 되도록 해줍니다.

### 3.5 `git clone`으로 기존 리포지토리 받아오기
```bash
git clone git@github.com:user/awesome-app.git
cd awesome-app
```
- 원격의 모든 히스토리를 새 폴더에 내려받습니다.
- 클론하면 자동으로 `origin` 원격과 기본 브랜치 추적 관계가 설정됩니다.
- 클론 후에는 반드시 `git status`, `git remote -v`로 현재 상태를 확인하세요.

## 4. 일상 워크플로 마스터 (`02_basic-workflow/`)

### 4.1 상태와 변경 확인
- `git status`: 어떤 파일이 수정·추가·삭제되었는지 한눈에 파악
- `git diff`: 작업 디렉터리 ↔ 스테이징 비교
- `git diff --cached`: 스테이징 ↔ 최근 커밋 비교

### 4.2 스테이징 전략
```bash
git add <파일>
git add -p            # 변경 덩어리 단위로 선택
```
- 의미 있는 커밋을 만들기 위해 관련 변경만 골라 담습니다.
- `git restore --staged <파일>`로 실수로 올린 내용을 내리세요.

### 4.3 커밋 메시지 작성법
- 권장: `type(scope): summary` 형식 (예: `feat(auth): OAuth 로그인 추가`)
- 템플릿 설정: `git config commit.template ~/.gitmessage`
- 좋은 메시지는 나중에 히스토리를 탐색할 때 큰 힘이 됩니다.

### 4.4 되돌리기 기본
- 작업 디렉터리 되돌리기: `git restore <파일>`
- 최근 커밋 덮어쓰기(아직 push 전): `git commit --amend`
- 변경 임시 저장: `git stash push -m "WIP"`

## 5. 브랜치와 병합 (`03_branching-and-merging/`)

### 5.1 브랜치 생성과 전환
```bash
git switch -c feature/login   # 생성 + 전환
git branch                     # 브랜치 목록
```
- 브랜치를 분리하면 각 기능/버그 수정이 서로 영향을 주지 않습니다.

### 5.2 머지 전략 이해
| 전략 | 명령 | 특징 |
| --- | --- | --- |
| Fast-forward | `git merge --ff-only branch` | 변형 없는 직선형 기록 유지 |
| 3-way merge | `git merge branch` | 병합 커밋 생성, 공통 조상 활용 |
| Rebase | `git rebase main` | 히스토리를 다시 쓰며 직선으로 정리 |

- 팀 정책에 따라 언제 rebase/merge를 사용할지 실습으로 익히세요.

### 5.3 충돌 처리 흐름
1. 머지/리베이스 실행
2. `<<<<<<<`, `=======`, `>>>>>>>` 표식이 생긴 파일 수정
3. 수정 완료 후 `git add <파일>`
4. `git merge --continue` 또는 `git rebase --continue`
5. 최종 로그 확인

### 5.4 Cherry-pick과 Reflog
- `git cherry-pick <커밋>`: 특정 커밋만 다른 브랜치에 복사
- `git reflog`: 브랜치 포인터 이동 기록 조회 → 실수 복구의 안전망

## 6. 히스토리 재작성 & 복구 (`04_rewriting-history/`)

### 6.1 Reset 종류
| 명령 | 유지되는 것 | 삭제되는 것 | 용도 |
| --- | --- | --- | --- |
| `git reset --soft HEAD~1` | 작업/스테이징 | 최신 커밋 | 커밋 메시지 수정 전환 |
| `git reset --mixed HEAD~1` | 작업 | 스테이징, 최신 커밋 | 커밋을 없애고 변경은 유지 |
| `git reset --hard HEAD~1` | 없음 | 모든 변경 | 되돌릴 수 없으니 주의 |

### 6.2 Revert
```bash
git revert <커밋>
```
- 공개된 히스토리는 `revert`로 되돌리는 것이 안전합니다. 새로운 커밋으로 “되돌림”을 기록합니다.

### 6.3 Restore
```bash
git restore --source=HEAD^ <파일>
```
- 특정 파일을 예전 버전으로 되돌리되, 다른 파일은 유지하고 싶을 때 유용합니다.

### 6.4 Stash 활용
- `git stash push -m "버그 실험"`
- `git stash list`, `git stash apply`, `git stash drop`
- 실험 중인 변경을 잠시 치워두고 깔끔한 상태로 다른 작업을 처리할 수 있습니다.

## 7. 협업과 원격 (`05_collaboration-and-remotes/`)

### 7.1 원격 브랜치 추적
```bash
git push -u origin feature/login
```
- 추적 관계를 만들면 `git pull`이 어느 브랜치를 기준으로 하는지 Git이 기억합니다.

### 7.2 Fetch vs Pull
- `git fetch`: 원격 변경만 내려받고 로컬 작업과 섞지 않음
- `git pull`: `fetch` + 자동 병합(또는 rebase)
- 실습 Tip: 항상 `fetch`로 변화를 살펴본 뒤 `merge` 또는 `rebase`를 직접 수행해보세요.

### 7.3 코드 리뷰 준비
1. 로컬 브랜치를 최신 `main`에 rebase
2. 테스트/빌드 통과 확인
3. `git push --force-with-lease`로 안전하게 업데이트
4. PR/머지 요청 시 커밋 메시지와 변경 요약 명확히 작성

### 7.4 GPG 서명 커밋
- `gpg --full-generate-key`
- `git config --global user.signingkey <키ID>`
- `git config --global commit.gpgsign true`
- 서명된 커밋은 기업 환경에서 신뢰성을 높이는 데 필수인 경우가 많습니다.

## 8. 릴리스 전략 (`06_release-strategies/`)

### 8.1 Git Flow (CLI)
- 설치: `brew install git-flow-avh` 또는 OS별 패키지 매니저
- `git flow init`으로 브랜치 모델 설정 (develop, feature, release, hotfix)
- 명령 예시: `git flow feature start 로그인`, `git flow release finish 1.0.0`
- 릴리스 절차가 복잡한 팀은 연습해보며 자동화 스크립트와 함께 다듬으세요.

### 8.2 Trunk-Based Development
- 짧은 브랜치, 빈번한 머지, 기능 플래그 활용
- CI/CD와 병행하여 항상 배포 가능한 상태 유지
- Git Flow와 대비하여 장단점을 기록해보세요.

### 8.3 태그와 릴리스
```bash
git tag -a v1.0.0 -m "첫 릴리스"
git push origin v1.0.0
```
- 태그는 릴리스 지점을 명확히 표시하고, 빌드 파이프라인의 기준점으로 활용됩니다.

## 9. 디버깅 & 유지보수 (`07_debugging-and-maintenance/`)

### 9.1 Bisect로 버그 찾기
```bash
git bisect start
git bisect bad HEAD
git bisect good <정상 커밋>
```
- 커밋을 절반씩 나누어 문제를 일으킨 시점을 빠르게 찾는 알고리즘입니다.

### 9.2 Blame
```bash
git blame src/app.js
```
- 각 줄이 어느 커밋에서 생겼는지 확인해 맥락을 파악합니다. 책임 추궁이 아닌 원인 분석 도구입니다.

### 9.3 저장소 청소
```bash
git gc --prune=now --aggressive
```
- 대용량 저장소의 불필요한 객체를 정리해 성능과 용량을 확보합니다.

### 9.4 Hook 활용
- `.git/hooks/pre-commit`: 린트/테스트 자동화
- `.git/hooks/pre-push`: 배포 전 검증
- 버전 관리를 위해 Husky, Lefthook 같은 도구를 디렉터리에 구성해보세요.

## 10. 자동화 & 도구 (`08_automation-and-tooling/`)

### 10.1 Git Alias
```bash
git config --global alias.lg "log --graph --decorate --oneline --all"
```
- 자주 쓰는 명령을 축약해 반복 작업에 쓰는 시간을 줄입니다.

### 10.2 템플릿 리포지토리
- `git init --template <디렉터리>`로 공통 훅/설정을 미리 적용
- GitHub/GitLab 템플릿 리포지토리로 팀 표준 프로젝트를 구성

### 10.3 GUI & CLI 혼합
- LazyGit, GitKraken, Fork 등 GUI 툴을
