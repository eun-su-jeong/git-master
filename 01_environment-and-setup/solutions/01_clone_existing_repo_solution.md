# Solution · Exercise 01 · 기존 React 리포지토리 클론 & 준비

## 실행 로그 예시
```bash
$ ssh -T git@github.com
Hi team! You've successfully authenticated, but GitHub does not provide shell access.

$ git clone git@github.com:team/react-dashboard.git
Cloning into 'react-dashboard'...
remote: Enumerating objects: 142, done.
...
$ cd react-dashboard

$ git config --list --show-origin | grep user.
file:"C:/Users/dev/.gitconfig"
user.name=홍길동
user.email=gil@example.com

$ git remote -v
origin  git@github.com:team/react-dashboard.git (fetch)
origin  git@github.com:team/react-dashboard.git (push)

$ git status
On branch main
nothing to commit, working tree clean

$ git checkout -b chore/setup-audit
Switched to a new branch 'chore/setup-audit'
```

## `notes.md` 샘플
```
# Local Git Environment (2024-05-01)
- git version 2.44.0
- user.name = 홍길동
- user.email = gil@example.com
- core.autocrlf = true (Windows)
- init.defaultBranch = main
- Remote: origin → git@github.com:team/react-dashboard.git
- Branch created: chore/setup-audit
```

## 커밋 & 푸시 확인
```bash
$ git add notes.md
$ git commit -m "chore: document local git environment"
[chore/setup-audit 1a2b3c4] chore: document local git environment
 1 file changed, 7 insertions(+)
 create mode 100644 notes.md

$ git push -u origin chore/setup-audit
Enumerating objects: 5, done.
...
branch 'chore/setup-audit' set up to track 'origin/chore/setup-audit'.
```

## 리뷰 체크포인트
- `git status` → clean
- `git log --oneline -1` → 커밋 메시지 확인
- 팀 리드가 GitHub에서 브랜치와 커밋을 확인하고 Merge 준비
