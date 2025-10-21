# Portfolio-Risk-Analysis-VaR-Model

| Column            | Description                            | Type               | Notes                                                                |
| ----------------- | -------------------------------------- | ------------------ | -------------------------------------------------------------------- |
| **Date**          | Exact calendar date                    | Date               | Useful for time-series analysis.                                     |
| **GSPC**          | S&P 500 index closing price            | Numeric            | Represents US stock market; used for US portfolio exposure.          |
| **HangSeng**      | Hang Seng index closing price          | Numeric            | Represents Hong Kong stock market; used for foreign market exposure. |
| **USD/HKD**       | USD to HKD exchange rate               | Numeric            | Used to convert Hang Seng values to USD.                             |
| **USD Hang Seng** | Hang Seng index value converted to USD | Numeric            | Calculated as `HangSeng / USD/HKD` (approx).                         |
| **DAX**           | DAX index closing price (Germany)      | Numeric            | Represents European market exposure.                                 |
| **EUR/USD**       | Euro to USD exchange rate              | Numeric            | Used to convert DAX to USD.                                          |
| **USD DAX**       | DAX index value converted to USD       | Numeric            | Calculated as `DAX / EUR/USD`.                                       |
| **Nikkei**        | Nikkei 225 index closing price (Japan) | Numeric            | Represents Japanese market exposure.                                 |
| **YEN/USD**       | JPY to USD exchange rate               | Numeric            | Used to convert Nikkei values to USD.                                |
| **USD Nikkei**    | Nikkei index value converted to USD    | Numeric            | Calculated as `Nikkei / YEN/USD`.                                    |


---

# Portfolio Loss Analysis Workbook

| Column                              | Description                                        | Formula / Notes                                 |
| ----------------------------------- | -------------------------------------------------- | ----------------------------------------------- |
| **Scenario**                        | Scenario number (1, 2, … 500)                      | Each simulated market scenario                  |
| **GSPC / Hang Seng / DAX / Nikkei** | Daily values of portfolio securities               | Taken from market/index data                    |
| **Portfolio**                       | Total portfolio value for that scenario            | Sum of individual securities' values            |
| **Profit / Loss**                   | Gain or loss in USD for that scenario              | `Portfolio - Initial Portfolio Value`           |
| **Bins**                            | Intervals of losses/gains                          | Example: -600, -550, … 500                      |
| **Frequency**                       | Number of scenarios in that bin                    | Count of portfolio outcomes falling in that bin |
| **Rel Freq (Relative Frequency)**   | % of total scenarios in the bin                    | `Frequency / Total Scenarios`                   |
| **Cum Freq (Cumulative Frequency)** | Cumulative % of scenarios up to that bin           | Sum of previous Rel Freq + current Rel Freq     |
| **NLoss**                           | Reference loss value for Normal distribution       | Same as bin center or specific loss point       |
| **F(Nloss)**                        | Cumulative probability under Normal distribution   | `=NORM.DIST(NLoss, Mean, StdDev, TRUE)`         |
| **Normal**                          | Probability of portfolio loss in that bin (Normal) | `Current F(Nloss) - Previous F(Nloss)`          |
| **Tloss**                           | Reference loss value for T-distribution            | Same as bin center or specific loss point       |
| **Std T**                           | Standardized T-value                               | `(Tloss - Mean) / StdDev`                       |
| **F(Tloss) / T**                    | Cumulative probability under T-distribution        | `=T.DIST(Tloss, Degrees of Freedom, TRUE)`      |


This Excel workbook is designed to **analyze the potential losses of a diversified portfolio** consisting of multiple securities under different market scenarios. It calculates scenario-wise portfolio values, profits, and losses to help **quantify risk**.

## Workbook Contents

1. **Portfolio Composition**

   * Lists securities like **GSPC (S&P 500), Hang Seng, DAX, Nikkei**.
   * Shows investment amount (Value in USD) and current price of each security.

