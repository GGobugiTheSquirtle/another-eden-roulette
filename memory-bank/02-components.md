# 주요 컴포넌트 및 모듈 상세

이 문서는 프로젝트의 주요 스크립트 내 핵심 함수 및 데이터 구조에 대해 상세히 설명합니다.

## 1. `another_eden_gui_scraper.py`

*   **`scraping_logic(...)`**: 핵심 스크레이핑 함수.
    *   입력: 타겟 URL, 출력 디렉토리, GUI 큐.
    *   처리: 웹 페이지 파싱, 데이터 추출, `download_image` 호출.
    *   출력: `all_character_data_for_final_excel` (리스트), `raw_data_for_structure_analysis` (리스트). 이 데이터는 Excel 및 CSV 파일로 저장됨.
*   **`download_image(image_url, subfolder)`**: 이미지 다운로드 및 저장.
    *   입력: 이미지 URL, 저장할 하위 폴더명 (예: "icons", "elements_equipment").
    *   처리: URL에서 파일명 추출, `character_art/` 내 지정된 하위 폴더에 이미지 저장.
    *   출력: 저장된 로컬 이미지 파일 경로.
*   **출력 Excel (`another_eden_characters_detailed.xlsx`) 주요 컬럼:**
    *   `Icon`: 캐릭터 아이콘 이미지 (openpyxl Image 객체).
    *   `Icon Filename`: 캐릭터 아이콘의 원본 파일명 (예: `101010011_rank5_style_3.png`).
    *   `Name`: 캐릭터명.
    *   `Rarity`: 희귀도.
    *   `Elem/Equip {i} Icon`: 속성/장비 아이콘 이미지 (openpyxl Image 객체).
    *   `Elem/Equip {i} Alt`: 해당 아이콘의 `alt` 태그 값 (예: `Skill Type 8 0.png`, `202000000 icon.png`).
    *   `Release Date`: 출시일.

## 2. `eden_data_preprocess.py`

*   **`main()`**: 전체 전처리 파이프라인 실행.
    *   `another_eden_characters_detailed.xlsx` 로드.
    *   "Buddy equipment" 행 필터링.
    *   각 캐릭터에 대해 정보 추출 및 변환 (아이콘 경로 생성, 이름 매핑).
    *   결과를 `eden_roulette_data.csv`로 저장.
*   **`build_alttext_to_filename_map_from_excel()`**:
    *   입력: `another_eden_characters_detailed.xlsx` (기본값).
    *   처리: Excel의 "Elem/Equip X Alt" 컬럼 값들과 `character_art/elements_equipment/` 내 실제 파일명을 비교하여 매핑 딕셔너리 (`ALT_TEXT_TO_ACTUAL_FILENAME_MAP`) 생성. `difflib`로 유사도 매칭 지원.
    *   출력: `(매핑 딕셔너리, 매칭 실패 alt_text 리스트)`.
*   **`get_char_icon_path(icon_file_name_from_excel)`**: (실제로는 `main` 내에서 직접 경로 조합)
    *   Excel 'Icon Filename'을 받아 `character_art/icons/` 내의 전체 경로 반환.
*   **`get_equip_element_icon_path(alt_text)`**:
    *   입력: alt 텍스트.
    *   처리: `ALT_TEXT_TO_ACTUAL_FILENAME_MAP`을 사용하여 실제 파일명 조회 후 `character_art/elements_equipment/` 내의 전체 경로 반환.
