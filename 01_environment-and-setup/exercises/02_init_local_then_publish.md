# Exercise 02 · 로컬에서 시작해 원격으로 발행하기

## 시나리오
- 팀이 새 React 위젯을 만들기로 했습니다. 아직 원격 저장소는 없으며, 로컬에서 기초 구조를 만든 뒤 GitHub에 원격을 생성해야 합니다.
- 리드는 Git 설정과 브랜치 정책을 잡고, 실습자는 커맨드를 직접 실행합니다.

## 목표
1. 빈 디렉터리에서 Git 저장소를 초기화하고 기본 브랜치를 `main`으로 유지합니다.
2. React 프로젝트용 `.gitignore`를 추가합니다. (내용은 주석 또는 TODO로 남겨도 됩니다.)
3. 첫 커밋을 만들고, GitHub(또는 사내 Git)에 원격 저장소를 생성해 연결합니다.
4. `feature/initial-layout` 브랜치를 생성하여 실험용 변경을 준비합니다.

## 준비물
- 작업용 폴더 (예: `mkdir react-widget`)
- 원격 저장소를 만들 수 있는 권한 (GitHub Personal Access Token 또는 조직 권한)
- React 프로젝트 초기 템플릿 (실습에서는 빈 폴더여도 무방)

## 진행 단계
1. **디렉터리 준비**
   ```bash
   mkdir react-widget
   cd react-widget
   git init --initial-branch main
   ```
2. **환경 점검 파일 작성**
   - `touch notes.md`
   - `echo "# Local setup log" > notes.md`
3. **.gitignore 준비**
   - `curl https://raw.githubusercontent.com/github/gitignore/main/Node.gitignore -o .gitignore`
   - 가까운 환경에서는 리드가 미리 준비한 `.gitignore`를 전달해도 됩니다.
4. **첫 커밋 생성**
   ```bash
   git status
   git add .gitignore notes.md
   git commit -m "chore: bootstrap repo with notes and ignore"
   ```
5. **원격 저장소 생성**
   - GitHub에서 빈 저장소 만들기 (예: `team/react-widget`)
   - SSH URL 복사
6. **원격 연결 & 푸시**
   ```bash
   git remote add origin git@github.com:team/react-widget.git
   git push -u origin main
   ```
7. **실험 브랜치 생성**
   ```bash
   git checkout -b feature/initial-layout
   ```
8. **로그 기록**
   - `notes.md`에 커밋/브랜치 로그 기록 (`git log --oneline`, `git branch -vv` 결과 요약)

## 완료 기준
- 원격 저장소에서 `main` 브랜치와 첫 커밋을 확인할 수 있다.
- 로컬 브랜치는 `feature/initial-layout`에 위치하며, `git status`가 깨끗하다.
- `notes.md`에 수행 기록이 남아 있다.

## 확장 미션 (선택)
- `git tag -a v0.1.0 -m "initial scaffolding"` 후 태그 푸시
- GitHub 보호 브랜치 설정(`main` → direct push 금지) 논의
