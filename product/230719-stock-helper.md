---
description: A helper for stock investors.
---

# Stock Helper

## Functional Requirements

### Stock Selection

1. Fama-French based on some Indexes or Fund

### Portfolio Construction

1. MPT
2. Kelly's Formula

### Risk Management

### Hedging

## Non-functional Requirements

## Stories

### Epic 1: Fama-French 3-Factor Model on a Basket of Stocks

[Fama-French三因子模型最深剖析](https://www.bilibili.com/video/BV11Y411x7jy/?spm_id_from=333.999.0.0&vd_source=3caf1cf2eb94db028e6f5b496be93bc8)

#### E1S1: Create a Basket of Selected Stocks

1. Input: tickers

#### E1S2: Prepare the Market Data

1. OHLCV of the tickers
2. Rf
3. Input: duration to look back

#### E1S3: Calculate the SMB of the Basket

#### E1S4: Calculate the SMB of each Stock

Here are the steps to calculate the SMB factor for a single stock:

1. Collect historical price data for the stock you are interested in, as well as for a small-cap index and a large-cap index. The small-cap index should consist of stocks of companies with relatively small market capitalizations, while the large-cap index should consist of stocks of companies with relatively large market capitalizations.

2. Calculate the returns for each of the three indices over a given time period. The returns can be calculated as the percentage change in the index value over the time period.

3. Run a regression analysis using the returns for the small-cap index and the large-cap index as independent variables, and the returns for the individual stock as the dependent variable. The resulting regression equation will provide the coefficients for the small-cap and large-cap indices.

4. Calculate the SMB factor as the difference between the return of the small-cap index and the return of the large-cap index, weighted by the coefficients from the regression analysis. The SMB factor represents the additional return that investors can expect to receive for investing in small-cap stocks relative to large-cap stocks.

Note that calculating the SMB factor for a single stock is not a commonly used approach, as the SMB factor is typically applied to portfolios of stocks rather than individual stocks. However, the same approach can be used to calculate the SMB factor for a portfolio of stocks by replacing the individual stock returns with the portfolio returns in the regression analysis.

#### E1S5: Calculate the HML of the Basket

#### E1S6: Calculate the HML of each Stock

#### E1S7: Calculate the Return of the Basket

#### E1S8: Sort the Stocks based on Alpha

#### E1S9: GRS test