*   **`KOREAN_NAME_MAP` (딕셔너리)**: alt 텍스트 (주로 파일명 형태)를 한글 표시명으로 매핑.
*   **출력 CSV (`eden_roulette_data.csv`) 컬럼:**
    *   `Character Name`: 캐릭터명 (문자열).
    *   `Rarity`: 희귀도 (문자열).
    *   `Character Icon Path`: `character_art/icons/` 내 캐릭터 아이콘의 상대 경로 (문자열).
    *   `Element 1 Name`: 첫 번째 속성 한글명 (문자열, 없으면 "N/A").
    *   `Element 1 Icon Path`: `character_art/elements_equipment/` 내 첫 번째 속성 아이콘의 상대 경로 (문자열, 없으면 "N/A").
    *   `Element 2 Name`: 두 번째 속성 한글명 (문자열, 없으면 "N/A").
    *   `Element 2 Icon Path`: `character_art/elements_equipment/` 내 두 번째 속성 아이콘의 상대 경로 (문자열, 없으면 "N/A").
    *   `Weapon Name`: 무기 한글명 (쉼표로 구분된 문자열 가능, 없으면 "N/A").
    *   `Weapon Icon Path`: `character_art/elements_equipment/` 내 무기 아이콘 상대 경로 (쉼표로 구분된 문자열 가능, 없으면 "N/A").
    *   `Armor Name`: 방어구 한글명 (쉼표로 구분된 문자열 가능, 없으면 "N/A").
    *   `Armor Icon Path`: `character_art/elements_equipment/` 내 방어구 아이콘 상대 경로 (쉼표로 구분된 문자열 가능, 없으면 "N/A").

## 3. `streamlit_eden_restructure.py`

*   **`main()`**: Streamlit 앱의 메인 실행 함수. UI 레이아웃, 데이터 필터링, 컴포넌트 호출 담당.
*   **`load_and_prepare_data(csv_path, column_map_config)`**:
    *   입력: `eden_roulette_data.csv` 경로, `COLUMN_MAP_CONFIG`.
    *   처리: CSV 로드, 컬럼 검증, `Element 1/2 Name`과 `Element 1/2 Icon Path`를 각각 `속성명리스트`(리스트 형태), `속성_아이콘경로리스트`(리스트 형태)로 변환하여 DataFrame에 추가. `@st.cache_data`로 캐싱.
    *   출력: (처리된 DataFrame, 주요 컬럼명 변수들).
*   **`COLUMN_MAP_CONFIG` (딕셔너리)**: `eden_roulette_data.csv`의 영문 컬럼명과 앱 내부에서 사용할 한글 키를 매핑.
    ```python
    # 예시
    COLUMN_MAP_CONFIG = {
        '희귀도': 'Rarity',
        '이름': 'Character Name',
        '캐릭터아이콘경로': 'Character Icon Path',
        '속성1명': 'Element 1 Name',
        '속성1아이콘': 'Element 1 Icon Path',
        # ... 등등
    }
    ```
*   **`display_category_info_for_streamlit(st_obj, os_module, row_data, label_text, name_col_key, icon_col_key, ...)`**:
    *   입력: Streamlit 객체, OS 모듈, 캐릭터 행 데이터, 표시할 카테고리 정보(라벨, 이름 컬럼키, 아이콘 컬럼키).
    *   처리: 이름과 아이콘 경로 (리스트 또는 단일 문자열)를 받아 HTML로 구성하여 표시. 아이콘은 Base64로 인코딩. Flexbox로 스타일링.
    *   출력: `st.markdown`으로 HTML 렌더링.
*   **`slot_machine_display(items, winner_index, ...)`**:
    *   입력: `items` (딕셔너리 리스트 - `{'name': ..., 'icon_base64': ...}`), `winner_index`.
    *   처리: HTML, CSS, JavaScript를 사용하여 슬롯머신 애니메이션 및 당첨자 표시.
    *   출력: `components.html`로 커스텀 컴포넌트 렌더링.
*   **`get_image_base64(image_path)`**: 이미지 파일을 Base64 문자열로 인코딩.

## 4. 이미지 에셋 (`character_art/`)

*   **`icons/`**: 캐릭터 아이콘 이미지 저장.
*   **`elements_equipment/`**: 속성, 무기, 방어구 아이콘 이미지 저장.
*   파일 경로는 `eden_roulette_data.csv`에 상대 경로로 저장되며, Streamlit 앱에서 이 경로를 사용해 직접 파일을 참조하거나 Base64로 인코딩하여 사용.

## 5. 코드 연결성/완결성 체크리스트
- 함수별 입출력 타입, 주요 파라미터/반환값 일치 여부 점검
- 데이터 경로/포맷(엑셀→CSV→Streamlit) 일관성 유지
- 매핑 딕셔너리(속성/무기/방어구 등) 최신화 및 누락 방지
- 이미지/CSV/엑셀 등 파일 존재 여부 자동/수동 점검
- 주요 예외 처리 및 경고 메시지 구현
