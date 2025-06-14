# 프로젝트 개요: 에덴 룰렛 (Eden Roulette)

## 1. 프로젝트 목표

Another Eden 게임의 캐릭터 정보를 효과적으로 탐색하고, 무작위 캐릭터 추첨(룰렛/슬롯머신) 기능을 제공하는 Streamlit 웹 애플리케이션입니다.

## 2. 주요 기능

*   **캐릭터 정보 뷰어:**
    *   캐릭터 목록을 카드 형태로 표시.
    *   각 카드에는 캐릭터 아이콘, 이름, 희귀도, 속성, 무기, 방어구 정보 (아이콘 포함) 표시.
    *   희귀도, 속성, 무기, 방어구, 캐릭터명으로 필터링 및 검색 기능 제공.
*   **랜덤 캐릭터 추첨 (슬롯머신):**
    *   필터링된 캐릭터 중에서 무작위로 한 명을 추첨하는 슬롯머신 애니메이션 제공.
    *   당첨된 캐릭터의 상세 정보 표시.

## 3. 대상 사용자

*   Another Eden 게임 플레이어.
*   특정 조건에 맞는 캐릭터를 찾거나, 재미로 캐릭터를 뽑아보고 싶은 사용자.

## 4. 데이터 흐름 및 소스

1.  **데이터 수집:** `another_eden_gui_scraper.py` (Tkinter GUI) 스크립트가 `https://anothereden.wiki/w/Characters` 에서 캐릭터 정보를 스크레이핑하여 `another_eden_characters_detailed.xlsx` 파일과 관련 이미지(`character_art/` 폴더)를 생성.
2.  **데이터 전처리:** `eden_data_preprocess.py` 스크립트가 `another_eden_characters_detailed.xlsx` 파일을 읽어, Streamlit 앱에 적합한 형식의 `eden_roulette_data.csv` 파일로 가공. 이 과정에서 아이콘 경로 생성, 이름 매핑 등이 이루어짐.
3.  **애플리케이션:** `streamlit_eden_restructure.py` (Streamlit 앱)이 `eden_roulette_data.csv` 파일을 읽어 사용자에게 정보 표시 및 룰렛 기능 제공.

## 5. 주요 기술 스택

*   **Backend & Data Processing:** Python 3.x
*   **Web Framework:** Streamlit
*   **Data Manipulation:** Pandas
*   **Web Scraping:** Requests, BeautifulSoup
*   **Excel Handling:** Openpyxl
*   **GUI (Scraper):** Tkinter

# 프로젝트 연대기/변경 이력

## 2024-06-XX
- Memory Bank 문서화 체계 정비 및 최신화
- 개발/배포 프로세스, API 명세, 변경 이력 등 문서화

## 2024-06-XX
- Streamlit 앱 구조 개선, 데이터 전처리 파이프라인 정비
- 캐릭터/속성/장비 아이콘 경로 매핑 로직 개선

## 2024-05~2024-06
- 초기 기능 구현: 스크레이퍼, 전처리, Streamlit UI, 룰렛/필터 등