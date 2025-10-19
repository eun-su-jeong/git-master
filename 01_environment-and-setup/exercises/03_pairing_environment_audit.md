# Exercise 03 · 페어 환경 점검 & 브랜치 정책 합의

## 시나리오
- 프로젝트를 시작하기 전, 두 명이 같은 작업 공간을 사용할 때 발생할 수 있는 환경 차이를 줄이고자 합니다.
- 리드는 회사 가이드라인을 바탕으로 점검표를 만들고, 실습자는 자신의 환경을 실제로 비교하여 수정합니다.

## 목표
1. 글로벌 Git 설정을 서로 비교하고 필요한 값을 통일합니다.
2. 동일한 원격 저장소를 기준으로 페어 개발 브랜치를 생성합니다.
3. 브랜치 보호 규칙과 협업 규칙을 문서로 남깁니다.

## 준비물
- 동일한 원격 저장소 접근 권한 (예: `git@github.com:team/react-dashboard.git`)
- 터미널 캡처 도구(스크린샷 또는 CLI 로그 저장)

## 진행 단계
1. **글로벌 설정 비교**
   - 각각 `git config --list --show-origin` 실행 후 주요 항목 공유 (`user.*`, `core.autocrlf`, `init.defaultBranch`, `pull.ff` 등)
   - 다른 항목은 왜 다른지 논의하고, 변경이 필요하다면 `git config --global`로 수정
2. **원격 설정 점검**
   - 리포지토리 폴더에서 `git remote -v` 실행
   - push URL이 HTTPS/SSH로 혼합되어 있지 않은지 확인, 필요 시 `git remote set-url`로 일치
3. **브랜치 네이밍 합의**
   - 협업 중 자주 쓰일 브랜치 패턴 정리 (예: `feature/*`, `bugfix/*`, `docs/*`)
   - `docs/branching-guidelines.md` 파일을 생성해 합의 내용을 작성 (실습자가 문서 작성, 리드가 리뷰)
4. **페어 브랜치 생성**
   ```bash
   git checkout -b feature/pair-setup-review
   ```
   - 브랜치 생성자는 커밋 없이 `git push -u origin feature/pair-setup-review`로 빈 브랜치 공유
   - 다른 한 명은 `git fetch origin feature/pair-setup-review` 후 체크아웃하여 동기화
5. **커밋 기록**
   - `docs/branching-guidelines.md` 파일을 커밋하여 규칙 확정
   - 커밋 메시지 예시: `docs: outline branch naming rules`

## 완료 기준
- 두 명 모두 동일한 글로벌 설정을 적용했고, 확인 로그를 보관했다.
- 원격 저장소에 `feature/pair-setup-review` 브랜치가 존재한다.
- 브랜치 규칙 문서(`docs/branching-guidelines.md`)가 버전 관리된다.

## 확장 미션 (선택)
- `git config --global commit.gpgsign true` 설정 후 서명 커밋 연습
- pre-commit 훅을 설치해 `lint` 혹은 `format` 검사 자동화 기초 마련
