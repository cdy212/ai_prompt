---
role: prompt_router
version: 1.0
usage: "이 파일을 먼저 읽고 keyword를 감지해서 해당 템플릿 파일을 로드하세요"
---

# Prompt Router Index

## 사용 규칙
1. 사용자 입력에서 아래 trigger_keywords 중 하나를 감지
2. 해당 파일을 GitHub에서 fetch
3. variables 목록의 값을 사용자 입력에서 추출해 치환
4. 치환된 프롬프트로 응답 생성

## Keyword → File Mapping

| trigger_keywords | file | variables |
|---|---|---|
| 1박2일, 근교여행, 주말여행, 숙박여행 | travel_overnight.md | location, companion, budget, season |
| 당일치기, 산책, 나들이, 드라이브 | travel_daytrip.md | location, companion, duration |
| 맛집, 밥집, 점심, 저녁, 카페 | food_nearby.md | location, mood, budget, companion |
| 신규개발, 새프로젝트, 개발시작, 기능추가 | dev_newproject.md | project_name, features, stack |
| 코드리뷰, 리뷰, 코드검토 | dev_codeReview.md | language, code, focus |
| 주식, 종목, 매수, 매도, 투자 | stock_analysis.md | ticker, period, strategy |

## Variable 추출 규칙
- 명시된 경우: "답십리에서" → location=답십리
- 미명시인 경우: 사용자에게 해당 변수만 질문
- 복수 감지: 가장 구체적인 keyword 우선 적용

## 아키텍처 개요
- Prompt Engineering: 템플릿 정의, role/constraints/output_format/few-shot 구성
- Context Engineering: 입력 메시지에서 trigger keyword와 변수 추출, 템플릿 치환, 불필요 토큰 제거
- Harness Engineering: Golden Dataset과 출력 비교, 스코어링, 템플릿 버전 관리
