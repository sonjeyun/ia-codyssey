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

<img width="306" height="150" alt="image" src="https://github.com/user-attachments/assets/3f854df3-8fec-4607-9bcc-0b5c32a127ee" />


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

git init


원격 저장소 연결 및 첫커밋,푸시

git remote add origin [저장소 주소]
git add .
git commit -m "first commit"
git push -u origin main


필수 파일 작성
-.gitignore 파일 생성 (불필요한 파일 제외)
-README.md 파일 생성 (프로젝트 제목 작성)

2-3 clone 실습
프로그램 주요 기능
git clone [저장소 주소]
git log --oneline


프로그램 주요 기능

3-1. 프로그램 실행 및 메뉴
프로그램 실행 시 메뉴 출력
번호 입력으로 기능 선택
잘못된 번호 입력 시 안내 메시지 출력 후 메뉴 재출력
"종료" 선택 시 프로그램 종료
각 기능 수행 후 메뉴로 복귀

def show_menu():
    print("\n===== AI 프롬프트 관리 프로그램 =====")
    print("1. 프롬프트 추가")
    print("2. 프롬프트 목록")
    print("3. 카테고리별 조회")
    print("4. 프롬프트 검색")
    print("5. 상세 보기")
    print("6. 즐겨찾기 관리")
    print("0. 종료")







