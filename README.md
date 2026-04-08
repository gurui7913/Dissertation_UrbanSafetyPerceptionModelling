# Urban Safety Perception Modelling

> **Polynomial Regression Analysis of Spatial Metrics and Visual Features in London Streetscapes — Discovering Non-Linear Relationships and Interaction Effects**

This repository contains the data analysis code for a research project that investigates how 2D spatial configurations (via Space Syntax) and 3D visual features (via semantic segmentation of street-view images) jointly influence perceived safety in London's urban environments.

## Research Overview

Urban safety perception is a critical factor influencing people's ability to move around and interact in public spaces. While previous studies have explored spatial configuration (Space Syntax) and visual perception (street-view analysis) separately, this project bridges the gap by combining both approaches within a unified analytical framework.

### Key Findings

- **Weak linear correlations** exist between individual predictors and perceived safety (baseline R² ≈ 0.017)
- **Non-linear (polynomial) models** significantly improve explanatory power (R² ≈ 0.081)
- **Inverted U-shaped relationships**: moderate levels of integration, greenery, and sky openness are associated with higher safety scores, while extreme values reduce perceived safety
- **Interaction effects**: spatial connectivity has a stronger positive impact when accompanied by sufficient visual openness and green space coverage

## Data Sources

| Source | Description |
|--------|-------------|
| **Place Pulse 2.0** | Crowdsourced safety perception scores (~900 London locations) |
| **Space Syntax Open Mapping** | Spatial configuration metrics (Integration & Choice at 2km radius) |
| **SegFormer-B0 (ADE20K)** | Semantic segmentation of street-view images for green view ratio & sky visibility |

## Models

The project implements four regression models with increasing complexity:

### 1. Multiple Linear Regression (Baseline)

$$Y = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + \beta_3 X_3 + \beta_4 X_4 + \varepsilon$$

### 2. Polynomial Regression (Degree 2)

$$Y = \beta_0 + \sum_{i=1}^{4} \beta_i X_i + \sum_{i=1}^{4} \gamma_i X_i^2 + \sum_{i=1}^{3} \sum_{j=i+1}^{4} \delta_{ij} X_i X_j + \varepsilon$$

### 3. Interaction Effect Model

Focuses on six theoretically motivated interaction terms without quadratic terms.

### 4. Enhanced Linear Model with Targeted Interactions

Combines linear effects with specific, theory-driven interaction terms (e.g., INT2K × green_view_ratio, green_view_ratio × sky_visibility).

**Variables:**
- `X₁` = INT2K (Integration at 2km radius)
- `X₂` = CH2K (Choice at 2km radius)
- `X₃` = green_view_ratio
- `X₄` = sky_visibility
- `Y` = safer_Trueskill_Scores (safety perception)

## Repository Structure

```
02_DataAnalysis/
├── Formula.ipynb          # Mathematical model formulations & notation reference
├── SS_Model_V1.ipynb      # Space Syntax baseline regression analysis
│                            - Multiple regression (INT2K + CH2K → Safety)
│                            - Correlation analysis & VIF multicollinearity check
│                            - Residual diagnostics & outlier detection
│                            - Visualization (distributions, scatter plots, heatmaps)
└── min_max.ipynb           # Extreme value analysis
                             - Identifies max/min for all key variables
                             - Exports extreme values to CSV for qualitative case study
```

## Quick Start

### Prerequisites

```bash
pip install pandas numpy matplotlib seaborn statsmodels scikit-learn scipy
```

### Running the Analysis

1. **Baseline Regression** — Open `SS_Model_V1.ipynb` to run the Space Syntax multiple regression model
2. **Extreme Values** — Open `min_max.ipynb` to identify and export extreme cases for qualitative analysis
3. **Model Reference** — See `Formula.ipynb` for all mathematical formulations

## Tech Stack

- **Python 3.12** with Jupyter Notebook
- **pandas / numpy** — data manipulation
- **statsmodels** — OLS regression & diagnostics
- **scikit-learn** — StandardScaler, PolynomialFeatures
- **matplotlib / seaborn** — visualization
- **scipy** — statistical tests (Shapiro-Wilk)

## Study Area

London, UK — selected for its urban complexity, rich historical depth, and abundant open spatial data resources. The final dataset covers diverse environments from central business districts to residential neighbourhoods.

## License

This project is for academic research purposes. Please cite the original dissertation if you use this code or methodology.

## Acknowledgements

- [Place Pulse 2.0](http://pulse.media.mit.edu/) — MIT Media Lab
- [Space Syntax Open Mapping](https://www.spacesyntax.net/)
- [SegFormer](https://huggingface.co/nvidia/segformer-b0-finetuned-ade-512-512) — NVIDIA / Hugging Face
