# Risk-And-Returns-The-Sharpe-Ratio

-- Use pandas to calculate and compare profitability and risk of different investments using the Sharpe Ratio


# 1. Read in the stock data for Facebook, Amazon and the S&P 500.

-- Importing required modules
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

-- Settings to produce nice plots in a Jupyter notebook
plt.style.use('fivethirtyeight')
%matplotlib inline

-- Reading in the data. A goal here is to read in the data as a time series by moving the Date column to the index. You can load the data using pd.read_csv(). Make sure you use the parse_dates parameter to set the 'Date' column to datetime64, and the index_col parameter to set the same column as index.

stock_data = pd.read_csv("datasets/stock_data.csv", parse_dates = ['Date'], index_col = 'Date')

benchmark_data = pd.read_csv("datasets/benchmark_data.csv", parse_dates = ['Date'], index_col = 'Date')

benchmark_data = benchmark_data.dropna()




# 2. Take a peek at the data you loaded in the last task. 

-- Let's take a look the data to find out how many observations and variables we have at our disposal.


-- Display summary for stock_data

print('Stocks\n')


-- Display a summary of each DataFrame's content using .info()

stock_data.info()


-- Display summary for benchmark_data
print('\nBenchmarks\n')


benchmark_data.info()

-- Show the first few lines of each DataFrame using .head()

stock_data.head()

benchmark_data.head()

# 3. Plot & summarize daily prices for Amazon and Facebook 

-- Before we compare an investment in either Facebook or Amazon with the index of the 500 largest companies in the US, let's visualize the data, so we better understand what we're dealing with.

-- visualize the stock_data - Use the pandas .plot() method on stock_data to show a line plot.

stock_data.plot(subplots = True, title = 'Stock Data')

plt.show()


-- summarize the stock_data - Apply the .describe() method to the stock data to produce summary statistics.

stock_data.describe()

# 4. Visualize & summarize daily values for the S&P 500

-- plot the benchmark_data

benchmark_data.plot(subplots = True, title = 'S&P 500')

plt.show()


-- summarize the benchmark_data

benchmark_data.describe()

# 5. The inputs for the Sharpe Ratio: Starting with Daily Stock Returns

-- The Sharpe Ratio uses the difference in returns between the two investment opportunities under consideration. 
However, our data show the historical value of each investment, not the return. To calculate the return, we need to calculate the percentage change in value from one day to the next. We'll also take a look at the summary statistics because these will become our inputs as we calculate the Sharpe Ratio.

-- The pandas method .pct_change() calculates the relative change from one value to the next in a DataFrame.

-- calculate daily stock_data returns

stock_returns = stock_data.pct_change()

-- plot the daily returns

stock_returns.plot(subplots = True, title = 'Stock Returns')

plt.show()


-- summarize the daily returns

stock_returns.describe()

# 6. Daily S&P 500 returns

-- For the S&P 500, calculating daily returns works just the same way, we just need to make sure we select it as a Series using single brackets [] and not as a DataFrame to facilitate the calculations in the next step.

-- calculate daily benchmark_data returns

sp_returns = benchmark_data['S&P 500'].pct_change()

-- plot the daily returns

sp_returns.plot(title = 'S&P Returns')

plt.show()


-- summarize the daily returns

sp_returns.describe()

# 7. Calculating Excess Returns for Amazon and Facebook vs. S&P 500

-- Next, we need to calculate the relative performance of stocks vs. the S&P 500 benchmark. This is calculated as the difference in returns between stock_returns and sp_returns for each day.
-- Use the .sub() method to subtract the sp_returns from the stock_returns and assign the resulting DataFrame to excess_returns. Make sure to set the parameter axis=0 to align the dates for both time series.

-- calculate the difference in daily returns

excess_returns = stock_returns.sub(sp_returns, axis = 0)

-- plot the excess_returns

excess_returns.plot()


-- summarize the excess_returns

excess_returns.describe()

# 8. The Sharpe Ratio, Step 1: The Average Difference in Daily Returns Stocks vs S&P 500

-- Now we can finally start computing the Sharpe Ratio. First we need to calculate the average of the excess_returns. This tells us how much more or less the investment yields per day compared to the benchmark.
-- Calculate the average of excess_returns using .mean() and assign the result to avg_excess_return
-- Plot the result using the pandas method .plot.bar() and set 'Mean of the Return Difference' as the title.

-- calculate the mean of excess_returns 

avg_excess_return = excess_returns.mean()

-- plot avg_excess_returns

avg_excess_return.plot.bar(title = 'Mean of the Return Difference')

# 9. The Sharpe Ratio, Step 2: Standard Deviation of the Return Difference

-- It looks like there was quite a bit of a difference between average daily returns for Amazon and Facebook. Next, we calculate the standard deviation of the excess_returns. This shows us the amount of risk an investment in the stocks implies as compared to an investment in the S&P 500.

-- calculate the standard deviations. Calculate the standard deviation of excess_returns using .std().

sd_excess_return = excess_returns.std()

-- plot the standard deviations

sd_excess_return.plot.bar(title = 'Standard Deviation of the Return Difference')

# 10. utting it all together

-- Now we just need to compute the ratio of avg_excess_returns and sd_excess_returns. The result is now finally the Sharpe ratio and indicates how much more (or less) return the investment opportunity under consideration yields per unit of risk.

-- The Sharpe Ratio is often annualized by multiplying it by the square root of the number of periods. We have used daily data as input, so we'll use the square root of the number of trading days (5 days, 52 weeks, minus a few holidays): √252

-- calculate the daily sharpe ratio. Apply .div() to divide avg_excess_return by sd_excess_return and assign the result to daily_sharpe_ratio


daily_sharpe_ratio = avg_excess_return.div(sd_excess_return)

-- annualize the sharpe ratio. Calculate the square root of 252 using np.sqrt().

annual_factor = np.sqrt(252)

annual_sharpe_ratio = daily_sharpe_ratio.mul(annual_factor)


-- plot the annualized sharpe ratio

annual_sharpe_ratio.plot.bar(title = 'Annualized Sharpe Ratio: Stocks vs S&P 500')





