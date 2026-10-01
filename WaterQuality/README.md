# Aquify — Water Quality Classification Website

A complete, self-contained website that classifies a water sample as
**safe** or **unsafe** to drink, using two pre-trained ML models
(SVM and Random Forest).

```
WaterQualityWebsite/
├── frontend/              # Website UI (plain HTML/CSS/JS, no build step)
│   ├── index.html
│   ├── style.css
│   └── script.js
├── backend/                 # Flask REST API
│   ├── app.py
│   └── requirements.txt
├── database/                 # SQLite database setup
│   ├── schema.sql
│   └── seed_database.py
├── models/                    # Pre-trained model files
│   ├── svm_model.pkl
│   ├── random_forest_model.pkl
│   └── scaler.pkl
├── src/                       # Original ML workflow files 01–09
│   ├── 01_load_dataset.py
│   ├── ...
│   └── 09_prediction.py
├── data/                       # Dataset used to train the models
│   ├── water_quality.csv
│   └── model_results.csv
└── README.md
```

## How it works

`backend/app.py` loads the three `.pkl` files in `models/`, exposes a
REST API on `http://localhost:5000`, and also serves the `frontend/`
folder — so the whole site runs from one command. Every prediction made
through the site is saved to `database/water_quality.db` (SQLite), which
also stores the training dataset (for the dashboard) and the models'
evaluation scores.

## Run it in VS Code

**1. Open this folder** in VS Code (`File → Open Folder…`).

**2. Open a terminal** (`` Ctrl+` ``) and create a virtual environment:

```bash
python -m venv venv
venv\Scripts\activate         # Windows PowerShell
```

**3. Install the backend dependencies:**

```bash
pip install -r backend/requirements.txt
```

**4. Build the database** (run once — or again any time to reset it):

```bash
python database/seed_database.py
```

**5. Start the server:**

```bash
python backend/app.py
```

**6. Open the website:** go to **http://localhost:5000**

That's it — the prediction form, dataset dashboard, model performance
table, and prediction history are all live.

## Original ML workflow

The original scripts are preserved in `src/` for reference. The website
uses the already-saved models in `models/` and does not retrain them.
Scripts `01`–`08` expect the intermediate `Dataset/`, `results/`, and
training-split files produced by the original workflow; those files are
not required to run the website. Script `09_prediction.py` can use the
saved model files directly, while `src/app.py` is the original Streamlit
application.

## API endpoints (backend/app.py)

| Method | Endpoint        | Description                                    |
|--------|-----------------|-------------------------------------------------|
| GET    | `/api/health`   | Health check                                     |
| GET    | `/api/features` | The 20 input parameters + units                  |
| POST   | `/api/predict`  | Send 20 values, get SVM + Random Forest results  |
| GET    | `/api/history`  | Recent predictions logged by the site            |
| GET    | `/api/metrics`  | Model accuracy / precision / recall / F1         |
| GET    | `/api/stats`    | Dataset-wide stats (safe/unsafe split, averages) |
| GET    | `/api/samples`  | Random sample rows (for "Load a random sample")  |

## Notes

- The two models sometimes disagree on a borderline sample — when that
  happens the site's overall verdict uses the Random Forest result (it
  scored a slightly higher F1 in `data/model_results.csv`), and says so.
- Predictions are a statistical estimate for exploratory use, not a
  certified water-safety test.


  "commands to run "
  python 
  # 1) (Optional) verify you are in the right folder
Get-Location

# 2) create virtual environment
python -m venv venv

# 3) activate it
.\venv\Scripts\Activate.ps1

# 4) install dependencies
pip install -r .\backend\requirements.txt

# 5) initialize/reset database (run once, or rerun to reset)
python .\database\seed_database.py

# 6) start backend + frontend server
python .\backend\app.py


