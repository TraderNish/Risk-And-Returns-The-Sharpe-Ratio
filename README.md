# Risk-And-Returns-The-Sharpe-Ratio
#Use pandas to calculate and compare profitability and risk of different investments using the Sharpe Ratio


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








