# Solution · Exercise 02 · 로컬에서 시작해 원격으로 발행하기

## 실행 로그 예시
```bash
$ mkdir react-widget
$ cd react-widget
$ git init --initial-branch main
Initialized empty Git repository in C:/projects/react-widget/.git/

$ echo "# Local setup log" > notes.md
$ curl https://raw.githubusercontent.com/github/gitignore/main/Node.gitignore -o .gitignore

$ git status
On branch main
No commits yet

$ git add .gitignore notes.md
$ git commit -m "chore: bootstrap repo with notes and ignore"
[main (root-commit) 5d6e7f8] chore: bootstrap repo with notes and ignore
 2 files changed, 45 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 notes.md
```

## 원격 생성 & 연결
```bash
# GitHub에서 team/react-widget 저장소 생성 후
$ git remote add origin git@github.com:team/react-widget.git
$ git remote -v
origin  git@github.com:team/react-widget.git (fetch)
origin  git@github.com:team/react-widget.git (push)

$ git push -u origin main
Enumerating objects: 5, done.
...
branch 'main' set up to track 'origin/main'.
```

## 실험 브랜치 준비
```bash
$ git checkout -b feature/initial-layout
Switched to a new branch 'feature/initial-layout'
$ git status
On branch feature/initial-layout
nothing to commit, working tree clean
```

## `notes.md` 기록 예시
```
# Local setup log
- 2024-05-02: Initialized repo (Git 2.44.0.windows.1)
- Added Node.gitignore template
- Remote origin: git@github.com:team/react-widget.git
- Branches: main (tracking origin/main), feature/initial-layout (local)
```

## 원격 검증
- GitHub → `team/react-widget` → `main` 브랜치에 첫 커밋 존재
- 브랜치 드롭다운에서 `feature/initial-layout` 확인 가능 (초기 커밋 없음)
- 필요한 경우 보호 브랜치 설정을 추가로 구성
