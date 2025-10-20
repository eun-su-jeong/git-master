# Environment & Setup Guide (Git Focus)

## 목적 & 활용 시나리오
- React 프로젝트를 받거나 새로 만들기 전에 Git 환경을 정비합니다.
- 두 명이 함께 연습한다는 가정에서, **리드(숙련자)**는 환경 점검과 브랜치 정책을 잡고 **실습자**는 명령어를 직접 실행하며 감을 잡습니다.
- Git 관련 설정과 절차만을 다루며, React 코드 생성 및 구성은 별도 문서에서 다룹니다.

## 1. 필수 체크리스트
| 항목 | 확인 방법 | 비고 |
| --- | --- | --- |
| Git 설치 여부 | `git --version` | Git 2.40 이상 권장 |
| 사용자 정보 | `git config --global user.name` / `user.email` | 회사/팀 메일과 일치하도록 |
| 기본 브랜치 | `git config --global init.defaultBranch` | 기본값을 `main`으로 고정 |
| 줄바꿈 정책 | `git config --global core.autocrlf` | Windows는 `true`, macOS/Linux는 `input` |
| Credential Helper | `git config --global credential.helper` | Windows: `manager-core`, mac: `osxkeychain`, Linux: `cache` 또는 `libsecret` |
| SSH 키 | `ssh -T git@github.com` (또는 사내 Git) | 첫 연결 시 지문(fingerprint) 확인 |

## 2. 설치 및 업데이트 플로우
1. 최신 Git 설치 → https://git-scm.com
2. 설치 후 Git Bash(Windows) 또는 터미널을 열어 필수 설정 적용
   ```bash
   git config --global user.name "홍길동"
   git config --global user.email "gil@example.com"
   git config --global init.defaultBranch main
   git config --global pull.ff only
   git config --global core.autocrlf true   # Windows 기준
   git config --global core.safecrlf warn
   git config --global credential.helper manager-core
   ```
3. `git config --list --show-origin`으로 설정 출처 확인
4. 팀원과 동시에 실습할 때는 서로의 설정값을 캡처/공유하여 누락된 항목을 체크합니다.

## 3. 로컬 작업 공간 준비
- React 소스는 `apps/` 또는 `frontend/` 디렉터리에 위치한다고 가정합니다. Git은 루트에서 초기화합니다.
- 새 프로젝트 초기화 절차:
  1. 작업용 폴더 생성: `mkdir my-react-app`
  2. `cd my-react-app`
  3. `git init --initial-branch main`
  4. `.gitignore` 템플릿 적용 (React용: [https://github.com/github/gitignore/blob/main/Node.gitignore])
  5. `git status`로 깨끗한 상태 확인
- 기존 리포지토리를 가져올 때는 `git clone` 후 `.git/config`에서 원격 URL을 SSH/HTTPS 등 팀 표준에 맞는지 검증
- 실습자에게 `.git/` 디렉터리를 직접 살펴보도록 하여 Git이 실제로 파일을 어떻게 관리하는지 감각을 익히게 합니다.

## 4. 원격 저장소 연결 전략
- **사전 조건**: 접근 권한 있는 원격 리포지토리, 혹은 로컬 bare 리포지토리를 원격처럼 사용
- 새 리포지토리를 원격에 올리기:
  ```bash
  git remote add origin git@github.com:team/my-react-app.git
  git push -u origin main
  ```
- 기존 리포지토리의 원격 URL 변경:
  ```bash
  git remote set-url origin git@github.com:team/my-react-app.git
  git remote -v
  ```
- 첫 `push` 전에 `git fetch`로 원격 브랜치 목록을 확인하고 충돌 여부 점검
- 실습자는 `git remote -v`, `git branch -vv` 명령의 의미를 설명하면서 실행해보면 효과적입니다.

## 5. 브랜치 네이밍 & 초깃값 통일
- 팀에서 사용할 네이밍 예시: `feature/*`, `bugfix/*`, `chore/*`
- 초기 세팅 실습:
  1. `git checkout -b feature/setup-ci`
  2. 변경 사항 만들기 (예: `docs/setup.md` 아래 Git 관련 체크리스트 작성)
  3. `git add .` → `git commit -m "chore: add setup checklist"`
  4. `git push -u origin feature/setup-ci`
- 실습자는 커밋 메시지 규칙(Conventional Commits 등)을 따라 써보게 합니다.

## 6. 협업 모드에서의 환경 점검 루틴
- 실습 시작 전 체크:
  - `git status`가 깨끗한지
  - 최신 원격 반영 (`git fetch --all`)
  - `git branch -r`로 필요한 브랜치 확인
- 실습 끝난 후 정리:
  - 스테이징되지 않은 파일 정리 (`git restore --staged`, `git restore`)
  - 실험용 브랜치는 `git branch -d experiment/tmp`
  - 설정 변경은 `~/.gitconfig` 백업 후 공유

## 7. 문제 상황 대비
- 잘못된 글로벌 설정 → `git config --global --edit`로 수정
- 인증 실패 → SSH 키 경로/권한 확인 (`ls -al ~/.ssh`), Credential Manager에서 캐시 삭제
- 줄바꿈 혼선 → `git config core.autocrlf` 재확인 후 문제 파일 재체크아웃 (`git checkout -- <파일>`)
- push 거부 → `git pull --rebase` 또는 `git fetch` + `git rebase origin/main`

## 8. 학습 확장 포인트
- `git config --global alias.lg "log --graph --decorate --oneline --all"`
- `pre-commit` 훅을 사용해 `lint` 또는 `test` 자동 실행 (추후 automation 섹션에서 심화)
- 개인 별 메모: Git 명령어와 결과를 캡처해 `notes.md`에 추가 → 팀 지식 베이스 구축

이 가이드를 바탕으로 실습 디렉터리(`exercises/`, `solutions/`)의 시나리오를 차례대로 진행하면, React 프로젝트를 받기 전 Git 환경을 안정적으로 세팅하는 방법을 익힐 수 있습니다.
