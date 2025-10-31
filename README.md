# generate_readme.py
# ✅ Creates README.md + copies visuals to /assets (auto-generates placeholders if missing)
# Author: Sayer Bin Shafi

import os
import shutil
import matplotlib.pyplot as plt
from textwrap import dedent

# ========== CONFIGURATION ==========
PROJECT_TITLE = "🏦 Business Failure Prediction using FDIC & BLS Data"
PERIOD = "2000–2024"
ASSETS_DIR = "assets"
README_NAME = "README.md"

BADGES = [
    '<img src="https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white"/>',
    '<img src="https://img.shields.io/badge/Power%20BI-Dashboard-yellow?logo=powerbi"/>',
    '<img src="https://img.shields.io/badge/Scikit--Learn-ML-orange?logo=scikit-learn"/>',
    '<img src="https://img.shields.io/badge/Status-Completed-brightgreen"/>'
]

IMAGES = {
    "failures_over_time": "output_13_0.png",
    "top_states": "output_15_0.png",
    "survival_curves": "output_19_0.png",
    "correlation": "output_21_0.png",
    "what_if": "output_25_0.png",
    "regression": "output_29_0.png"
}

POWER_BI_URL = "https://github.com/user-attachments/assets/278c99a8-0ba3-4ffd-a379-47a1124e9bae"

# ========== FUNCTIONS ==========

def ensure_assets():
    """Create /assets directory and copy or create placeholder visuals."""
    os.makedirs(ASSETS_DIR, exist_ok=True)

    for label, file_name in IMAGES.items():
        src = os.path.join(os.getcwd(), file_name)
        dst = os.path.join(ASSETS_DIR, file_name)

        if os.path.exists(src):
            shutil.copy2(src, dst)
        else:
            # create placeholder
            plt.figure(figsize=(6, 3))
            plt.plot(range(1, 6), [i * 1.5 for i in range(1, 6)], marker="o")
            plt.title(f"Placeholder for {label.replace('_', ' ').title()}")
            plt.tight_layout()
            plt.savefig(dst)
            plt.close()


def make_img_block():
    """Generate markdown blocks for visuals."""
    titles = {
        "failures_over_time": "🏦 Bank Failures Over Time",
        "top_states": "🌍 Top 10 States by Failures",
        "survival_curves": "📉 Business Survival Curves by Cohort",
        "correlation": "🔍 Correlation: Bank Failures vs Survival Rates",
        "what_if": "📈 What-if Scenario: Survival Rate Improvement",
        "regression": "📊 Regression & Employment Insights"
    }

    blocks = []
    for key, title in titles.items():
        path = f"{ASSETS_DIR}/{IMAGES[key]}"
        block = f"### {title}\n<img src='{path}' width='90%' alt='{title}'/>\n"
        blocks.append(block)
    return "\n---\n\n".join(blocks)


def create_readme():
    """Build the README markdown content."""
    badges = "\n  ".join(BADGES)
    visuals = make_img_block()

    readme_content = f"""
<h1 align="center">{PROJECT_TITLE}</h1>

<p align="center">
  {badges}
</p>

<p align="center">
  <b>Analyzing two decades of U.S. business and bank failures using data science & machine learning.</b><br>
  <i>Integrating FDIC + BLS datasets to uncover trends, correlations, and predictive signals of economic distress.</i><br>
  <i>📅 {PERIOD}</i>
</p>

---

## 🌐 Project Overview
This project builds an AI-driven framework to analyze and predict **U.S. business failures** ({PERIOD}).  
By merging **FDIC bank failure data** and **BLS business survival data**, the study identifies early indicators of financial distress to support **regulators**, **policy-makers**, and **investors**.

---

## 📊 Datasets

| Source | Description | Key Features |
|:--|:--|:--|
| 🏛️ [FDIC Failed Bank List](https://www.fdic.gov/resources/resolutions/bank-failures/failed-bank-list/) | Records of U.S. bank closures and acquisitions | Bank name, state, closing date, acquirer |
| 📈 [BLS Business Dynamics](https://www.bls.gov/bdm/bdmage.htm) | Establishment openings, closures & survival rates | Cohort year, survival %, employment size |

---

## ⚙️ Workflow

```mermaid
graph TD
A[Data Collection<br>FDIC + BLS] --> B[Data Cleaning<br>Standardize & Merge]
B --> C[Exploratory Data Analysis<br>Trends + State-level Insights]
C --> D[Predictive Modeling<br>Logistic, RF, XGBoost]
D --> E[Visualization<br>Power BI Dashboard + Python Plots]
E --> F[Key Insights<br>Business Risk Detection]
