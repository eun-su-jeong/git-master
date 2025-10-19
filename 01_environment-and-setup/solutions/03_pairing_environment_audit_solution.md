# Solution · Exercise 03 · 페어 환경 점검 & 브랜치 정책 합의

## 글로벌 설정 비교
```bash
# 리드
$ git config --list --show-origin | grep -E "user\.|core.autocrlf|init.defaultBranch|pull.ff"
file:C:/Users/lead/.gitconfig    user.name=Lead Dev
file:C:/Users/lead/.gitconfig    user.email=lead@example.com
file:C:/Users/lead/.gitconfig    core.autocrlf=true
file:C:/Users/lead/.gitconfig    init.defaultBranch=main
file:C:/Users/lead/.gitconfig    pull.ff=only

# 실습자 (변경 전)
$ git config --list --show-origin | grep -E "user\.|core.autocrlf|init.defaultBranch|pull.ff"
file:C:/Users/junior/.gitconfig  user.name=Junior Dev
file:C:/Users/junior/.gitconfig  user.email=junior@example.com
file:C:/Users/junior/.gitconfig  core.autocrlf=false
file:C:/Users/junior/.gitconfig  pull.ff=merge

# 실습자 수정
$ git config --global core.autocrlf true
$ git config --global pull.ff only
```

## 원격 URL 통일
```bash
$ git remote -v
origin  https://github.com/team/react-dashboard.git (fetch)
origin  https://github.com/team/react-dashboard.git (push)

$ git remote set-url origin git@github.com:team/react-dashboard.git
$ git remote -v
origin  git@github.com:team/react-dashboard.git (fetch)
origin  git@github.com:team/react-dashboard.git (push)
```

## 브랜치 가이드 문서 예시 (`docs/branching-guidelines.md`)
```
# Branching Guidelines
- Default branch: main (protected)
- Feature work: feature/<scope>-<summary>
- Bug fixes: bugfix/<ticket-id>
- Maintenance: chore/<task>
- Release tags: vMAJOR.MINOR.PATCH (lightweight tags)
- Pull Requests must be created from feature/* or bugfix/* branches.
```

```bash
$ git checkout -b feature/pair-setup-review
$ git add docs/branching-guidelines.md
$ git commit -m "docs: outline branch naming rules"
[feature/pair-setup-review 0a1b2c3] docs: outline branch naming rules
 1 file changed, 6 insertions(+)
 create mode 100644 docs/branching-guidelines.md

$ git push -u origin feature/pair-setup-review
branch 'feature/pair-setup-review' set up to track 'origin/feature/pair-setup-review'.
```

## 검증 체크리스트
- 두 명 모두 `git config --global --list` 출력이 합의된 값으로 동일
- 원격 URL이 SSH로 일치
- GitHub에서 `feature/pair-setup-review` 브랜치와 문서 커밋 확인
- 협업 규칙 문서를 PR로 공유해 리뷰 진행
