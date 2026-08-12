#  AI 프롬프트 관리 프로그램 개발 보고서

##  프로젝트 개요
Python과 Git을 활용하여 AI 프롬프트를 체계적으로 관리하는 CLI 프로그램을 개발하였다.
프롬프트의 추가, 조회, 검색, 즐겨찾기 관리 기능을 제공하며, Git을 통한 버전 관리와 브랜치 협업 방식을 
실습하였다.

---

##  개발 환경 설정

### 1-1. VSCode 및 확장 프로그램 설치
- VSCode에 **Python 확장(Extension)** 설치
- **Korean Language Pack** 설치 (한글화)

<img width="295" height="222" alt="image" src="https://github.com/user-attachments/assets/70fc36b4-5262-4bd4-ab6c-fd9aa6f95027" />



### 1-2. Python 환경 확인
- 터미널에서 Python 버전 확인 (Python 3.10 이상)

<img width="518" height="171" alt="image" src="https://github.com/user-attachments/assets/82c7e9fe-60bd-4e58-910c-5419f2ecc214" />


hello 

<img width="761" height="577" alt="image" src="https://github.com/user-attachments/assets/47182fd5-c037-481c-b1cb-5fdb3b91de17" />

git 버전

<img width="400" height="102" alt="image" src="https://github.com/user-attachments/assets/3c37c373-8ac3-4c15-ab75-d499d45d06bb" />

Git 사용자 정보 설정,기본 브랜치 이름 main

<img width="629" height="87" alt="image" src="https://github.com/user-attachments/assets/fb926e04-0a2d-496d-a0d1-9f5bb772888b" />

### 2 Git 저장소 설정 및 초기화
git 저장소 생성 및 연결
```bash
git init
```

원격 저장소 연결 및 첫커밋,푸시
```bash
git remote add origin [저장소 주소]
git add .
git commit -m "first commit"
git push -u origin main
```

필수 파일 작성


-.gitignore 파일 생성 (불필요한 파일 제외)

-README.md 파일 생성 (프로젝트 제목 작성)

2-3 clone 실습
프로그램 주요 기능

<img width="574" height="412" alt="image" src="https://github.com/user-attachments/assets/2f3afcf5-79b1-40a5-839f-080ec5c20a6d" />

<img width="884" height="473" alt="image" src="https://github.com/user-attachments/assets/3634fbc3-5722-4837-a77e-fe56bf573d7b" />


프로그램 주요 기능

3-1. 프로그램 실행 및 메뉴
-프로그램 실행 시 메뉴 출력

-번호 입력으로 기능 선택

-잘못된 번호 입력 시 안내 메시지 출력 후 메뉴 재출력

-"종료" 선택 시 프로그램 종료

-각 기능 수행 후 메뉴로 복귀

```bash
def show_menu():
    print("\n===== AI 프롬프트 관리 프로그램 =====")
    print("1. 프롬프트 추가")
    print("2. 프롬프트 목록")
    print("3. 카테고리별 조회")
    print("4. 프롬프트 검색")
    print("5. 상세 보기")
    print("6. 즐겨찾기 관리")
    print("0. 종료")
```

3-2. 기본 프롬프트 데이터
-프로그램 시작 시 최소 3개 이상의 프롬프트 등록

-리스트 + 딕셔너리 구조로 저장

-각 프롬프트: 제목, 내용, 카테고리, 즐겨찾기 여부 포함

-카테고리: 텍스트 생성, 이미지 생성, 영상 생성, 페르소나, 자동화, 기타

```bash
prompts = [
    {"title": "제목", "content": "내용", "category": "텍스트 생성", "favorite": False},
    ...
]
```
3-3. 프롬프트 추가

-제목, 내용, 카테고리 입력

-입력값이 비어있으면 재입력 요청

-카테고리는 목록 선택 또는 직접 입력

-즐겨찾기 기본값은 False

3-4. 프롬프트 목록 (브랜치 활용) 

-feature 브랜치를 생성하여 해당 기능 작업

