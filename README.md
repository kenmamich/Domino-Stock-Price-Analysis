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

<h3>Analysis</h3>

```
dominos.describe()
```
<p>Found the summary statistics for each column.</p>

```
        Open	       High	      Low        Close	     Adj Close	       Volume
count	505.000000	505.000000	505.000000	505.000000	505.000000	5.050000e+02
mean	385.391723	390.291347	380.870416	385.614693	382.575584	6.996162e+05
std	67.485552	    67.673877	 67.327659	   67.378659	  68.525904	  5.993578e+05
min	254.890000	257.610000	253.080000	255.700000	251.350000	1.843000e+05
25%	353.730000	360.580000	347.730000	354.620000	350.690000	4.495000e+05
50%	384.600000	387.580000	380.910000	383.920000	380.590000	5.776000e+05
75%	418.090000	423.110000	415.740000	419.700000	415.290000	7.863000e+05
max	541.990000	548.720000	536.110000	540.470000	539.480000	1.027880e+07
```





