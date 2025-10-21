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


Ahh, got it! 😎 Ye aapka **Portfolio Loss Analysis workbook ka data** hai, not the full project itself.

Chalo mai **clear explanation likh deta hun jo aap repo me use kar sako**, jisse koi bhi samajh sake ki ye **workbook kya karta hai**:

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

