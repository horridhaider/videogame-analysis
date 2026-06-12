# 🎮 Video Game Review Analysis — Metacritic Dataset

![Python](https://img.shields.io/badge/Python-3.14-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-3.0.3-150458?logo=pandas&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-0.13.2-4C72B0)
![Matplotlib](https://img.shields.io/badge/Matplotlib-3.10.9-orange)
![License](https://img.shields.io/badge/License-MIT-green)

> Exploring the gap between what critics score and what players feel — across genres, studios, ratings, and two decades of game releases.

<div align="center">
  <img src="https://github.com/user-attachments/assets/09d07a45-9f1c-4b29-a64e-1bfbb67beaa8" width="48%"/>
  <img src="https://github.com/user-attachments/assets/dd75ae9f-3a34-4734-b0df-a7626505b52e" width="48%"/>
</div>

---

## 📌 Overview

This project is an end-to-end exploratory data analysis of Metacritic video game reviews.
The central question is deceptively simple: **do critics and players actually agree?**
The analysis unpacks that question across multiple dimensions — genre, developer, ESRB rating,
and release timeline — surfacing patterns that aggregate stats alone would miss.

**Dataset:** 5,000+ games scraped from Metacritic, covering titles from the early 2000s through the mid-2020s.

---

## 🗂️ Repository Structure

```
videogame-analysis/
│
├── data/
│   └── videogame_dataset.csv       # Raw dataset
│
├── plots/                          # All exported visualizations (PNG, 300 DPI)
│   ├── metascores-distribution-hist.png
│   ├── metascore-userscore-scatter.png
│   ├── top-genres-scores-groupbarh.png
│   ├── genre-score-gap-divergingbarh.png
│   ├── developers-highest-metascores-barh.png
│   ├── developers-with-most-games-barh.png
│   ├── distributors-with-most-success-barh.png
│   ├── unstable-developers-lollipop.png
│   ├── metascore-rating-violin.png
│   ├── rating-frequency-dotplot.png
│   ├── scores-trends-across-year-plot.png
│   └── score-trends-across-months-plot.png
│
├── videogame_analysis.ipynb        # Main analysis notebook
└── README.md
```

---

## 🔧 Tech Stack

| Tool | Purpose |
|------|---------|
| `pandas` | Data loading, cleaning, transformation, groupby aggregations |
| `NumPy` | Numerical operations, type coercion |
| `Matplotlib` | Base plotting, figure/axis control, custom styling |
| `Seaborn` | Statistical plots — violin plots, heatmaps |
| `os` | Dynamic path handling, output directory management |

---

## ⚙️ Pipeline & Workflow

### 1 — Data Loading & Initial EDA
- Loaded CSV, inspected dtypes, shape, null counts, and unique values per column
- Renamed columns for consistency and dropped irrelevant fields
- Identified key issues: string values in numeric columns (`"tbd"`, `"first review"`),
  wrong dtypes on `release_date`, `metascore`, and `userscore`

### 2 — Data Cleaning
- Replaced invalid string entries in `metascore` and `userscore` with `NaN`, then dropped those rows
- Converted `release_date` to `datetime64`, extracted `release_year` and `release_month`
- Cast score columns to `float`
- Removed duplicates

### 3 — Multi-Value Column Handling
Genres, developers, and distributors are stored as comma-separated strings (e.g. `"Action, RPG"`).
To avoid inflating stats in the main DataFrame, three separate working copies were created and exploded:

```
genre_df  → genres column exploded into individual rows
dev_df    → developer column exploded
dist_df   → distributor column exploded
```

Each copy is only used for its respective section — preventing cross-contamination between analyses.

### 4 — Exploratory Analysis

**Distribution & Agreement**
- Histogram of metascore distribution to assess skewness
- Critic vs. user scatter plot to visually assess agreement
- Pearson correlation coefficient to quantify the relationship

**Genre Analysis**
- Median metascore and userscore per genre (filtered to genres with 30+ entries)
- Grouped bar chart comparing both scores side by side per genre
- Score gap calculated as `(userscore × 10) − metascore` per game, averaged by genre
- Diverging bar chart: red = critics rate higher, blue = users rate higher

**Developer & Distributor Analysis**
- Top developers by average metascore (filtered to 90+)
- Most prolific developers by game count
- Top distributors by average metascore
- Developer trajectory: year-over-year metascore trend per studio (Pearson correlation of score vs. year, minimum 5 active years)
- Developer inconsistency: standard deviation of metascores — visualized as a lollipop chart

**Rating Analysis**
- Violin plot of metascore distribution per ESRB rating (inner quartile lines)
- Dot plot of rating frequency across the dataset

**Time-Based Analysis**
- Average critic and user scores plotted across release years
- Monthly score trends (mapped to numeric month order)
- Genre count comparison: games tagged with exactly 2 genres vs. 3+

---

## 📊 Key Questions Answered

- Which genres do critics and users systematically disagree on?
- Which developers are consistently high quality vs. volatile across releases?
- Are studios improving or declining in quality over time?
- Does ESRB rating correlate with review performance?
- Do games released in certain months score differently?
- Does having more genre tags help or hurt a game's critical reception?

---

## 📁 Output

All plots are saved automatically to the `plots/` directory at 300 DPI via a custom
`save_figure()` utility defined in the notebook. No manual export steps required.

---

## 🚀 Running the Notebook

1. Clone the repo and navigate to the project folder
2. Place the dataset at `data/videogame_dataset.csv`
3. Install dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn
   ```
4. Launch and run all cells:
   ```bash
   jupyter notebook videogame_analysis.ipynb
   ```

Plots will be saved automatically to `plots/` on execution.

---

*Built by [Haider](https://github.com/horridhaider) · Data Analysis Portfolio*
