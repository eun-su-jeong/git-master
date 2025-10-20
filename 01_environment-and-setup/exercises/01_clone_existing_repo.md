# Exercise 01 · 기존 React 리포지토리 클론 & 준비

## 시나리오
- 팀 리드가 만들어 둔 `react-dashboard` 원격 저장소가 있습니다.
- 실습자는 첫날 온보딩 중이며, Git 클론부터 기본 브랜치 설정까지 차근차근 연습합니다.
- 리드는 실습자와 화면을 공유하며 각 단계에서 체크리스트를 검토합니다.

## 목표
1. 원격 저장소를 SSH URL로 클론합니다.
2. 글로벌 사용자 설정과 줄바꿈 정책을 점검합니다.
3. 기본 브랜치가 `main`인지 확인하고, 실습용 브랜치를 생성합니다.
4. 초기 커밋 상태를 정리하고 환경 점검 결과를 `notes.md`에 기록합니다.

## 준비물
- 원격 저장소 URL (예: `git@github.com:team/react-dashboard.git`)
- Git 2.40+ 설치
- 사전에 발급된 SSH 키 (없다면 리드가 발급 도와주기)

## 진행 단계
1. **네트워크 확인**
   - `ssh -T git@github.com`으로 SSH 연결 테스트 → 첫 연결이면 지문(Fingerprint) 확인 후 `yes`
2. **리포지토리 클론**
   ```bash
   git clone git@github.com:team/react-dashboard.git
   cd react-dashboard
   ```
3. **기본 정보 점검**
   - `git config --list --show-origin | grep user.`
   - `git remote -v`
   - `git status`
4. **줄바꿈 & 브랜치 확인**
   - `git config core.autocrlf`
   - `git branch --show-current`
5. **실습 브랜치 생성**
   ```bash
   git checkout -b chore/setup-audit
   ```
6. **환경 기록 작성**
   - `docs/notes.md` 혹은 `notes.md` 파일에 현재 Git 설정 요약 (실습자 작성 → 리드 리뷰)
   - `git add notes.md`
7. **커밋 & 공유 준비**
   ```bash
   git commit -m "chore: document local git environment"
   git push -u origin chore/setup-audit
   ```

## 완료 기준
- `git status`가 깨끗하고 브랜치가 `chore/setup-audit`에 위치
- `git log --oneline -1`에 환경 기록 커밋이 존재
- 리드와 실습자가 동일한 SSH 방식으로 원격에 접근 가능한지 확인 완료

## 확장 미션 (선택)
- `git config --global alias.co checkout` 등 유용한 alias 설정
- `git remote set-url --push origin git@github.com:team/react-dashboard.git` 명시로 push URL 고정
