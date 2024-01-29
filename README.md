<h1>Domino's Stock Price Analysis</h1>
<p>This project conducts analysis on a dataset containing Domino's Stock price data using the Python programming language and the popular library Pandas. The analysis explores various aspects of the stock, such as trading volume, stock price trends, returns, and potential patterns or clusters in stock price movements over time.</p>

<h2>Installation</h2>
<p>Due to the project being completed using Jupyter Notebook, there is no need to install my project on your machine. If you want to explore the code and its outputs more in-depth click on Domino's Stock Price Analysis.ipynb file in the repository and there you can find code blocks with their outputs.</p>

<h2>Documentation</h2>

<h3>Library</h3>
<p>To start, all necessary libraries must be imported. For this project, the only libraries needed are pandas, scripy, sklearn.cluster, and matplotlib.pyplot. These libraries will allow me to perform analysis, create visualizations, and perform Kmeans clustering on the dataset.</p>

```
import pandas as pd
from scipy import stats
from sklearn.cluster import KMeans
import matplotlib.pyplot as plt
```

<h3>Getting Started</h3>
<p>Next step is to thoroughly look through the data set looking for the following:</p>
<ul>
  <li>Names of the columns and rows</li>
  <li>Any noticeable missing data</li>
  <li>Types of data values </li>
</ul>

<p>The code below saves the dominos stock price data to the variable dominos and finds its shape and column names. The data set has 505 rows and 7 columns: Date, Open, High, Low, Close, Adj Close, and  Volume.</p>

```
dominos = pd.read_csv('Dominos_Stock_Data.csv')
dominos.shape
dominos.columns
```

<p>Now, its time to determine the data types for each column: Date is object, Open/High/Low/Close/Adj Close are float64, and Volume in int64. Date will be changed to a different data type later. </p>

```
dominos.dtypes
```

<p>There are no null values.</p>

```
dominos.isnull().sum()
```

<p>There are no duplicate rows.</p>

```
dominos.duplicated().sum()
```

<h3>Analysis</h3>

```
dominos.describe()
```
<p>Found the summary statistics for each column.</p>

```
	Open	         High	         Low        Close	     Adj Close	       	Volume
count	505.000000	505.000000	505.000000	505.000000	505.000000	5.050000e+02
mean	385.391723	390.291347	380.870416	385.614693	382.575584	6.996162e+05
std	67.485552	    67.673877	 67.327659	   67.378659	  68.525904	  5.993578e+05
min	254.890000	257.610000	253.080000	255.700000	251.350000	1.843000e+05
25%	353.730000	360.580000	347.730000	354.620000	350.690000	4.495000e+05
50%	384.600000	387.580000	380.910000	383.920000	380.590000	5.776000e+05
75%	418.090000	423.110000	415.740000	419.700000	415.290000	7.863000e+05
max	541.990000	548.720000	536.110000	540.470000	539.480000	1.027880e+07
```

```
dominos['Date'] = pd.to_datetime(dominos['Date'])
dominos['Date'].min()
dominos['Date'].max()
```

<p>Converted the date column to the date time data type, found the dataset start date of 2019-10-16, and the dataset end date of 2021-10-15.</p>

```
dominos.corr()
```

<p>Found the correlation of each variable concerning each other. The volume appears to be the only variable that has a slightly negative correlation close to 0 in relation to the other variables. Open, High, Low, Close, and Adj Close all share a correlation close positive 1. </p>

```
	      Open	     High	   Low	      Close	  Adj Close	    Volume
Open	1.000000	0.996773	0.997980	0.995542	0.995534	-0.175188
High	0.996773	1.000000	0.995794	0.997934	0.997772	-0.147154
Low	0.997980	0.995794	1.000000	0.997341	0.997395	-0.187099
Close	0.995542	0.997934	0.997341	1.000000	0.999897	-0.163411
Adj Close 0.995534	0.997772	0.997395	0.999897	1.000000	-0.165840
Volume	-0.175188	-0.147154	-0.187099	-0.163411	-0.165840	1.000000
```

```
z_scores = stats.zscore(dominos['Close']) 
dominos[(z_scores > 3) | (z_scores < -3)]
```

<p>Attempted to find any existing outliers based on having a z score greater than 3 or  smaller than -3. There are no outliers.</p>

```
dominos.nlargest(5,'Volume')
```

<p>The top 5 days where traders were the most active are 2020-05-11, 2020-02-20, 2020-10-08, 2021-07-22, and 2021-02-25</p>


```
dominos.nsmallest(5,'Volume')
```

<p>The top 5 days where traders were the least active are 2020-12-24, 2021-08-26, 2021-08-27, 2021-09-29, and 2021-09-23.</p>

```
dominos.drop('Adj Close', axis = 1, inplace = True)
```

<p>Dropped Adj Close and will only look at the Close column.</p>

```
plt.figure(figsize=(12, 6))
plt.subplot(2, 1, 1)
plt.plot(dominos.index, dominos['Volume'], color='blue', label='Trading Volume')
plt.title('Trading Volume Over Time')
plt.xlabel('Date')
plt.ylabel('Trading Volume')
plt.legend()
```

