# 개발 프로세스

## 1. 작업 흐름
- 데이터 수집: `another_eden_gui_scraper.py`로 위키에서 데이터/이미지 수집
- 데이터 전처리: `eden_data_preprocess.py`로 엑셀→CSV 변환, 경로/이름 매핑
- 웹앱 개발: `streamlit_eden_restructure.py`로 Streamlit UI, 데이터 필터/룰렛 구현

## 2. 브랜치 전략
- 메인 브랜치: `main` (배포/운영)
- 개발 브랜치: `dev` (기능 개발, 통합 전 테스트)
- 기능별 브랜치: `feature/XXX` (주요 기능 단위 개발)

## 3. 협업 및 코드 리뷰
- PR(Pull Request) 기반 코드 리뷰 필수
- 주요 변경/이슈는 `05-progress-log.md`에 기록

## 4. 테스트 및 배포
- 데이터/이미지 누락, 경로 불일치 등 수동/자동 점검
- Streamlit 앱은 로컬에서 충분히 테스트 후 배포
- 주요 배포/릴리즈는 `05-progress-log.md`에 기록