2. **Scenario Analysis**

   * Simulates portfolio value under multiple hypothetical market scenarios.
   * Calculates **profit and loss** for each scenario.
   * Updates individual security values using:

     ```
     Scenario Value = Investment * (Next Day / Current Price)
     ```

3. **Loss Distribution**

   * Uses **bins, frequency, relative frequency, cumulative frequency** to show how often certain loss ranges occur.
   * Helps visualize **portfolio risk profile**.

4. **Statistical Metrics**

   * Minimum Loss, Maximum Gain, Mean, Standard Deviation, Skewness, Kurtosis.
   * Provides insights into **extreme losses, volatility, and distribution characteristics** of portfolio returns.


5. **Loss Statistics (NLoss, F(Nloss), Normal, Tloss, Std T, T):**

   * Provides **advanced risk metrics** for portfolio analysis.
   * Includes **normal distribution percentages, T-distribution calculations, and standardized loss values**.
   * Helps assess **extreme loss probabilities** and **tail risk**.

6. **Insightful Interpretation:**

   * Most portfolio scenarios cluster around **small gains/losses (0 to ±50 USD)**.
   * Extreme losses (e.g., -600 to -300 USD) are **rare**, shown by very low relative frequencies.
   * This distribution helps **calculate Value at Risk (VaR)** and **stress test portfolio performance**.

---

## Purpose

* To help investors or risk managers understand **how a portfolio reacts to market fluctuations**.
* To quantify potential **worst-case losses** using historical or scenario-based simulations.
* * To visualize and quantify **portfolio risk exposure**.
* To provide investors and risk managers with **scenario-based probability estimates of potential losses or gains**.
* To support **VaR calculation and risk management decisions**.
---
| Metric        | Value     | Description                                                                           |
| ------------- | --------- | ------------------------------------------------------------------------------------- |
| **Scenarios** | 500       | Total number of simulated portfolio scenarios                                         |
| **Min**       | -159.8253 | Minimum portfolio loss observed (worst-case scenario)                                 |
| **Max**       | 237.6732  | Maximum portfolio gain observed (best-case scenario)                                  |
| **Mean**      | -2.0144   | Average portfolio gain/loss across all scenarios                                      |
| **Std Dev**   | 61.6684   | Standard deviation of portfolio outcomes (risk/volatility measure)                    |
| **Skewness**  | 0.4284    | Measure of asymmetry in loss distribution: >0 → right-skewed, <0 → left-skewed        |
| **Kurtosis**  | 0.5681    | Measure of "tailedness" of distribution: higher → fatter tails, lower → thinner tails |


# Portfolio VAR Analysis Workbook

