# 아키텍처 문서

## 시스템 아키텍처

## 1. 개요

본 프로젝트는 데이터 수집, 데이터 전처리, 웹 애플리케이션의 세 가지 주요 단계로 구성됩니다. 각 단계는 독립적인 Python 스크립트로 구현되어 있으며, 파일 시스템을 통해 데이터를 주고받습니다.

## 2. 컴포넌트 다이어그램 및 데이터 흐름

```
[Web: anothereden.wiki/w/Characters]
       |
       | (HTML, Images)
       v
[1. 데이터 수집: another_eden_gui_scraper.py]
  - 역할: 웹 스크레이핑, 이미지 다운로드
  - 기술: Python, Requests, BeautifulSoup, Tkinter
  - 출력: character_art/ (이미지 파일), another_eden_characters_detailed.xlsx
       |
       | (another_eden_characters_detailed.xlsx, character_art/)
       v
[2. 데이터 전처리: eden_data_preprocess.py]
  - 역할: Excel 데이터 파싱, 아이콘 경로/이름 매핑, 데이터 정제 및 구조화
  - 기술: Python, Pandas, Openpyxl
  - 출력: eden_roulette_data.csv
       |
       | (eden_roulette_data.csv, character_art/)
       v
[3. 웹 애플리케이션: streamlit_eden_restructure.py]
  - 역할: 데이터 시각화, 사용자 인터랙션(필터, 룰렛), 웹 UI 제공
  - 기술: Python, Streamlit, Pandas, HTML/CSS/JS (for custom components)
       |
       | (HTML/CSS/JS over HTTP)
       v
[사용자 웹 브라우저]
```

## 3. 주요 컴포넌트 설명

### 3.1. `another_eden_gui_scraper.py`

*   **책임:** Another Eden 위키에서 캐릭터 정보를 수집하고 원본 데이터를 생성합니다.
*   **기능:**
    *   Tkinter 기반 GUI 제공.
    *   지정된 URL에서 HTML을 가져와 파싱.
    *   캐릭터 아이콘 및 속성/장비 아이콘 다운로드 (`character_art/` 폴더에 저장).
    *   수집된 정보를 `another_eden_characters_detailed.xlsx` 파일로 저장 (이미지 포함).
    *   (선택적) `structure_analysis.csv` 생성.

### 3.2. `eden_data_preprocess.py`

*   **책임:** 스크레이핑된 원본 데이터를 Streamlit 애플리케이션이 사용하기 쉬운 형태로 가공합니다.
*   **기능:**
    *   `another_eden_characters_detailed.xlsx` 로드.
    *   "Buddy equipment" 등 불필요 데이터 제거.
    *   캐릭터 아이콘, 속성/장비 아이콘의 로컬 파일 경로 생성 및 검증.
    *   아이콘 파일명/alt 태그를 한글 이름으로 변환 (사전 정의된 맵 사용).
    *   최종적으로 `eden_roulette_data.csv` 파일 생성.
        *   컬럼: `Character Name`, `Rarity`, `Character Icon Path`, `Element 1 Name`, `Element 1 Icon Path`, `Element 2 Name`, `Element 2 Icon Path`, `Weapon Name`, `Weapon Icon Path`, `Armor Name`, `Armor Icon Path`.

### 3.3. `streamlit_eden_restructure.py`

*   **책임:** 사용자에게 웹 인터페이스를 제공하고, 캐릭터 정보 조회 및 룰렛 기능을 수행합니다.
*   **기능:**
    *   `eden_roulette_data.csv` 로드 및 캐싱 (`@st.cache_data`).
    *   `COLUMN_MAP_CONFIG`를 사용해 CSV 컬럼을 내부적으로 관리.
    *   사이드바를 통해 필터링 옵션(희귀도, 속성, 무기, 방어구, 이름) 제공.
    *   필터링된 결과를 캐릭터 카드 형태로 동적 표시.
        *   이미지는 Base64로 인코딩되어 HTML `<img>` 태그에 사용되거나 `st.image`로 표시.
    *   HTML/CSS/JavaScript 기반의 커스텀 슬롯머신 컴포넌트 구현.
    *   세션 상태(`st.session_state`)를 활용하여 룰렛 결과 등 상태 관리.

## 4. 데이터 저장소

*   **이미지 에셋:** `character_art/` 디렉토리 (하위: `icons/`, `elements_equipment/`)
    *   스크레이퍼가 다운로드하고, 전처리기와 Streamlit 앱이 참조.
*   **중간/최종 데이터:**
    *   `another_eden_characters_detailed.xlsx`: 스크레이퍼의 원본 출력. 전처리기의 입력.
    *   `eden_roulette_data.csv`: 전처리기의 출력. Streamlit 앱의 주 입력.
*   **설정 파일:**
    *   `scraper_config.ini`: 스크레이퍼 출력 디렉토리 설정.

## 5. 디자인 패턴 및 결정사항

*   **파이프라인 아키텍처:** 각 주요 기능(수집, 전처리, 표시)을 별도의 스크립트로 분리하여 모듈성을 높임.
*   **파일 기반 데이터 교환:** 스크립트 간 데이터 전달은 CSV 및 Excel 파일을 통해 이루어짐.
*   **Streamlit 활용:** 빠른 UI 개발 및 데이터 시각화를 위해 Streamlit 채택.
*   **Base64 인코딩:** Streamlit 앱 내 커스텀 HTML 컴포넌트에서 이미지 깨짐 문제를 방지하고 안정적인 이미지 표시를 위해 사용.

## 설계 패턴
- [프로젝트에 사용된 주요 설계 패턴과 적용된 부분을 설명하세요]

## 데이터 흐름
[시스템 내에서 데이터가 어떻게 흐르는지 설명하세요]

## 보안 고려 사항
[적용된 보안 조치나 중요한 보안 관련 사항을 기록하세요]

## 데이터베이스 스키마
[데이터베이스 구조나 관계를 설명하거나 다이어그램을 첨부하세요]

## 기술 결정
[프로젝트 진행 중 내려진 중요한 기술적 결정과 그 이유를 기록하세요]

## 6. 코드 연결성 및 완결성 체크

- 각 단계(스크레이퍼→전처리→웹앱) 간 데이터 포맷/경로 일치 여부 수시 점검
- 이미지/CSV/엑셀 등 파일 누락, 경로 불일치 발생 시 예외 처리 및 경고 메시지 구현
- 주요 함수/컴포넌트 간 입출력 타입, 매핑 딕셔너리 등 일관성 유지
- Streamlit 앱 내 상태 관리(`st.session_state`) 및 캐싱(`@st.cache_data`) 활용
- 데이터/에셋 변경 시 전체 파이프라인 테스트 필수