![image](https://github.com/kenmamich/Domino-Stock-Price-Analysis/assets/137173829/f2cf866e-faca-46a6-89d3-6124bd6cd494)


<p>Visualization displaying trading volume over time. The graph shows relative stability in trader activity with steep peaks scattered throughout, which indicates heavy investor activity on certain days.</p>

```
plt.figure(figsize=(12, 6))
plt.subplot(2, 1, 2)
plt.plot(dominos.index, dominos['Close'], color='green', label='Closing Price')
plt.title('Closing Price Over Time')
plt.xlabel('Date')
plt.ylabel('Closing Price')
plt.legend()
```

![image](https://github.com/kenmamich/Domino-Stock-Price-Analysis/assets/137173829/5884c416-16c0-4e15-8d3d-5c49bfac6632)


<p>Visualization displaying the change in closing price over time. The graph shows that as time passes, the stock price is experiencing a gradual upward trend showing dominos could be a potentially good stock to hold in the long term. </p>

```
dominos.reset_index(inplace=True)
dominos['Return'] = dominos['Close'].pct_change() * 100
plt.figure(figsize=(12, 6))
plt.plot(dominos['Date'], dominos['Return'], label='Daily Returns', color='blue')
plt.title('Daily Returns for Domino\'s Stock')
plt.xlabel('Date')
plt.ylabel('Percentage Change (%)')
plt.legend()
plt.grid(True)
plt.show()

```

![image](https://github.com/kenmamich/Domino-Stock-Price-Analysis/assets/137173829/d606cc59-e6a1-44c2-9479-0ab734c1d88b)


<p>Created a return column showing daily returns as a percentage and made a visualization displaying daily returns for the Domino stock price. The graph displays unpredictability in daily returns in the stock price as expected, additionally, shows relative stability around 0% daily returns.</p>


```
dominos['Year'] = dominos['Date'].dt.year
dominos.groupby('Year')['Return'].mean()
```

<p>Creates Year column and finds the average daily return for each year. 2019 had an average daily return of 0.270001%, 2020 had an average of 0.141122%, and 2021 had an average of 0.098777%.</p>

```
conin = (dominos['Return']> 0)
conin.groupby((~conin).cumsum()).cumcount().max() + 1

conde = (dominos['Return']> 0)
conde.groupby((~conde).cumsum()).cumcount().max() + 1 
```

<p>During the period covered by the dataset, the longest consecutive positive return days were 8 and the longest consecutive negative return days were 8, as well. </p>


```
dominos['Price Gap'] = (dominos['Open'] - dominos['Close'].shift(1))
dominos['Price Gap'].fillna(0,inplace=True)
dominos[dominos['Price Gap'].notna()].apply(lambda row: row['Return'], axis=1).mean()
```

<p>Creates the Price Gap column, which is the difference between one day's opening price and the previous day's closing price. The purpose of creating this column is to see if there was an after-market stock price movement and whether investing in the opportunity to pursue after-marketing trading is worth it for the Domino stock. All possible na values were filled with 0 and found the average price gap to be 0.13796142236277276 indicating there is after-market stock price movement.</p>

```
dominos['Price Gap'].mean()
```

<p>Finds the mean price gap, as well, producing the output 0.17310891089108943, which is different than the average before.</p>


```
investment_horizon = 252 
total_return = (1 + dominos['Return'] / 100).cumprod().tail(investment_horizon).prod() - 1
total_return
```

<p>The total return for one trading year represented in the dataset, which is 252 days, is 9.036274116179685e+54%. This is on par with the S&P 500 seeing average yearly returns of 8-12% each investment year.</p>

```
dominos['Price Range'] = dominos['High'] - dominos['Low']
dominos.groupby('Year')['Price Range'].mean()
```

<p>Creates price range column which is the difference between that day's high and its low. The purpose of creating this column is to display how much the stock price moved around for the day. I also found the average price gap for each year. 2019 is 5.569245, 2020 is 10.744111, and 2021 is 8.764523. 


```
(dominos['Return'] > 0).apply(lambda x: x).mean() * 100
dominos['Return'].apply(lambda x: 1 if x > 0 else 0).sum()

(dominos['Return'] < 0).apply(lambda x: x).mean() * 100
dominos['Return'].apply(lambda x: 1 if x < 0 else 0).sum()

(dominos['Return'] == 0).apply(lambda x: x).mean() * 100
dominos['Return'].apply(lambda x: 1 if x == 0 else 0).sum()

```

<p>Wanted to find how many days have positive, negative, or zero returns. Found that 260 days experienced positive returns, 241 days experienced negative returns, and 4 days experienced no returns. The stock price saw more positive return days than negative return days. </p>


```
features = dominos[['Open', 'High', 'Low', 'Close', 'Volume']]
features_standardized = (features - features.mean()) / features.std()
kmeans = KMeans(n_clusters=3, random_state=42)
dominos['Cluster'] = kmeans.fit_predict(features_standardized)
plt.scatter(dominos.index, dominos['Close'], c=dominos['Cluster'], cmap='viridis')
plt.title('Clustering of Domino\'s Pizza Stock Prices')
plt.xlabel('Days')
plt.ylabel('Adj Close Price')
plt.show()
```

![image](https://github.com/kenmamich/Domino-Stock-Price-Analysis/assets/137173829/caa43bce-4f11-42a7-bccd-2f06f228bffb)

<p>Used Kmeans clustering on the dataset and categorized the data points into three different clusters: green, purple, and yellow. Yellow displayed stock price trends that were unstable and erratic, purple displayed stock price trends that were relatively stable, and green represented stock price trends displaying gradual and stable increases.</p>




