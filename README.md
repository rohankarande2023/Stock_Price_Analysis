# Stock_Price_Analysis
Equity Portfolio Management
Equity Portfolio Management

### Data Preparation

Download the historial daily data of the entire 2018 for the 10 stocks 

```python
universe = ['IBM', 'MSFT', 'GOOG', 'AAPL', 'AMZN', 'META', 'NFLX', 'TSLA', 'ORCL', 'SAP']
```


You should have 10 csv files on your disk now. IBM.csv, MSFT.csv, etc. We call the 10 stocks "universe" which is the entire stock market you can trade.



### Retrieve the "Close" and "Adj Close" values for each stock

You will create a dataframe where there are 20 columns for the 10 stocks, each row is the "Close" and "Adj Close" prices for the 10 stocks on each day, in the order of the business days in 2018. Assume all buy/sell on the "Close" prices and there is no transaction cost.

### You start to manage 5 million dollars fund on Jan 02, 2018

You have a strategy to manage the fund.

1. On Jan 02 2018, you split the $5M into 5 $1M, and use them to buy 5 stocks from the 10 stocks. For example, IBM close price was $154.25 With $1M, you can buy max 6482 shares with cost $999848.5 with $151.5 cash left. You decided to spend $\$1m$ on each of `['IBM', 'MSFT', 'GOOG', 'AAPL', 'AMZN']` respectively and keep the rest cash into a zero-interest cash account. On Jan 02 2018, your mark to market value (MTM) is $5M if combining all stocks value and cash. Your holdings of stocks and cach account is your portfolio.


MTM =  cash + sum{k=1 to 5} Shares * ClosePrice



2. Your trading strategy is "5 days rebalancing of buying low". Here is how it works. You keep your portfolio unchanged until 5 days later on Jan 09 2018. Now you want to re-check the market and adjust your portfolio. You will compute the "Adj Close" price changes from Jan 02 to Jan 09, and find the 5 stocks whose "Adj Close" prices dropped the most in terms of percentage. You sell all current holdings on Jan 09 "Close" prices to convert your portfolio to all cash. Then immediately split your cash, including your cash account, to 5 equal parts to buy the 5 stocks that dropped the most from Jan 02 to Jan 09 on 'Adj Close' prices. You always buy the max shares of stock on the "Close" price and keep the rest cash in cash account. Now the portfolio should be different from 5 days ago. This operation is called "rebalancing".

    Keep in mind, the MTM will change every day, even when your portfolio holdings don't change, because the stock prices change.


3. Corporations generally issue stock dividends on some days. The total dividend you get on such a day is the stock dividend  times your shares if you have shares of this stock on the dividend day. If you buy shares on the dividend day, these bought shares are not qualified to get dividend. If you sell shares on the dividend day, the sold shares are qualified to get dividend.

4. 5 business days later on Jan 17 (Jan 15 was a holiday), you re-check the market and adjust your portfolio again. You will have a new portfolio on Jan 17.


5. If you run this strategy every 5 days all the way to Dec 31 2018, you will have a daily MTM. You expect the MTM on Dec 31 2018 should be higher than \$5m because you always buy the stocks that dropped the most, i.e., you always buy low.


6. Another strategy is "5 days rebalancing of buying high". You always buy the 5 stocks whose "Adj Close" prices surge the most in terms of percentage because you believe the trend will continue. Run the new strategy and see how the MTM will change.


7. You will create a "high tech index" which is simply the daily average of the 10 stocks "Close" prices. Compare your MTM series with the "high tech index" and plot their curves. To plot the two curves together, you may want to convert the series to daily percentage change with regard to Jan 02 2018.


8. Download the USD/JPY 2018 historical data at https://finance.yahoo.com/quote/JPY%3DX/history?period1=1514764800&period2=1546300800&interval=1d&filter=history&frequency=1d&includeAdjustedClose=true then use the "Close" column as the rate to convert your MTM series from USD to JPY. Plot the two MTM curves. You will need to convert to daily percentage change too.


9. The above two strategies both rebalance every 5 days. Try to change the days interval and find the optimal days interval that maximizes the MTM on 12/31/2018.


# List of functions and variable names

## Functions
- strategy_top_5
    - **input:** day_number
    - **returns** stocks_to_buy[{stock_name : closing_value}]
- buy_stocks
    - **input:** total_cash, stocks_to_buy  
    - **returns** no_of_stocks[{stock name: no_of_stocks_bought}] and remaning_cash i.e remaining cash
- div 
    - **input:** day_number, no_of_stocks
    - **returns** div_cash
- mtm
    - **input:** total_cash, remaning_cash, no_of_stocks
    - **returns** mtm_value[] i.e a list everyday MTM values
- totalCash_afterSell
    - **input:** no_of_stocks[{}], day_number, remaning_cash 
    - **returns** total_cash
- select_strategy
    - **input:** strategy (dtype- int)
    - **returns** stocks_to_buy[{stock_name : closing_value}]
- stock_analysis --> main function
    - **input:** strategy,period
    - **returns** None
- stock_analysis_without_plot --> main function
    - **input:** period,strategy,finalMTM
    - **returns** None
- plot_mtm_vs_high_tech_index --> plot MTM vs High tech index
    - **input:** None
    - **returns** None
- plot_mtm_jpy --> Function to plot MTM vs jpyMTM values
    - **input:** None
    - **returns** None
- optimal --> function to find out the optimal period for each strategy
    - **input:** None
    - **returns** None
- plot_mtm_jpy --> Function to plot MTM vs jpyMTM values
    - **input:** None
    - **returns** None

## Variables
- all_stocks --> **df** of all 10 stocks
- total_cash --> **float** containig total cash
- stocks_to_buy --> **dict** stocks_to_buy[{stock_name : closing_value}]
- day_number --> **int** ranges from 0-250
- no_of_stocks --> **dict** no_of_stocks[{stock name: no_of_stocks_bought}]
- div_cash --> **float** is the value of total dividend
- remaning_cash --> **float** is the value of remaining cash after buying
- mtm_value --> **list** contains daily mtm values
- period --> **int** is the no of days you keep the stocks
- days_batch --> **list** contains list of days when stocks are sold as per the selected strategy
