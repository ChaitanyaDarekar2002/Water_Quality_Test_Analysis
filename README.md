# 💧 Water Quality Analysis Dashboard — Indore

An interactive **Power BI** dashboard that monitors drinking water quality across **12 zones of Indore**, flags unsafe samples against **BIS IS 10500** drinking water limits, and links quality with water supply hours and rainfall.

> 📸 **Dashboard preview:**

<img width="1907" height="785" alt="image" src="https://github.com/user-attachments/assets/24658c50-b7d2-4d72-b608-dc0a72d3d94c" />


---

## 📌 Project Overview

Water quality readings are collected daily for each zone. This project cleans and models that data, classifies every sample into a risk level, and presents the results in a single dashboard so that high-risk zones and the parameters causing them can be spotted quickly.

**Key questions answered**

- Which zones have the highest share of unsafe water?
- Which parameters (TDS, turbidity, hardness, etc.) breach limits most often?
- How does quality change over time?
- Is there a relationship between rainfall, supply hours and water quality?

---

## 📂 Dataset

| File | Rows | Description |
|---|---|---|
| `water_quality_labeled.csv` | 1,200 | Daily readings for 12 zones (100 days each, 1 Jan – 10 Apr 2025) with a `risk_level` label |
| `water_quality.csv` | 1,200 | Same readings without the risk label (raw version) |
| `availability.csv` | 4,380 | Daily `supply_hours` and `rainfall` per zone, with `season` (full year 2025) |
| `water_body_supply_data.csv` | 4 | Water sources (Bilawali Tank, Yeshwant Sagar, Sirpur Lake, etc.) with coordinates and the zones they serve |

**Quality parameters:** `pH`, `TDS`, `turbidity`, `hardness`, `chloride`, `fluoride`, `nitrate`

**Zones:** Vijay Nagar, Palasia, Bhawarkuan, Khajrana, Bengali Square, Rau, Rajendra Nagar, Annapurna, Sudama Nagar, Aerodrome, Kanadia, Silicon City

> ℹ️ Water source details in `water_body_supply_data.csv` are project-defined for demonstration purposes.

---

## ⚖️ Risk Classification Logic

Each sample is checked against the BIS acceptable limits:

| Parameter | Acceptable limit |
|---|---|
| pH | 6.5 – 8.5 |
| TDS | ≤ 500 mg/L |
| Turbidity | ≤ 1 NTU |
| Hardness | ≤ 200 mg/L |
| Chloride | ≤ 250 mg/L |
| Fluoride | ≤ 1.0 mg/L |
| Nitrate | ≤ 45 mg/L |

The number of parameters outside their limit decides the risk level:

| Parameters out of limit | Risk level |
|---|---|
| 0 | 🟢 Safe |
| 1 – 2 | 🟡 Moderate Risk |
| 3 or more | 🔴 High Risk |

**DAX implementation**

```DAX
Violations =
  IF(Quality[pH] < 6.5 || Quality[pH] > 8.5, 1, 0)
+ IF(Quality[TDS] > 500, 1, 0)
+ IF(Quality[turbidity] > 1, 1, 0)
+ IF(Quality[hardness] > 200, 1, 0)
+ IF(Quality[chloride] > 250, 1, 0)
+ IF(Quality[fluoride] > 1, 1, 0)
+ IF(Quality[nitrate] > 45, 1, 0)

risk_level =
SWITCH(TRUE(),
    Quality[Violations] = 0, "Safe",
    Quality[Violations] <= 2, "Moderate Risk",
    "High Risk")
```

---

## 🔍 Key Findings

- Overall sample split: **57% High Risk, 27% Safe, 16% Moderate Risk**.
- *(Add your own insights after exploring the dashboard — e.g. the worst zone, the most frequently breached parameter, rainfall impact on turbidity.)*

---

## 🧱 Data Model

```
Zone (dimension) ──< Quality (fact)
        │
        ├──< Availability (fact)
        └──< Water Body Supply
Date (dimension) ──< Quality / Availability
```

- Relationships are one-to-many from the Zone and Date dimensions.
- A Date table is used for time intelligence.

---

## 📊 Dashboard Features

- **Zone selector** to filter the whole report
- **KPI tiles** for pH, TDS, turbidity, hardness, chloride, fluoride and nitrate with status colours
- **Safe % gauge** showing the share of safe samples
- **Trend chart** of any selected parameter over time
- **Risk by zone** bar chart
- **Supply & rainfall** panel from the availability data
- **Map** of water sources and served zones
- Dark glassmorphism theme

---

## 🛠️ Tools & Skills

- Power BI Desktop (Power Query, DAX, data modelling)
- Python / pandas (data validation and profiling)
- Data visualization and dashboard design

---

## 🚀 How to Use

1. Clone the repository
   ```bash
   git clone https://github.com/ChaitanyaDarekar2002/<repo-name>.git
   ```
2. Open `WaterQuality.pbix` in Power BI Desktop.
3. If prompted, update the data source path to the `data/` folder (*Transform data → Data source settings*).
4. Refresh and explore.

---

## 📁 Repository Structure

```
├── data/
│   ├── water_quality.csv
│   ├── water_quality_labeled.csv
│   ├── availability.csv
│   └── water_body_supply_data.csv
├── images/
│   └── dashboard.png
├── WaterQuality.pbix
└── README.md
```

---

## 👤 Author

**Chaitanya Darekar**  
Data Analyst · SQL · Python · Power BI · Excel

- GitHub: [ChaitanyaDarekar2002](https://github.com/ChaitanyaDarekar2002)
- LinkedIn: [chaitanyadarekar](https://www.linkedin.com/in/chaitanyadarekar02)
- Portfolio: [chaitanyadarekar2002.github.io](https://chaitanyadarekar2002.github.io)
- Email: chaitanyadarekar2002@gmail.com
