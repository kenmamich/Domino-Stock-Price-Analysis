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

<p>Decided to drop Adj Close.</p>

```
plt.figure(figsize=(12, 6))
plt.subplot(2, 1, 1)
plt.plot(dominos.index, dominos['Volume'], color='blue', label='Trading Volume')
plt.title('Trading Volume Over Time')
plt.xlabel('Date')
plt.ylabel('Trading Volume')
plt.legend()
```

<p>Ploted a visualization displaying trading volume over time.</p>

```
plt.figure(figsize=(12, 6))
plt.subplot(2, 1, 2)
plt.plot(dominos.index, dominos['Close'], color='green', label='Closing Price')
plt.title('Closing Price Over Time')
plt.xlabel('Date')
plt.ylabel('Closing Price')
plt.legend()
```

<p>Ploted a visualization displaying the change is closing price over time.</p>




