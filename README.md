# Multi-Factor Equity Risk & Performance Attribution Model

> **Author:** Quantitative Research Portfolio Project  
> **Domain:** Quantitative Finance, Applied Econometrics, Risk Management  
> **Stack:** Python, Pandas, NumPy, StatsModels, Matplotlib, Seaborn  

---

## 1. Executive Summary

This project implements an empirical **Fama-French Multi-Factor Risk Decomposition Model** to evaluate performance attribution, risk exposures, and idiosyncratic alpha across a dynamic equities portfolio (`AAPL`, `MSFT`, `JNJ`, `JPM`, `AMZN`). 

By decomposing excess returns over 1,260 trading sessions into systematic market risk, size premium ($\text{SMB}$), and value premium ($\text{HML}$), this framework isolates true risk-adjusted manager skill ($\alpha$) from broader factor exposures ($\beta$).

### Key Findings
* **Alpha Isolation:** Microsoft (`MSFT`) demonstrated the highest annualized Jensen's Alpha (**18.80%**), indicating substantial equity-specific returns independent of systematic factor movements.
* **Factor Sensitivity:** All analyzed assets maintained defensive-to-moderate market betas ($\beta_{\text{MKT}} \in [0.428, 0.521]$) and positive sensitivity to small-cap factor shocks ($\beta_{\text{SMB}} \in [0.186, 0.258]$).
* **Explanatory Power:** Multi-factor regression models yielded $R^2$ values ranging from **13.16% to 18.57%**, proving that over 80% of daily return variance in this asset universe is driven by idiosyncratic, firm-specific shocks rather than broad macroeconomic factor exposures.

---

## 2. Theoretical Framework & Econometric Specification

In traditional Capital Asset Pricing Model (CAPM) theory, asset returns are assumed to be driven solely by a single market factor. The multi-factor extension accounts for persistent anomaly premiums in historical market data.

The econometric specification for asset $i$ at time $t$ is defined by the following Ordinary Least Squares (OLS) multivariate regression:

$$R_{i,t} - R_{f,t} = \alpha_i + \beta_{i,\text{MKT}}(R_{m,t} - R_{f,t}) + \beta_{i,\text{SMB}}\text{SMB}_t + \beta_{i,\text{HML}}\text{HML}_t + \varepsilon_{i,t}$$

Where:
* **$R_{i,t} - R_{f,t}$**: Daily excess return of asset $i$ over the daily risk-free benchmark ($R_f \approx 4.0\%$ annualized).
* **$\alpha_i$ (Jensen's Alpha)**: Unexplained daily intercept. Annualized via $252 \times \alpha_i$ to quantify annual risk-adjusted outperformance.
* **$\beta_{i,\text{MKT}}$**: Sensitivity to broad market excess return ($R_m - R_f$).
* **$\beta_{i,\text{SMB}}$ (Small Minus Big)**: Sensitivity to the size risk factor (Small-Cap ETF return minus Broad Market ETF return).
* **$\beta_{i,\text{HML}}$ (High Minus Low)**: Sensitivity to the value style factor (Value ETF return minus Growth ETF return).
* **$\varepsilon_{i,t}$**: Zero-mean Gaussian disturbance term representing idiosyncratic residual risk.

---

## 3. Empirical Results & Performance Attribution

The multivariate regression model was fitted using 1,260 daily observations per asset. Below is the full risk factor exposure matrix and explanatory summary:

| Ticker | Annualized Alpha ($\alpha$) | Market Beta ($\beta_{\text{MKT}}$) | Size Beta ($\beta_{\text{SMB}}$) | Value Beta ($\beta_{\text{HML}}$) | Model $R^2$ |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **AAPL** | `+2.06%` | `0.4520` | `0.2188` | `0.0008` | `0.1384` |
| **MSFT** | **`+18.80%`** | `0.4988` | `0.2575` | `0.0547` | `0.1740` |
| **JNJ**  | `+0.18%` | **`0.5206`** | **`0.2465`** | `-0.0016` | **`0.1857`** |
| **JPM**  | `+13.95%` | `0.4284` | `0.1864` | `-0.0180` | `0.1316` |
| **AMZN** | `+13.72%` | `0.4958` | `0.2171` | `0.0057` | `0.1627` |

---

## 4. Quantitative Analysis & Portfolio Insights

> ### 1. Deconstructing Jensen's Alpha ($\alpha$)
> Assets like `MSFT` (+18.80%), `JPM` (+13.95%), and `AMZN` (+13.72%) exhibited high annualized alpha values. In an institutional context, this indicates that holding these individual assets rewarded investors with return premiums far above what would be predicted purely by their systematic exposure to market, size, and value factors.

> ### 2. Value vs. Growth Neutrality ($\beta_{\text{HML}}$)
> All observed assets yielded near-zero $\beta_{\text{HML}}$ values (ranging from $-0.0180$ to $+0.0547$). This confirms that during the evaluation period, portfolio return dynamics were largely uncorrelated with broader value-versus-growth style rotation trades in the market.

> ### 3. Residual Risk & Hedging Implications ($1 - R^2$)
> Because the multi-factor model explains less than $20\%$ of return variance across all assets ($R^2 < 0.20$), factor hedging alone (e.g., shorting factor ETFs) will not fully neutralize portfolio volatility. Risk managers must utilize asset-specific options overlay strategies or long-short pairs trading to mitigate remaining idiosyncratic variance ($80\%+$).

---

## 5. Repository Structure

```text
├── data/
│   └── simulated_factor_returns.csv   # Pre-processed daily log-returns data
├── notebooks/
│   └── factor_attribution_model.ipynb # Interactive Jupyter/JupyterLite notebook
├── src/
│   └── factor_model.py                # Standalone production Python script
├── assets/
│   └── attribution_dashboard.png      # High-resolution output heatmaps & charts
├── README.md                          # Institutional research overview
└── requirements.txt                   # Dependency environment specs