| Section                     | Column / Field         | Description                                               | Excel Formula / Calculation                                        |
| --------------------------- | ---------------------- | --------------------------------------------------------- | ------------------------------------------------------------------ |
| **Portfolio Composition**   | Security               | Name of the asset or index in the portfolio               | Manual input                                                       |
|                             | Value (USD)            | Amount invested in each security                          | Manual input                                                       |
|                             | Price (USD)            | Current price per unit of the security                    | Manual input / Market data                                         |
|                             | Units                  | Number of units purchased                                 | `=Value / Price`                                                   |
| **Loss Scenarios**          | Scenario               | Simulated market scenario number                          | Generated by Monte Carlo or Historical Simulation                  |
|                             | Loss                   | Portfolio loss for that scenario                          | `Loss = Σ(Units × ΔPrice of each security)`                        |
|                             | Ranked Loss            | Losses sorted from worst to best                          | `=RANK(Loss, LossRange)`                                           |
|                             | T-Loss                 | Standardized loss or probability transformation           | `=T.DIST(Loss, degrees_of_freedom, TRUE)`                          |
| **Loss Bins / Histogram**   | Bin                    | Loss ranges for histogram                                 | Defined manually or with `FLOOR` / `CEILING`                       |
|                             | Frequency              | Number of scenarios in each bin                           | `=COUNTIFS(LossRange, ">=" & BinLower, LossRange, "<" & BinUpper)` |
|                             | Relative Frequency     | Fraction of scenarios in each bin                         | `=Frequency / TotalScenarios`                                      |
| **Descriptive Statistics**  | Min                    | Minimum loss across all scenarios                         | `=MIN(LossRange)`                                                  |
|                             | Max                    | Maximum loss across all scenarios                         | `=MAX(LossRange)`                                                  |
|                             | Mean                   | Average portfolio loss                                    | `=AVERAGE(LossRange)`                                              |
|                             | Std Dev                | Standard deviation of portfolio loss                      | `=STDEV.P(LossRange)`                                              |
|                             | Skewness               | Measure of asymmetry in loss distribution                 | `=SKEW(LossRange)`                                                 |
|                             | Kurtosis               | Measure of tail risk in loss distribution                 | `=KURT(LossRange)`                                                 |
| **Value at Risk (VaR)**     | Confidence Level (B21) | Probability for VaR CI (e.g., 0.95 = 95%)                 | Manual input                                                       |
|                             | VaR (B23)              | Portfolio loss threshold not exceeded at given confidence | `=PERCENTILE.EXC($J$5:$J$504,A23)`                  |
|                             | Std Error (C23)        | Standard error of VaR estimate                            | `=StdDev / SQRT(Number of Scenarios)`                              |
|                             | CI_Low                 | Lower bound of VaR confidence interval                    | `=B23 - NORM.S.INV($B$21) * C23`                                   |
|                             | CI_Up                  | Upper bound of VaR confidence interval                    | `=B23 + NORM.S.INV($B$21) * C23`                                   |
|                             | Explanation            | CI gives the range where “true” VaR likely lies           | Subtract / add margin of error (z-score × Std Error) to VaR        |
| **Expected Shortfall (ES)** | ES                     | Average loss beyond VaR (tail risk)                       | `=AVERAGEIF(LossRange, "<=" & -VaR)`                               |

# Normal VAR Analysis Workbook

| **Formula**                                                 |  **Meaning / Purpose**                                                                                  |
| ----------------------------------------------------------- |------------------------------------------------------------------------------------------------------ | 
| `=B23/G23`                                                  | Calculates **ratio of Value at Risk (VaR) to Expected Shortfall (ES)**                                 |
| `=NORM.INV($A23,$B$15,$B$16)`                               | Gives **Normal VaR** — VaR assuming returns follow a **normal distribution**                           |
| `=$B$15 + NORM.DIST(I23,$B$15,$B$16,FALSE)*$B$16^2/(1-A23)` | Calculates **Normal Expected Shortfall (N ES)** — average loss **beyond Normal VaR**                   | 
| `=I23/J23`                                                  | Ratio of **Normal VaR to Normal ES**, shows relationship between tail loss and average loss beyond VaR |

# T VAR Analysis Workbook

| **Formula**                                        | **Simple Meaning**                                                                                  |
| -------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| `=T.INV(A23,$J$20)`                                | Finds the **t-value** for a given confidence level (like 95%) and degrees of freedom.               |
| `=I23*$B$16*SQRT(ABS(($J$19-2)/$J$19))+$B$15`      | Calculates **t-VaR** — the possible worst loss, adjusted for **fat tails** (more extreme outcomes). |
| `=($J$19-2+I23^2)/($J$19-1)`                       | Finds an **adjustment factor** used later to make ES more accurate.                                 |
| `=$B$15+$B$16*T.DIST(I23,$J$19,FALSE)*K23/(1-A23)` | Calculates **t-ES (Expected Shortfall)** — the **average loss beyond VaR** under t-distribution.    |
| `=J23/L23`                                         | Compares **t-VaR to t-ES**, showing how much the worst loss differs from the average extreme loss.  |

# Weighted VAR Analysis Workbook

> **Note:** Why I Adjust FTSE-100 Using Hang Seng

