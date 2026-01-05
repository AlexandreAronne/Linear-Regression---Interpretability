# Linear Regression for Asset Pricing - Interpretability

## Overview

This repository provides a practical, hands-on introduction to **linear regression in empirical asset pricing**, demonstrating how classic factor models (CAPM, Fama-French 3-Factor, and 5-Factor models) are applied to analyze stock returns. The project uses real financial data from both US and Brazilian markets to estimate factor exposures, evaluate model fit, and interpret the results through comprehensive visualizations.

## Purpose

The goal is to **teach and demonstrate** how linear regression is used in finance to:
- Understand the relationship between stock returns and systematic risk factors
- Quantify a stock's sensitivity (beta) to various market factors
- Measure abnormal returns (alpha) after accounting for systematic risk
- Compare model performance across different factor specifications

This is an **educational resource** suitable for students, researchers, and practitioners interested in empirical asset pricing and quantitative finance.

## Repository Contents

```
.
├── Interpretabilidade_linear_regression_asset_pricing_Github_Final.ipynb
│   └── Main Jupyter notebook with complete analysis workflow
├── F-F_Research_Data_5_Factors_2x3_daily.csv
│   └── Fama-French 5 Factors daily data (US market)
└── nefin_factors.csv
    └── NEFIN factors daily data (Brazilian market)
```

### Data Files

1. **F-F_Research_Data_5_Factors_2x3_daily.csv** (~15,600 records)
   - Source: Kenneth French Data Library
   - Contains daily returns for US market factors (1963-present)
   - Factors: Mkt-RF, SMB, HML, RMW, CMA, RF
   - Values in **percent** (converted to decimals in analysis)

2. **nefin_factors.csv** (~6,100 records)
   - Source: NEFIN (Brazilian Center for Research in Financial Economics)
   - Contains daily returns for Brazilian market factors (2001-2025)
   - Factors: Rm_minus_Rf, SMB, HML, WML, IML, Risk_Free
   - Values already in **decimal** format
   - Limited to data through May 29, 2025

## Methodology

### Asset Pricing Models

The notebook implements three classic factor models:

#### 1. **CAPM (Capital Asset Pricing Model)**
```
R_i,t - R_f,t = α_i + β_i,MKT(R_m,t - R_f,t) + ε_i,t
```
- **Single factor**: Market excess return (MKT-RF)
- Tests whether a stock's return is fully explained by market movements

#### 2. **Fama-French 3-Factor Model (FF3)**
```
R_i,t - R_f,t = α_i + β_i,MKT(R_m,t - R_f,t) + β_i,SMB·SMB_t + β_i,HML·HML_t + ε_i,t
```
- **Three factors**:
  - MKT-RF: Market excess return
  - SMB: Small Minus Big (size premium)
  - HML: High Minus Low (value premium)

#### 3. **5-Factor Models**

**US Market (Fama-French 5-Factor):**
```
R_i,t - R_f,t = α_i + β_MKT·MKT_t + β_SMB·SMB_t + β_HML·HML_t + β_RMW·RMW_t + β_CMA·CMA_t + ε_i,t
```
- Adds two factors:
  - RMW: Robust Minus Weak (profitability premium)
  - CMA: Conservative Minus Aggressive (investment premium)

**Brazilian Market (NEFIN 5-Factor):**
```
R_i,t - R_f,t = α_i + β_MKT·MKT_t + β_SMB·SMB_t + β_HML·HML_t + β_WML·WML_t + β_IML·IML_t + ε_i,t
```
- Uses Brazil-specific factors:
  - WML: Winners Minus Losers (momentum)
  - IML: Illiquid Minus Liquid (illiquidity premium)

### Data Processing Workflow

1. **Load Factor Data (Daily → Weekly)**
   - Load daily factor returns from CSV files
   - Convert Fama-French data from percent to decimal format
   - Compound daily returns to weekly using: `(1+r)_week = ∏(1+r_d) - 1`
   - Resample to weekly frequency (Friday close)

2. **Download Stock Prices**
   - Fetch daily stock prices from Yahoo Finance
   - Support for both US tickers and Brazilian stocks (.SA suffix)
   - Handle multiple ticker candidates for Brazilian market

3. **Calculate Weekly Returns**
   - Compound daily prices to weekly returns
   - Align with factor data frequency
   - Focus on last 100 weeks of data

4. **Run Regressions**
   - Calculate excess returns: `R_i,t - R_f,t`
   - Estimate OLS regressions with HAC standard errors (Newey-West)
   - Extract coefficients, t-statistics, R², adjusted R²

5. **Generate Visualizations**
   - Factor price indices (base = 100)
   - Stock price and return time series
   - Factor exposure bar charts
   - Actual vs. fitted return comparisons (CAPM only)

## Key Features

### Robust Statistical Methods
- **HAC Standard Errors**: Newey-West estimator with 4 lags to handle autocorrelation and heteroskedasticity in weekly returns
- **Proper Return Compounding**: Geometric compounding for daily-to-weekly conversion
- **Excess Return Framework**: All regressions use stock returns minus risk-free rate

### Comprehensive Output
For each stock and model combination:
- **Coefficient Estimates**: α (alpha), β (betas) with interpretation
- **Statistical Significance**: t-statistics and p-values
- **Model Fit Metrics**: R² and adjusted R²
- **Visual Analysis**: Charts showing factor exposures and model fit

### Educational Design
- Clear documentation explaining each step
- Mathematical notation for all models
- Interpretation guidelines for results
- Learning objectives explicitly stated

## Learning Objectives

After working through this notebook, you will:

1. **Understand linear regression in asset pricing context**
   - How factor models are specified as regression equations
   - The meaning of alpha (abnormal return) and beta (factor loading)
   - Why excess returns are used as the dependent variable

