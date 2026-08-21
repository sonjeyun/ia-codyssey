#  자취생 레시피 AI

냉장고 속 재료만 입력하면 AI가 레시피를 추천해주는 웹 서비스

##  서비스 소개
자취생을 위한 AI 요리 추천 서비스입니다.
가진 재료를 입력하면 Gemini AI가 만들 수 있는
요리 레시피를 추천해줍니다.

##  주요 기능
- 재료 입력 → AI 레시피 추천
- 다크 모드 지원
- 반응형 디자인 (모바일/데스크톱)

##  기술 스택
- **프론트엔드:** HTML, CSS, JavaScript (바닐라)
- **백엔드:** Python (Vercel Serverless Functions)
- **AI:** Google Gemini API
- **배포:** Vercel

##  배포 URL
```bash
recipe-app-g4zi.vercel.app
```
## 구조


<img width="432" height="599" alt="image" src="https://github.com/user-attachments/assets/97d4d025-2186-484a-882c-e9309ab6c0d8" />


##  환경 변수 
| 변수명 | 설명 |
|--------|------|
| GEMINI_API_KEY | Google Gemini API 키 


### ai 실패

```bash
API 오류(4xx/5xx)->😢 오류가 발생했어요. 다시 시도해주세요!
지연/타임아웃(응답이 늦을 때)->🍳 레시피를 만들고 있어요... 잠깐만요!
```