So here’s the thing — even though FTSE-100 is from the UK and Hang Seng is from Hong Kong, I adjust FTSE-100 based on Hang Seng in my simulations.

The reason is cross-market correlations. Basically, a big move in Hong Kong can affect Europe too, so I scale FTSE-100’s value according to Hang Seng’s price changes.

Important: This isn’t a real market conversion — it’s just a simulation trick.
If I wanted FTSE-100 to be completely independent, I’d just use the base value $C$6 and ignore Hang Seng. But for VaR and Monte Carlo simulations, this makes the portfolio risk more realistic.

| Index    | Country     | Why it’s included                |
| -------- | ----------- | -------------------------------- |
| DJIA     | 🇺🇸 USA    | Exposure to U.S. market          |
| FTSE-100 | 🇬🇧 UK     | Exposure to European (UK) market |
| CAC-40   | 🇫🇷 France | Exposure to Eurozone             |
| Nikkei   | 🇯🇵 Japan  | Exposure to Asian market         |

| Column                                   | Example Formula                             | Purpose / Logic                                                                         |
| ---------------------------------------- | ------------------------------------------- | --------------------------------------------------------------------------------------- |
| **A:** Date                              | Direct from “Data” sheet                    | Date / observation number.                                                              |
| **B–M:** Asset Returns                   | `=Data!C3/Data!C2 - 1` (patterned)          | Individual asset returns for each day.                                                  |
| **N:** Portfolio Return                  | `=MMULT(J5:M5,$D$5:$D$8)`                   | Weighted portfolio return (dot product of asset returns × weights).                     |
| **O:** Mean-Adjusted Return              | `=N5 - $B$9`                                | Demeaned portfolio return (subtracting average).                                        |
| **P:** Portfolio Loss                    | `=-O5`                                      | Loss = - (return). Converts positive return to loss value.                              |
| **Q:** Exponential Weight                | `=$Q$2^(500-I5)*(1-$Q$2)/(1-$Q$2^500)`      | Weight for each observation using decay factor λ (`$Q$2`). More recent = higher weight. |
| **R–S:** Weighted Return / Weighted Loss | `=P5*Q5`                                    | Multiply each loss by its weight → gives weighted loss.                                 |
| **T:** Cumulative Weight                 | `=SUM($Q$5:Q5)`                             | Builds up total probability mass (for percentile extraction).                           |


# Parm VaR Eq Weights Analysis Workbook

In a stress testing or portfolio risk context, the correlation matrix you showed is not calculated by Excel automatically — it’s a user-defined assumption used as a model input.

You’re essentially specifying how strongly different indices (DJIA, FTSE 100, CAC40, Nikkei 225) move together under the stress scenario.

So:

“1” on the diagonal = perfect correlation with itself.

“0.8” = a judgmental estimate of how correlated DJIA is with FTSE 100 (or CAC40) during stress.

These values don’t come from Excel formulas — they’re chosen by analysts based on historical data, expert judgment, or stress scenario assumptions.

🔹 2. Why 0.8 specifically?

In practice:

Under normal market conditions, DJIA and FTSE 100 might have a correlation around 0.7–0.9 (since both are large developed-market indices).

Under stress, correlations often increase — markets move more together when risk aversion spikes.

Analysts might round or adjust to a simple number (like 0.8) for modeling convenience.

So, 0.8 is manually chosen to reflect that under stress, U.S. and U.K./European markets are highly but not perfectly correlated.