2. **Master practical data handling**
   - Converting between return frequencies (daily to weekly)
   - Proper compounding of returns
   - Aligning different data sources temporally

3. **Interpret regression outputs**
   - Statistical significance of coefficients
   - Economic interpretation of factor loadings
   - Model comparison using R² and adjusted R²

4. **Apply multiple factor models**
   - Compare simple (CAPM) vs. multi-factor models
   - Understand factor definitions (size, value, profitability, etc.)
   - Recognize market-specific factors (US vs. Brazil)

## Technical Requirements

### Python Dependencies
- `numpy`: Numerical computations
- `pandas`: Data manipulation and time series handling
- `scikit-learn`: Machine learning utilities
- `statsmodels`: Statistical models and HAC standard errors
- `matplotlib`: Plotting and visualization
- `seaborn`: Enhanced statistical visualizations
- `yfinance`: Yahoo Finance data download (implied from usage)

### Installation
```bash
pip install numpy pandas scikit-learn statsmodels matplotlib seaborn yfinance
```

## Usage

1. **Open the Jupyter Notebook**
   ```bash
   jupyter notebook Interpretabilidade_linear_regression_asset_pricing_Github_Final.ipynb
   ```

2. **Run All Cells**
   - The notebook is self-contained and runs top-to-bottom
   - First cells install dependencies and load data
   - Middle cells define helper functions
   - Final cells execute analysis and generate results

3. **Customize Analysis**
   - Modify stock tickers in the analysis cells
   - Adjust the time window (default: last 100 weeks)
   - Change the resampling frequency (default: weekly on Fridays)

## Interpretation Guide

### Alpha (α)
- **α ≈ 0 and insignificant**: Factors fully explain the stock's average return (expected result)
- **α > 0 and significant**: Positive abnormal return (outperformance)
- **α < 0 and significant**: Negative abnormal return (underperformance)

### Betas (β)
- **Market Beta (β_MKT)**: Sensitivity to overall market movements
  - β > 1: More volatile than market (aggressive)
  - β < 1: Less volatile than market (defensive)
- **SMB Beta (β_SMB)**: Size exposure
  - β > 0: Small-cap tilt
  - β < 0: Large-cap tilt
- **HML Beta (β_HML)**: Value exposure
  - β > 0: Value stock characteristics
  - β < 0: Growth stock characteristics
- **RMW Beta (β_RMW)**: Profitability exposure (US only)
- **CMA Beta (β_CMA)**: Investment exposure (US only)
- **WML Beta (β_WML)**: Momentum exposure (Brazil only)
- **IML Beta (β_IML)**: Illiquidity exposure (Brazil only)

### Model Fit
- **R²**: Proportion of return variance explained by factors
  - Higher R² = better factor model fit
  - Typically increases with more factors
- **Adjusted R²**: R² penalized for number of factors
  - Better for comparing models with different numbers of factors
  - Only increases if new factor genuinely improves fit

## Data Sources

### Fama-French Factors
- **Source**: Kenneth R. French - Data Library
- **Website**: https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html
- **Coverage**: US market, daily frequency, 1963-present
- **Construction**: Based on 2x3 sorts on size and book-to-market (and other characteristics)

### NEFIN Factors  
- **Source**: NEFIN - Núcleo de Pesquisas em Economia Financeira (USP)
- **Coverage**: Brazilian market, daily frequency, 2001-2025
- **Note**: Data limited to May 29, 2025 in this dataset

## Educational Context

This repository is designed for:
- **Finance Courses**: Empirical methods in asset pricing, portfolio management
- **Statistics/Econometrics**: Applied regression analysis with financial data
- **Quantitative Finance**: Factor modeling and risk attribution
- **Self-Study**: Practitioners learning asset pricing empirically

## Key Insights and Discussion Points

1. **Alpha Interpretation**: Near-zero and insignificant alpha suggests the factors successfully capture systematic return patterns

2. **Factor Relevance**: Compare R² across models to assess whether additional factors provide explanatory power

3. **Market Differences**: US and Brazilian markets use different factor sets, reflecting distinct market structures and anomalies

4. **Statistical vs. Economic Significance**: A coefficient can be statistically significant but economically small (or vice versa)

## Visualization Strategy

- **Factor Price Indices**: Display before stock analysis to show factor performance (cumulative returns starting at 100)
- **Per-Stock Charts**: Show price and return time series immediately after stock name
- **CAPM Analysis**: Focus on actual vs. fitted comparison (1 plot)
- **Multi-Factor Models**: Show factor exposure bar charts (no actual vs. fitted to avoid clutter)

## License and Attribution

This educational project uses publicly available financial data:
- Fama-French factors: © Kenneth R. French
- NEFIN factors: © Núcleo de Pesquisas em Economia Financeira, USP

Please cite appropriately if using this code or methodology in research or publications.

## Author

**Alexandre Aronne**
- Repository: https://github.com/AlexandreAronne/Linear-Regression---Interpretability

## Contributing

This is an educational repository. Feedback, suggestions for improvement, or additional examples are welcome through GitHub issues or pull requests.

## Further Reading

### Academic Papers
- Fama, E. F., & French, K. R. (1993). Common risk factors in the returns on stocks and bonds. *Journal of Financial Economics*, 33(1), 3-56.
- Fama, E. F., & French, K. R. (2015). A five-factor asset pricing model. *Journal of Financial Economics*, 116(1), 1-22.
- Carhart, M. M. (1997). On persistence in mutual fund performance. *Journal of Finance*, 52(1), 57-82.

### Resources
- Kenneth French Data Library: https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html
- NEFIN: http://nefin.com.br/
- Newey-West HAC Standard Errors: statsmodels documentation
