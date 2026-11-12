# ShelfIQ // Perishable Spoilage Prediction & Restock Decision System

> A collaborative Streamlit decision-support application and Machine Learning pipeline that predicts perishable inventory spoilage risk, estimates Remaining Useful Life (RUL), and triggers automated rescue-pricing and markdown actions for Philippine retail grocers and cold-chain distributors.

![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=flat&logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-1.35+-FF4B4B?style=flat&logo=streamlit&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-Random%20Forest-F7931E?style=flat&logo=scikit-learn&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-Forecasting-blue?style=flat)
![Uplift Code Camp](https://img.shields.io/badge/Capstone-Python%20for%20Data%20%26%20AI-success?style=flat)

---

## 📋 Overview

### The Problem
Mid-size grocery stores, chilled food retailers, and perishable distributors in tropical climates face recurring inventory shrinkage. Ambient temperature spikes and cold-chain bottlenecks accelerate spoilage, causing stock write-offs, margin loss, and food waste when near-expiry items are discovered too late for discount interventions.

### The Solution
**ShelfIQ** centralizes master inventory telemetry and trains machine learning models on live stock data to:
1. **Predict Spoilage Likelihood:** Classifies batches based on velocity, shelf-life runway, supplier lead time, and product perishability sensitivity.
2. **Prioritize Intervention Tiers:** Categorizes at-risk inventory into actionable tiers (Critical, High Risk, Moderate, Stable).
3. **Automate Rescue Actions:** Recommends targeted markdown discounts, promotional bundles, or upcycling protocols before inventory reaches zero value.
4. **Forecast Future Demand:** Models product velocity to avoid over-ordering high-risk perishables.

---

## 🏛️ Application Architecture & Modules

```
ShelfIQ/
├── app.py                      # Multi-page Streamlit application shell & sidebar data loader
├── assets/                     # Brand identity assets & application logos
├── capstone_dataset_2years (1).csv # 2-year capstone telemetry dataset (12,500+ records)
├── pages/
│   ├── dashboard.py            # Executive KPI overview & Plotly expiry timeline
│   ├── rul_model.py            # Spoilage Prediction Engine (Random Forest classifier)
│   ├── marketing.py            # Marketing action & markdown discount tier generator
│   └── forecasting.py          # Sales demand forecasting engine (XGBoost Regressor)
├── requirements.txt            # Python runtime dependencies
└── README.md                   # Technical system documentation
```

### Module Breakdown

| Page / Component | Core Capabilities & Algorithms |
| :--- | :--- |
| **`app.py`** | Central layout navigation, custom dark navy/green dashboard styling, and unified sidebar CSV uploader providing session-state access across pages. |
| **`pages/rul_model.py`** | **Core Spoilage Engine:** Trains a `RandomForestClassifier` (100 estimators) on `stock_to_sell`, `days_to_sell`, `daily_demand`, `supplier_lead_time_days`, and `sensitivity_score`. Computes feature importance rankings, evaluates batch-level Remaining Useful Life (RUL), and flags items into action tiers. |
| **`pages/dashboard.py`** | Executive inventory overview: Real-time calculation of spoiled items, inventory value at risk, overall spoilage rate, category breakdown, supplier reliability benchmarks, and interactive Plotly expiration timeline. |
| **`pages/marketing.py`** | Operational action engine translating ML spoilage risk into specific rescue operations: immediate markdown percentages (10%–40%), bundle promotions, and food waste diversion/upcycling recommendations. |
| **`pages/forecasting.py`** | Time-series replenishment planning using `XGBRegressor` to forecast SKU-level demand and visualize order schedules using `streamlit_calendar`. |

---

## 🛠️ Tech Stack

- **Framework:** [Streamlit](https://streamlit.io/) (Multi-page App Architecture, Session State Management)
- **Machine Learning & Modeling:** [Scikit-learn](https://scikit-learn.org/) (`RandomForestClassifier`), [XGBoost](https://xgboost.readthedocs.io/) (`XGBRegressor`)
- **Data Manipulation:** [Pandas](https://pandas.pydata.org/), [NumPy](https://numpy.org/)
- **Data Visualization:** [Plotly](https://plotly.com/python/), [Matplotlib](https://matplotlib.org/)
- **UI Components:** `streamlit-calendar`

---

## 👥 Attribution & Team Contribution

ShelfIQ was developed as the **Capstone Project for Uplift Code Camp** (*Python for Data and AI*).

- **Jan Gabriel Chavez ([@jgmchavez-22](https://github.com/jgmchavez-22)):**
  - Engineered the core **Spoilage & Expiry Prediction Engine** ([`pages/rul_model.py`](pages/rul_model.py)).
  - Implemented the `RandomForestClassifier` training pipeline, feature importance extraction, and multi-tier spoilage risk classification logic.
  - Structured the session-state telemetry handoff between model inference and marketing action rules.
- **Capstone Collaborators:**
  - **Ruth Ebbuan** ([@mrebbuan-8](https://github.com/mrebbuan-8)) — Project architecture & coordination.
  - **Euie David** ([@euiedavid](https://github.com/euiedavid)) — Analytics dashboard & data pipeline.
  - **EJ Revecho** ([@erevecho](https://github.com/ejrevecho)) — Forecasting engine & marketing actions.
- **Upstream Repository:** [`mrebbuan-8/ShelfIQ`](https://github.com/mrebbuan-8/ShelfIQ)

---

## 🚀 Getting Started

### Prerequisites

- Python 3.9, 3.10, or 3.11 installed.
- `pip` package manager.

### Installation

1. **Clone the repository:**
   ```bash
   git clone git@github.com:jgmchavez-22/ShelfIQ.git
   cd ShelfIQ
   ```

2. **Create and activate a virtual environment (recommended):**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

### Running the Application

1. **Launch Streamlit:**
   ```bash
   streamlit run app.py
   ```

2. **Access the Web Interface:**
   The application will automatically open in your default browser at:
   ```text
   http://localhost:8501
   ```

3. **Load Telemetry Data:**
   Use the **📥 Global Master Data** file uploader in the sidebar to upload your store dataset or select the bundled sample dataset (`capstone_dataset_2years (1).csv`).

---

## 📊 Sample Telemetry Schema

The ML pipeline expects tabular inventory data containing the following standard retail fields:

| Field Name | Type | Description |
| :--- | :--- | :--- |
| `item_sku` | String | Unique product identifier |
| `product_name` | String | Commercial product title |
| `product_category` | String | Category (Dairy, Produce, Beverages, etc.) |
| `quantity_received` | Integer | Batch intake quantity |
| `arrival_date` | Date | Warehouse / store delivery date |
| `expiry_date` | Date | Batch expiration date |
| `daily_demand` | Float | Average daily sales velocity |
| `supplier_lead_time_days` | Integer | Restock replenishment lead time |
| `spoilage_sensitivity` | Categorical | Perishability rating (`Low`, `Medium`, `High`) |
| `items_spoiled` | Integer | Historical spoiled count (used for model ground truth) |

---

## ⚠️ Known Limitations

- **Model Training on Master Upload:** The model trains dynamically in-memory when the dataset is loaded in the session; production enterprise deployments would benefit from pre-trained serialized model artifacts (`joblib` / `onnx`).
- **Cold-Chain Sensor Integration:** Current inputs rely on tabular inventory and transit duration; dynamic IoT temperature/humidity sensor streams are not currently integrated.
