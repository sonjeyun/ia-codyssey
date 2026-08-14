### 여행 추천 CLI 프로그램 보고서 
### 1. 프로그램 개요
### 1-1. 프로젝트 목적
```bash
본 프로젝트는 사용자가 CLI에서 날짜를 입력하면
LLM API를 통해 해당 날짜에 어울리는 여행지를 추천받고
지도/장소 검색 API를 이용해 해당 지역의 맛집 정보를 검색한 뒤
최종적으로 여행 리포트를 Markdown 파일로 저장하는 Python 프로그램을 구현하는 것을 목표로 한다.
```
### 1-2. 주요 기능
```bash
-argparse 기반 CLI 입력 처리

-날짜 형식 검증 (YYYY-MM-DD)

-LLM API를 이용한 여행지 추천 JSON 생성

-장소 검색 API를 이용한 맛집 검색

-최종 여행 리포트 Markdown 생성

-결과 파일을 results/ 폴더에 저장

-오류 발생 시 예외 처리 및 오류 요약 기록
```

### 2. 최종 결과물
본 과제에서는 다음 3가지 결과물이 포함된 프로그램 1개를 완성하였다.
```bash
2-1. CLI 기반 Python 프로그램
실행 방식: 명령줄에서 Python 파일 실행
필수 입력: -date "YYYY-MM-DD"
출력 내용:
진행 로그 출력
결과 저장 경로 안내
results/
 ├─ 2025-03-15_result.json
 └─ 2025-03-15_report.md
```

<img width="585" height="853" alt="스크린샷 2026-08-14 141149" src="https://github.com/user-attachments/assets/84e9573f-9f97-4ad5-a227-874ef6271c45" />

### 3. 과제 목표
이 과제를 수행하며 다음 내용을 설명할 수 있도록 구현하였다.

### 3-1. REST API와 HTTP 메서드 이해
```bash
REST API는 클라이언트가 서버에 요청을 보내고, 서버가 응답을 반환하는 방식으로 동작한다.
장소 검색 API는 주로 GET 요청을 통해 데이터를 조회하며,
응답은 JSON 형식으로 전달된다.
본 프로그램에서는 Kakao Local API에 GET 요청을 보내 도시별 맛집 정보를 검색하였다.

LLM API는 프롬프트를 전달하고 응답을 생성받는 방식으로 사용되며,
본 코드에서는 Gemini 라이브러리를 통해 여행 추천 정보를 생성하였다.
```
### 3-2. LLM 출력의 구조화(JSON)
```bash
본 프로그램은 Gemini에게 자유로운 문장이 아니라
반드시 JSON 형식으로만 응답하도록 프롬프트를 설계하였다.
이를 통해 recommended_cities 배열 안의 도시명, 날씨, 행사, 추천 이유, 일정 정보를 구조적으로 추출할 수 있었다.
이후 각 도시명을 Kakao 장소 검색 API의 입력값으로 사용하여 다음 단계로 연결하였다.
```
### 3-3. 외부 API 오류와 대응
```bash
외부 API를 호출할 때는 인증 오류, 네트워크 오류, 응답 지연, JSON 파싱 오류 등이 발생할 수 있다.
본 프로그램에서는 try-except를 사용하여 예외를 처리하였고,
오류가 발생하면 errors 리스트에 저장한 뒤 마지막에 요약하여 출력하도록 하였다.
또한 Gemini 호출 실패 시 최대 3회까지 재시도하도록 구현하였다.
```
### 3-4. API 키를 환경변수로 관리하는 이유
```bash
API 키를 코드에 직접 작성하면 키가 외부에 노출될 위험이 있다.
따라서 본 프로그램은 .env 파일과 python-dotenv를 사용하여
GEMINI_API_KEY, KAKAO_API_KEY를 안전하게 불러오도록 구현하였다.
이 방식은 보안에 유리하고, 다른 환경에서도 쉽게 설정을 바꿀 수 있다는 장점이 있다.
```
### 4-1. CLI 인터페이스
```bash
argparse를 사용하여 CLI 입력을 처리하였다.

필수 옵션: -date
형식 검증: YYYY-MM-DD
datetime.strptime()를 이용해 날짜 형식을 검증잘못된 입력 시 사용법 출력 후 종료
예시 설명
사용자가 날짜를 잘못 입력하면 프로그램은 예외를 발생시키는 대신,
올바른 형식을 안내하고 종료하도록 구현하였다.
```

<img width="656" height="210" alt="image" src="https://github.com/user-attachments/assets/5cefb175-c092-49b2-afb0-bf48596a0e38" />

