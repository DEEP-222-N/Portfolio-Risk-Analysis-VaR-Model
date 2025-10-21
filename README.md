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