| Column                                   | Example Formula                                             | Purpose / Logic                                                     |
| ---------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------- |
| **A–F:** Asset Returns                   | `=(Data!C3-Data!C2)/Data!C2`                                | Raw daily return per asset (equal weights assumed).                 |
| **G–L:** Covariance & Correlation Matrix | `=CORREL(B$3:B$502, C$3:C$502)` or `=COVARIANCE.P(...)`     | Calculates pairwise correlation/covariance between assets.          |
| **M:** Asset Variances                   | `=VAR.S(B3:B502)`                                           | Individual asset variances (used to build Σ matrix).                |
| **N:** Confidence Level                  | Manually set (e.g., 0.99 or 0.95)                           | Used for z-score.                                                   |
| **O:** Z-score                           | `=_xlfn.NORM.S.INV(N3)`                                     | Inverse normal quantile for given confidence level.                 |
| **P:** Parm VaR                          | `=$H$22*O5`                                                 | Estimates maximum expected portfolio loss at confidence level.      |
| **Q:** ES                                | `=$H$22*NORM.S.DIST(O3,FALSE)/(1-N3)`                       | Calculates average loss beyond VaR during extreme events..          |


# Parm VaR  EWMA Analysis Workbook


| Column                         | Example Formula                                       | Purpose / Logic                                                   |
| ------------------------------ | ----------------------------------------------------- | ----------------------------------------------------------------- |
| **A–E:** Historical Returns    | `=(Data!C3-Data!C2)/Data!C2`                          | Base returns for assets.                                          |
| **F:** Lambda (λ)              | `=0.94`                                               | Decay factor (higher = smoother volatility).                      |
|  EWMA Variances                | `=G5*$F$3 + A5^2*(1-$F$3)`                            | Recursive formula: new σ² = λ * old σ² + (1-λ) * r².              |
| EWMA Covariances               | `=K5*$F$3 + A5*B5*(1-$F$3)`                           | Covariance update between assets (same EWMA rule).                |
| **O:** EWMA Portfolio Variance | `=MMULT(TRANSPOSE(Weights),MMULT(CovMatrix,Weights))` | Portfolio σ² using latest EWMA covariance matrix.                 |


# $ VaR Vol Adjust Analysis Workbook

| **Loss Bins / Histogram**   | Bin                    | Loss ranges for histogram                                 | Defined manually or with `FLOOR` / `CEILING`                       |
|                             | Frequency              | Number of scenarios in each bin                           | `=COUNTIFS(LossRange, ">=" & BinLower, LossRange, "<" & BinUpper)` |
|                             | Relative Frequency     | Fraction of scenarios in each bin                         | `=Frequency / TotalScenarios`                                      |
| **Descriptive Statistics**  | Min                    | Minimum loss across all scenarios                         | `=MIN(LossRange)`                                                  |
|                             | Max                    | Maximum loss across all scenarios                         | `=MAX(LossRange)`                                                  |
|                             | Mean                   | Average portfolio loss                                    | `=AVERAGE(LossRange)`                                              |
|                             | Std Dev                | Standard deviation of portfolio loss                      | `=STDEV.P(LossRange)`                                              |
|                             | Skewness               | Measure of asymmetry in loss distribution                 | `=SKEW(LossRange)`                                                 |
|                             | Kurtosis               | Measure of tail risk in loss distribution                 | `=KURT(LossRange)`                                                 |
| **Value at Risk (VaR)**     | Confidence Level (B21) | Probability for VaR CI (e.g., 0.95 = 95%)                 | Manual input                                                       |
|                             | VaR (B23)              | Portfolio loss threshold not exceeded at given confidence | `=PERCENTILE.EXC($J$5:$J$504,A23)`                  |
|                             | Std Error (C23)        | Standard error of VaR estimate                            | `=StdDev / SQRT(Number of Scenarios)`                              |
|                             | CI_Low                 | Lower bound of VaR confidence interval                    | `=B23 - NORM.S.INV($B$21) * C23`                                   |
|                             | CI_Up                  | Upper bound of VaR confidence interval                    | `=B23 + NORM.S.INV($B$21) * C23`                                   |
|                             | Explanation            | CI gives the range where “true” VaR likely lies           | Subtract / add margin of error (z-score × Std Error) to VaR        |
| **Expected Shortfall (ES)** | ES                     | Average loss beyond VaR (tail risk)                       | `=AVERAGEIF(LossRange, "<=" & -VaR)`                               |
