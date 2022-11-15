---
toc: True
tags: etc
---
# Git - Commit Message Convention 이란?
협업을 수행할 때 진행하는 사람들끼리 어떤 Commit을 수행했는지 한눈에 파악하기 위함이다.

# Structure
```text
type : subject
body
footer
```
## element
* type
  * feat : 새로운 기능 개발
  * fix : 버그 수정
  * docs : 문서 수정 
  * style : 코드 포맷팅, 세미콜론 누락, 코드 변경이 없는 경우
  * refactor: 코드 리팩토링(코드의 가독성 및 유지보수성을 높이기 위해 내부 구조를 변경하는 것)
  * test: 테스트 코드, 리펙토링 코드 추가
  * chore: 빌드 업무 수정, 패키지 매니저 수정
* subject:
  * 제목은 50자를 넘기지 않고, 대문자로 시작하고 과거시제를 사용하지 않는다. 그리고 명령어로 작성한다.
* body, footer: 선택사항 
  * body: 부연설명이 필요하거나, 커밋의 이유를 설명
  * footer: issue tracker id를 작성할 때 사용한다  

# CLI 환경
CLI 환경에서는 git commit -m "커밋 제목 (엔터 두버)"으로 수행한다.
