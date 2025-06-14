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

## 이름
[여기에 프로젝트 이름을 적으세요]

## 설명
[프로젝트의 목적, 주요 목표 등을 상세히 설명하세요]

## 주요 이해관계자
- [팀 구성원, 역할, 책임 등을 목록으로 만드세요]

## 일정 및 이정표
- [중요한 날짜나 프로젝트의 주요 단계를 기록하세요]

## 기술 스택
- [사용된 프로그래밍 언어, 프레임워크, 라이브러리, 도구 등을 나열하세요]

## 최신 폴더/파일 구조 (2024-06 기준)

```
프로젝트 루트/
├── another_eden_gui_scraper.py
├── eden_data_preprocess.py
├── streamlit_eden_restructure.py
├── character_art/
│   ├── icons/
│   └── elements_equipment/
├── another_eden_characters_detailed.xlsx
├── eden_roulette_data_from_excel.csv
├── scraper_config.ini
├── memory-bank/
│   └── ...
```

## 저장소 구조 설명
[위 자동 생성된 폴더 구조를 바탕으로, 주요 디렉토리와 각 디렉토리의 목적을 간략히 설명해주세요. 이 설명은 AI가 프로젝트를 이해하는 데 큰 도움이 됩니다.]

## 시작하기
- [프로젝트 설정 방법이나 빠른 시작 가이드를 적으세요]
