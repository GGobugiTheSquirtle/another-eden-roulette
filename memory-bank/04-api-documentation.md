# API 문서

## 1. 데이터 입출력 포맷
- 입력: `eden_roulette_data.csv` (컬럼: Character Name, Rarity, Character Icon Path, Element 1/2 Name/Icon Path, Weapon/Armor Name/Icon Path)
- 출력: Streamlit UI(카드, 룰렛, 필터 등)

## 2. 주요 함수/컴포넌트
- `load_and_prepare_data(csv_path, column_map_config)`: CSV 로드, 컬럼 매핑, DataFrame 반환
- `display_category_info_for_streamlit(...)`: 카테고리별(속성/무기/방어구) 정보 카드 렌더링
- `slot_machine_display(items, winner_index, ...)`: 슬롯머신 애니메이션 및 당첨자 표시
- `get_image_base64(image_path)`: 이미지 Base64 인코딩

## 3. 상태 관리
- Streamlit `st.session_state`로 룰렛 결과, 필터 상태 등 관리
