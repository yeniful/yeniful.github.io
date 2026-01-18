+++
title = 'Git Submodule로 멀티플랫폼 프로젝트 데이터 관리하기'
date = 2025-12-01T00:00:00+09:00
draft = true
tags = ["Git", "Submodule", "iOS"]
categories = []
summary = "iOS 앱에서 시작해 웹까지 확장하면서, Submodule을 활용해 공유 데이터를 관리한 경험을 공유합니다."
+++
https://code.claude.com/docs/ko/costs

/usage 명령어롤 통해 확인할 수 있음
![[Screenshot 2025-12-15 at 23.12.34.png]]

공식문서
https://code.claude.com/docs/ko/costs
![[Screenshot 2025-12-15 at 23.13.47.png]]


- 대화 압축
	- Claude는 컨텍스트가 95% 용량을 초과할 때 기본적으로 자동 압축을 사용합니다
	- 자동 압축 토글: `/config`를 실행하고 “Auto-compact enabled”
	- 컨텍스트가 커질 때 `/compact`를 수동으로 사용
	- 사용자 정의 지침 추가: `/compact Focus on code samples and API usage`
	- CLAUDE.md에 추가하여 압축을 사용자 정의하는 것도 가능
- 구체적인 쿼리 작성: 불필요한 스캔을 유발하는 모호한 요청을 피합니다
- **복잡한 작업 분해:** 큰 작업을 집중된 상호작용으로 분할합니다
- **작업 간 기록 지우기:** `/clear`를 사용하여 컨텍스트를 재설정합니다

- 비용은 다음을 기반으로 크게 달라질 수 있습니다:
	- 분석 중인 코드베이스의 크기
	- 쿼리의 복잡성
	- 검색 또는 수정 중인 파일 수
	- 대화 기록의 길이
	- 대화 압축 빈도


백그라운드 토큰 사용량

Claude Code는 유휴 상태에서도 일부 백그라운드 기능에 토큰을 사용합니다:

- **대화 요약**: `claude --resume` 기능을 위해 이전 대화를 요약하는 백그라운드 작업
- **명령 처리**: `/cost`와 같은 일부 명령은 상태를 확인하기 위해 요청을 생성할 수 있습니다

이러한 백그라운드 프로세스는 활성 상호작용 없이도 적은 양의 토큰(일반적으로 세션당 $0.04 미만)을 소비합니다.


클로드 세션~
https://docs.google.com/presentation/d/1ElL9HKRIu-vVo532ItvPIPWpBQoCFa3sg7aBrtclofY/mobilepresent?slide=id.g38bce0fba31_0_16

레딧 참고