-모든 프롬프트를 번호와 함께 출력

-제목, 카테고리, 즐겨찾기(⭐) 표시

-완성 후 커밋하고 main 브랜치로 병합
```bash
git checkout -b feature/list
# 작업 및 커밋
git checkout main
git merge feature/list
```

3-5. 카테고리별 조회
-카테고리 목록 표시 후 선택

-선택한 카테고리의 프롬프트만 출력

-프롬프트가 없으면 안내 메시지 출력

3-6. 프롬프트 검색
-키워드 입력

-제목 또는 내용에 포함된 프롬프트 검색

-검색 결과 목록 출력

-결과가 없으면 안내 메시지 출력

3-7. 프롬프트 상세 보기
-번호 입력 시 해당 프롬프트 전체 내용 출력

-제목, 카테고리, 즐겨찾기 여부, 내용 전체 표시

-잘못된 번호 입력 시 안내 메시지 출력

3-8. 즐겨찾기 관리 
-번호 입력으로 즐겨찾기 추가/해제

-즐겨찾기된 프롬프트만 모아서 조회 가능

코드 구조

기능별로 함수를 분리하여 가독성과 유지보수성 향상
주요 함수 목록:
```bash
함수명	기능
show_menu()	메뉴 출력
add_prompt()	프롬프트 추가
show_list()	목록 출력
search_prompt()	검색
show_favorites()	즐겨찾기 조회
save_prompts()	JSON 저장 (보너스)
load_prompts()	JSON 불러오기 (보너스)
```

코드저장소
```bash
https://github.com/sonjeyun/python.git
```



커밋 

<img width="513" height="426" alt="image" src="https://github.com/user-attachments/assets/cbaecfda-d486-484f-978a-02218dac1c5c" />


```bash
순번 커밋 ID커밋 메시지	의미 
1	b7147c7	Initial commit	                        프로젝트 시작  
2	73f2f3e	remove unnecessary files	            불필요한 파일 정리 
3	ba683f1	add gitignore	                        gitignore 추가
4	b5f336c	Delete mkdir my-project.py	            잘못된 파일 삭제 
5	55852d2	Delete Untitled-1.py	                임시 파일 삭제 
6	3ad2c57	feat: AI 프롬프트 관리 프로그램 완성	    핵심 기능 완성
7	dfedb32	docs: 목록 기능 주석 추가	            문서화 작업
8	15e17b4	내 프로젝트에 맞게 gitignore 수정	    설정 개선 
9	b8ceb85	feat: JSON 저장/불러오기 기능 추가	    새 기능 추가
10	5af13be	Update README.md	                    문서 업데이트 
```

보너스 기능 (JSON 저장/불러오기-> md파일로 변환) 
```bash
import json 활용
프로그램 종료 후에도 데이터 유지
save_prompts() : 프롬프트를 JSON 파일로 저장
load_prompts() : JSON 파일에서 프롬프트 불러오기
def export_to_markdown():
    """프롬프트를 마크다운 파일로 내보내는 함수"""
    show_list()
    if not prompts:
        return

    choice = input("\n내보낼 프롬프트 번호: ").strip()
    if not (choice.isdigit() and 1 <= int(choice) <= len(prompts)):
        print("⚠️ 잘못된 번호입니다.")
        return

    p = prompts[int(choice) - 1]
    category = p["category"]
    content = f"# {p['title']}\n\n"
    content += f"**카테고리:** {category}\n\n"
    content += f"## 내용\n\n{p['content']}\n"

    with open(f"{p['title']}.md", "w", encoding="utf-8") as f:
        f.write(content)

    print(f"✅ '{p['title']}.md' 파일로 내보냈어요!"),
elif choice == "8":             
            export_to_markdown()
```

<img width="888" height="363" alt="image" src="https://github.com/user-attachments/assets/9591eea4-c84d-4856-a130-55c409104853" />



<img width="883" height="391" alt="image" src="https://github.com/user-attachments/assets/39f291f7-eb20-47fa-b3a1-94da511dd96f" />


