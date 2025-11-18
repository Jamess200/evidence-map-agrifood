# Evidence Map for Agrifood (EU Network)

A reproducible pipeline that:
- ingests an Excel participant list,
- cleans and profiles the data (EDA),
- and builds an interactive world map + an accessible HTML summary.

**Live site (GitHub Pages)**  
- Interactive map: https://jamess200.github.io/evidence-map-agrifood/interactive_country_map.html  
- Accessible summary page: https://jamess200.github.io/evidence-map-agrifood/map_accessible.html  
- Landing page: https://jamess200.github.io/evidence-map-agrifood/

> Built with Python, Pandas, Plotly, and a robust country name matcher (exact → alias → fuzzy).

---

## ✨ Highlights

- **Reproducible**: one command builds clean CSVs + EDA + web pages.
- **Robust joining**: matches countries by GeoJSON names (works for Kosovo) via:
  1) case-insensitive exact match → 2) alias table → 3) fuzzy match (RapidFuzz if available).
- **Accessible output**: a keyboard/screen-reader friendly HTML summary with data tables.
- **Zero/Non-zero choropleth layering**: every country stays visible; non-zeros pop.
- **Transparent QA**: writes `name_match_review.csv` and `unmapped_entities.csv` for quick fixes.

---

## 🗂️ Repository layout

```
evidence-map-agrifood/
├── data/
│   ├── geo/                # GeoJSON shapes
│   │   └── countries.geojson.json
│   ├── raw/                # input Excel here
│   │   └── CA23107participant list.xlsx
│   └── processed/          # auto-created by scripts
│       └── data_clean.csv
├── docs/                   # GitHub Pages site (published)
│   ├── index.html
│   ├── interactive_country_map.html
│   ├── map_accessible.html
│   ├── style.css
│   └── .nojekyll
├── outputs/                # EDA + diagnostics
│   ├── eda_overview.csv
│   ├── name_match_review.csv
│   └── unmapped_entities.csv # (Only if unmapped exist)
├── scripts/
│   ├── EDA.py         # Excel → raw+clean CSVs + long-form 
│   └── make_map.py         # Builds map pages (writes to docs/)
├── requirements.txt
├── LICENSE
└── README.md
```

---

## 🚀 Quickstart

### 1) Clone & set up
```bash
git clone https://github.com/jamess200/evidence-map-agrifood.git
cd evidence-map-agrifood

# (optional but recommended)
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS/Linux
source .venv/bin/activate

pip install -r requirements.txt
```

### 2) Drop your input
Place your Excel file in `data/raw/`.  
If your file is named exactly `CA23107participant list.xlsx`, the EDA script will pick it up automatically.

### 3) Build clean data + EDA
```bash
python scripts/EDA.py
```
Outputs:
- `data/raw/data_raw.csv`
- `data/processed/data_clean.csv`
- `outputs/eda_overview.csv`

### 4) Build the map pages
```bash
python scripts/make_map.py
```
Outputs:
- `docs/interactive_country_map.html`
- `docs/map_accessible.html`
- diagnostics in `outputs/`:
  - `name_match_review.csv` (exact/alias/fuzzy decision for each name)
  - `unmapped_entities.csv` (labels we intentionally don’t map, e.g., “European RTD Organisations”)

---

## 🌍 How it works (mapping)

- We **join by GeoJSON country names** (e.g., `properties.name`/`ADMIN`), which handles Kosovo reliably.
- Name resolution order:
  1. **Exact (case-insensitive)**  
  2. **Alias table** (UK→United Kingdom, Turkey/Türkiye→Turkey, Macedonia→North Macedonia, etc.)  
  3. **Fuzzy** (RapidFuzz→WRatio; difflib fallback) with a conservative threshold.
- Working Group labels show as **“Working Group (WG) N”** on the map, but remain **wg1, wg2…** in tables.

---

## 🧪 Re-running with a different file/sheet

```bash
# Use a specific Excel file and sheet (index or name)
python scripts/EDA.py --input data/raw/your_file.xlsx --sheet 0

# Then rebuild map pages
python scripts/make_map.py
```

---

## 🌐 Publish to GitHub Pages

1. Repo → **Settings → Pages**  
   - Source: **Deploy from a branch**  
   - Branch: `main` • Folder: `/docs`
2. Ensure these files exist:
   - `docs/index.html` (landing)
   - `docs/interactive_country_map.html`
   - `docs/map_accessible.html`
   - `docs/.nojekyll` (empty file)
3. Commit & push. Your site will publish at:  
   `https://<your-username>.github.io/evidence-map-agrifood/`

---

## 🧩 Troubleshooting

- **404 on the site** → Make sure Pages points to `/docs` and `docs/index.html` exists (case sensitive).
- **Map has gaps/mismatches** → Check `outputs/name_match_review.csv` and `outputs/unmapped_entities.csv`.  
  Add or tweak aliases in `scripts/make_map.py` if needed.
- **Jupyter throws unknown \`--f=…\` arg** → The EDA script ignores unknown args automatically; run it from a terminal for clean logs.
- **Excel reading errors** → Install engines: `pip install openpyxl` (for .xlsx) or `xlrd` (legacy .xls).

---

## 🗺️ Roadmap (nice-to-haves)

- Add a small UI to edit/extend the alias table from `name_match_review.csv`.
- Country drilldowns: list members per country in the accessible page.
- Add unit tests for name matching and column detection.

---

## Contributing

Issues and PRs welcome. If you spot a country naming edge case, add it to the alias map and include a minimal failing example.

---

## License

MIT — see `LICENSE`.

---

## 👋 About & Links

Project by **James Simmill**.  
- LinkedIn: *(www.linkedin.com/in/james-simmill-a2459a194)*
