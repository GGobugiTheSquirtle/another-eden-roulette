# Another Eden Character Data Pipeline

## Overview
This repository provides **two standalone scripts** that together deliver a full data-collection → preprocessing → web-app workflow for *Another Eden* character information.

| Role | Script | Purpose |
|------|--------|---------|
| **① Scraper + Preprocessor** | `another_eden_gui_scraper copy.py` | GUI tool for internal use. Downloads images, builds Excel, then automatically converts to `eden_roulette_data.csv`. Run whenever you need fresh data. |
| **② Streamlit Web App** | `streamlit_eden_restructure.py` | Public-facing Streamlit app that visualises the CSV (roulette, filters, cards). Deploy this file + generated assets to Streamlit Community Cloud. |

```
└─ project_root/
   ├─ character_art/
   │   ├─ icons/
   │   └─ elements_equipment/
   ├─ eden_roulette_data.csv           ← auto-generated
   ├─ another_eden_gui_scraper copy.py ← internal
   ├─ streamlit_eden_restructure.py    ← web app
   └─ README.md                        ← this guide
```

---
## 1. Update Data (internal)
1. **Run the GUI**
   ```bash
   python another_eden_gui_scraper\ copy.py
   ```
2. Choose an output folder (default: repo root).
3. Click *“최종 보고서 생성”*.
4. The script will:
   - Create/refresh `character_art/*` images (deduplicated by filename).
   - Write `another_eden_characters_detailed.xlsx`.
   - Auto-generate **`eden_roulette_data.csv`** using `eden_data_preprocess_gui.make_roulette_csv_from_excel()`.
5. Logs and a progress-bar indicate status. Errors appear in red.

### Duplicate Images Handling
If a file with the same name already exists it is kept; new duplicates save as `name (1).png`, `name (2).png`, etc.

---
## 2. Run / Deploy Web App
### Local test
```bash
streamlit run streamlit_eden_restructure.py
```

*The app assumes `eden_roulette_data.csv` & `character_art/*` are in the same directory.*

### Deployment (Streamlit Community Cloud)
Upload only:
- `streamlit_eden_restructure.py`
- `eden_roulette_data.csv`
- `character_art/…` folder hierarchy

The app converts local image paths to Base64 at runtime, so no additional hosting is required.

---
## Maintenance Notes
* **Hard-coded mappings** for Element/Weapon/Armor icons reside in `eden_data_preprocess_gui.py`. Update when new equipment appears.
* Duplicate scripts (`another_eden_gui_scraper.py`, `eden_data_preprocess_gui (1)(2).py`) are legacy and can be archived.
* Paths inside the CSV are **relative**; the Streamlit app now resolves them against its own location for portability.

---
## Troubleshooting
| Issue | Fix |
|-------|-----|
| Streamlit shows “CSV 파일이 없습니다” | Run the scraper to regenerate `eden_roulette_data.csv`. |
| Missing images in web app | Ensure corresponding files exist under `character_art/…`; rerun scraper if needed. |
| New element/weapon icon not labelled | Add mapping entry in `eden_data_preprocess_gui.py`. |