### 4-2. API 
```bash
LLM|Google AI Studio | Gemini API
지도|Kakao Developers| kakao local API
Gemini는 프롬프트 기반으로 구조화된 JSON 응답을 생성하는 데 적합하며,
Kakao Local API는 국내 도시 이름을 기반으로 맛집 검색을 수행할 수 있어 과제 목적에 적합하다.
```
### 4-3. LLM API 연동 - 날씨/행사 정보 생성
```bash
사용자가 입력한 날짜를 바탕으로 Gemini에 프롬프트를 보내
여행하기 좋은 한국의 도시 3곳을 추천받도록 구현하였다.

프롬프트에서는 반드시 아래와 같은 JSON 형식으로만 응답하도록 요구하였다.

{
  "recommended_cities": [
    {
      "city": "도시명1",
      "weather": "해당 시기 날씨 요약",
      "events": ["행사1", "행사2"],
      "reason": "추천 이유 2~4문장",
      "itinerary": {
        "morning": "오전 일정",
        "afternoon": "오후 일정",
        "evening": "저녁 일정"
      }
    }
  ]
}
또한 응답 실패에 대비해 Gemini 호출은 최대 3회까지 재시도하도록 하였다.
for attempt in range(3):
    try:
        response = model.generate_content(
            prompt,
            request_options={"timeout": 120}
        )
        break
    except Exception as e:
        ...
응답에서 코드블록 표시가 포함될 수 있으므로,
```json, ```를 제거한 뒤 json.loads()로 파싱하였다.
```
### 4-4. 지도/장소 검색 API 연동 - 맛집 검색
```bash

Gemini가 추천한 각 도시의 이름을 이용하여
Kakao Local API에 "도시명 맛집" 키워드로 검색 요청을 보냈다.

params = {"query": f"{city} 맛집", "size": 5}
res = requests.get(url, headers=headers, params=params)
검색 결과에서 다음 정보를 추출하였다.

place_name
address
category
url
lng
lat
```
### 4-5. 최종 여행 리포트 생성
```bash
최종 리포트는 Python 코드에서 직접 Markdown 형식으로 작성하여 저장하였다.
각 도시별로 다음 항목이 포함되도록 구성하였다.

추천 도시명
날씨
행사 목록
추천 이유
1일 일정
오전
오후
저녁
맛집 리스트
오류 요약
예를 들어, 각 도시에 대해 다음과 같은 형식으로 Markdown이 작성된다.
```

<img width="784" height="836" alt="image" src="https://github.com/user-attachments/assets/2c3f8a80-d69a-4da0-be49-10b4ea74fd0e" />

### 4-6. 에러 처리
```bash
본 프로그램은 예외 상황에 대비해 try-except를 활용하였다.

1.구현된 예외 처리 내용
API 키 미설정
.env에서 키를 읽어온 뒤 누락 여부를 검사
누락 시 즉시 종료하고 설정 방법 안내 출력

2.Gemini 호출 실패
최대 3회 재시도
실패 시 오류 목록에 저장

3.Gemini JSON 파싱 실패
파싱 오류 메시지 출력
None을 반환하여 이후 작업을 중단

4.Kakao 맛집 검색 실패
오류를 errors 리스트에 저장
해당 도시의 맛집 정보는 빈 리스트 처리


5.리포트 생성은 계속 진행
파일 저장 실패

JSON 저장 또는 Markdown 저장 실패 시 오류 목록에 추가
마지막에는 report_errors(errors) 함수를 통해
발생한 문제를 한 번에 출력하도록 구성하였다.
```

예외처리 5단계 최종 위치 정리표

| # | 예외 상황 | 처리 방식 | 위치 (함수/키워드) |
|---|-----------|-----------|--------------------|
| 1 | API 키 미설정 | `exit(1)` 즉시 종료 + 안내 메시지 | `check_api_keys()`(16) |
| 2 | Gemini 호출 실패 | 3회 재시도 + 타임아웃 120초 후 `raise` | `for attempt in range(3)` (91)|
| 3 | JSON 파싱 실패 | 재요청 1회 후 `None` 반환 | `except json.JSONDecodeError` (107)|
| 4 | Kakao 맛집 검색 실패 | 빈 리스트 대체 후 다음 도시 진행 | `except` → `places = []`(218-2803) |
| 5 | 파일 저장 실패 | `errors.append()`로 기록 후 계속 | JSON/MD 저장 `except` (247-252),(257-297)|


### 4-7. API 키 관리(보안)
```bash
API 키는 코드에 직접 작성하지 않고 .env 파일에서 읽어온다.

load_dotenv()
GEMINI_API_KEY = os.getenv("GEMINI_API_KEY")
KAKAO_API_KEY = os.getenv("KAKAO_API_KEY")
```

### 4-8. 결과 저장


<img width="425" height="139" alt="image" src="https://github.com/user-attachments/assets/d1a6db82-98ed-47a7-8223-73d270d56c8e" />


```bash
프로그램은 실행 시 results/ 폴더가 없으면 자동으로 생성한다.

os.makedirs("results", exist_ok=True)
이후 입력한 날짜를 기준으로 다음 파일을 저장한다.!



results/{date}_result.json
results/{date}_report.md
```
