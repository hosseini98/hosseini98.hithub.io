
# Customer value analysis

The goal of this post is to analyse a dataset and provide some useful information regarding customers demographics and buying behaviour.  I choose to qork on a dataset from IBM Watson Analytics (available [here](https://www.ibm.com/communities/analytics/watson-analytics-blog/guide-to-sample-datasets/)). I will provide answers to assigned questions by visualization methods in Python. This dataset will show us the most profitable customers and how they interact. I will show you how you can increase profitable customer response, retention and growth. let's start!




```python
import pandas as pd
import numpy as np
import os
# Load the raw data using the read_csv object
df = pd.read_csv("WA_Fn_UseC__Marketing_Customer_Value_Analysis.csv")
```

## Introduction to dataset

There were a total of 24 variables in this dataset . The variables and the description of the values are as follows
1. Customer: The customer ID number
2. State
3. Customer lifetime value: CLV present the finantial value of a customer. It is a prediction of the net profit attributed to the customer during his/her entire relationship with the company. CLV is highly important in shaping managers’ decisions. 
4. Reponse: Yes or no response
5. Coverage: Coverage type eigher basic, Premium, or extended
6. Education
7. Effective to Date
8. Employmnet status
9. Employed or unemployed
10. Gender: F or M
11. Income
12. Location Code
13. Marital Status
14. Monthly Premium Auto
15. Months Since Policy inception
16. Number of Open Complaints
17. Number of Polices
18. Policy Type
19. Policy
20. Renew Offer Type
21. Sales Channel
22. Total Claim Amount
23. Vehicle Class
24. Vehicle Size



```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Customer</th>
      <th>State</th>
      <th>Customer Lifetime Value</th>
      <th>Response</th>
      <th>Coverage</th>
      <th>Education</th>
      <th>Effective To Date</th>
      <th>EmploymentStatus</th>
      <th>Gender</th>
      <th>Income</th>
      <th>...</th>
      <th>Months Since Policy Inception</th>
      <th>Number of Open Complaints</th>
      <th>Number of Policies</th>
      <th>Policy Type</th>
      <th>Policy</th>
      <th>Renew Offer Type</th>
      <th>Sales Channel</th>
      <th>Total Claim Amount</th>
      <th>Vehicle Class</th>
      <th>Vehicle Size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>BU79786</td>
      <td>Washington</td>
      <td>2763.519279</td>
      <td>No</td>
      <td>Basic</td>
      <td>Bachelor</td>
      <td>2/24/11</td>
      <td>Employed</td>
      <td>F</td>
      <td>56274</td>
      <td>...</td>
      <td>5</td>
      <td>0</td>
      <td>1</td>
      <td>Corporate Auto</td>
      <td>Corporate L3</td>
      <td>Offer1</td>
      <td>Agent</td>
      <td>384.811147</td>
      <td>Two-Door Car</td>
      <td>Medsize</td>
    </tr>
    <tr>
      <th>1</th>
      <td>QZ44356</td>
      <td>Arizona</td>
      <td>6979.535903</td>
      <td>No</td>
      <td>Extended</td>
      <td>Bachelor</td>
      <td>1/31/11</td>
      <td>Unemployed</td>
      <td>F</td>
      <td>0</td>
      <td>...</td>
      <td>42</td>
      <td>0</td>
      <td>8</td>
      <td>Personal Auto</td>
      <td>Personal L3</td>
      <td>Offer3</td>
      <td>Agent</td>
      <td>1131.464935</td>
      <td>Four-Door Car</td>
      <td>Medsize</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AI49188</td>
      <td>Nevada</td>
      <td>12887.431650</td>
      <td>No</td>
      <td>Premium</td>
      <td>Bachelor</td>
      <td>2/19/11</td>
      <td>Employed</td>
      <td>F</td>
      <td>48767</td>
      <td>...</td>
      <td>38</td>
      <td>0</td>
      <td>2</td>
      <td>Personal Auto</td>
      <td>Personal L3</td>
      <td>Offer1</td>
      <td>Agent</td>
      <td>566.472247</td>
      <td>Two-Door Car</td>
      <td>Medsize</td>
    </tr>
    <tr>
      <th>3</th>
      <td>WW63253</td>
      <td>California</td>
      <td>7645.861827</td>
      <td>No</td>
      <td>Basic</td>
      <td>Bachelor</td>
      <td>1/20/11</td>
      <td>Unemployed</td>
      <td>M</td>
      <td>0</td>
      <td>...</td>
      <td>65</td>
      <td>0</td>
      <td>7</td>
      <td>Corporate Auto</td>
      <td>Corporate L2</td>
      <td>Offer1</td>
      <td>Call Center</td>
      <td>529.881344</td>
      <td>SUV</td>
      <td>Medsize</td>
    </tr>
    <tr>
      <th>4</th>
      <td>HB64268</td>
      <td>Washington</td>
      <td>2813.692575</td>
      <td>No</td>
      <td>Basic</td>
      <td>Bachelor</td>
      <td>2/3/11</td>
      <td>Employed</td>
      <td>M</td>
      <td>43836</td>
      <td>...</td>
      <td>44</td>
      <td>0</td>
      <td>1</td>
      <td>Personal Auto</td>
      <td>Personal L1</td>
      <td>Offer1</td>
      <td>Agent</td>
      <td>138.130879</td>
      <td>Four-Door Car</td>
      <td>Medsize</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 24 columns</p>
</div>




```python
# the size or structure of the dataset: 24 columns and over 9000 rows. 
df.shape
```




    (9134, 24)



Dataset comprises of 9134 observations and 24 characteristics.

## investigating the dataset
First, I will get some basic information regarding this dataset.


```python
df.isnull().sum()
```




    Customer                         0
    State                            0
    Customer Lifetime Value          0
    Response                         0
    Coverage                         0
    Education                        0
    Effective To Date                0
    EmploymentStatus                 0
    Gender                           0
    Income                           0
    Location Code                    0
    Marital Status                   0
    Monthly Premium Auto             0
    Months Since Last Claim          0
    Months Since Policy Inception    0
    Number of Open Complaints        0
    Number of Policies               0
    Policy Type                      0
    Policy                           0
    Renew Offer Type                 0
    Sales Channel                    0
    Total Claim Amount               0
    Vehicle Class                    0
    Vehicle Size                     0
    dtype: int64



The data look clean since there is no NaN value in our feature culumns. The data types have been also properly set. 


```python
df.dtypes
```




    Customer                          object
    State                             object
    Customer Lifetime Value          float32
    Response                          object
    Coverage                          object
    Education                         object
    Effective To Date                 object
    EmploymentStatus                  object
    Gender                            object
    Income                             int64
    Location Code                     object
    Marital Status                    object
    Monthly Premium Auto               int64
    Months Since Last Claim            int64
    Months Since Policy Inception      int64
    Number of Open Complaints          int64
    Number of Policies                 int64
    Policy Type                       object
    Policy                            object
    Renew Offer Type                  object
    Sales Channel                     object
    Total Claim Amount               float64
    Vehicle Class                     object
    Vehicle Size                      object
    dtype: object



## Exploring the features
Now lets build up some graphs to explore the features (variables).


```python
import seaborn as sb
import scipy
from scipy.stats.stats import pearsonr
from scipy.stats import spearmanr
from scipy.stats import chi2_contingency
from IPython.display import display
pd.options.display.max_columns = None
import matplotlib.pyplot as plt
sb.set()
import pylab as pl
from pylab import rcParams
from matplotlib import cm
import plotly.graph_objs as go
import plotly.offline as py
import os
import plotly.tools as tls
import plotly.figure_factory as ff
import matplotlib.ticker as mtick

%matplotlib inline
#rcParams['figure.figsize'] = 28, 4
sb.set_style('whitegrid')
```


```python
df["Vehicle Class"].value_counts()
```




    Four-Door Car    4621
    Two-Door Car     1886
    SUV              1796
    Sports Car        484
    Luxury SUV        184
    Luxury Car        163
    Name: Vehicle Class, dtype: int64




```python
df["Education"].value_counts()
```




    Bachelor                2748
    College                 2681
    High School or Below    2622
    Master                   741
    Doctor                   342
    Name: Education, dtype: int64




```python
df["Coverage"].value_counts()
```




    Basic       5568
    Extended    2742
    Premium      824
    Name: Coverage, dtype: int64




```python
df["State"].value_counts()
```




    California    3150
    Oregon        2601
    Arizona       1703
    Nevada         882
    Washington     798
    Name: State, dtype: int64




```python
df["Customer Lifetime Value"].mean()
```




    8004.93017578125



The describe() function in pandas is useful in getting various statistics. As shown below, this function returns the count, mean, standard deviation, minimum and maximum values and the quantiles of the dataset. We can see that a notably large difference between 75% tile and max values of most of our features. this implys that we have some Outliers in our data set.


```python
df.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Customer Lifetime Value</th>
      <th>Income</th>
      <th>Monthly Premium Auto</th>
      <th>Months Since Last Claim</th>
      <th>Months Since Policy Inception</th>
      <th>Number of Open Complaints</th>
      <th>Number of Policies</th>
      <th>Total Claim Amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>9134.000000</td>
      <td>9134.000000</td>
      <td>9134.000000</td>
      <td>9134.000000</td>
      <td>9134.000000</td>
      <td>9134.000000</td>
      <td>9134.000000</td>
      <td>9134.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>8004.930176</td>
      <td>37657.380009</td>
      <td>93.219291</td>
      <td>15.097000</td>
      <td>48.064594</td>
      <td>0.384388</td>
      <td>2.966170</td>
      <td>434.088794</td>
    </tr>
    <tr>
      <th>std</th>
      <td>6870.965820</td>
      <td>30379.904734</td>
      <td>34.407967</td>
      <td>10.073257</td>
      <td>27.905991</td>
      <td>0.910384</td>
      <td>2.390182</td>
      <td>290.500092</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1898.007690</td>
      <td>0.000000</td>
      <td>61.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.099007</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>3994.251831</td>
      <td>0.000000</td>
      <td>68.000000</td>
      <td>6.000000</td>
      <td>24.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>272.258244</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>5780.182129</td>
      <td>33889.500000</td>
      <td>83.000000</td>
      <td>14.000000</td>
      <td>48.000000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>383.945434</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>8962.166992</td>
      <td>62320.000000</td>
      <td>109.000000</td>
      <td>23.000000</td>
      <td>71.000000</td>
      <td>0.000000</td>
      <td>4.000000</td>
      <td>547.514839</td>
    </tr>
    <tr>
      <th>max</th>
      <td>83325.382812</td>
      <td>99981.000000</td>
      <td>298.000000</td>
      <td>35.000000</td>
      <td>99.000000</td>
      <td>5.000000</td>
      <td>9.000000</td>
      <td>2893.239678</td>
    </tr>
  </tbody>
</table>
</div>



## Data visualization
This was just a glimpse of this dataset. Let’s use visualization methods in Python and explore data with some  graphs. Python's Seaborn library, built on top of matplotlib, will be used to get statistical graphs in order to perform Univariate and Multivariate analysis.


```python
sb.pairplot(df)
```




    <seaborn.axisgrid.PairGrid at 0x1a27ead860>




![png](output_20_1.png)



```python
df.corr()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Customer Lifetime Value</th>
      <th>Income</th>
      <th>Monthly Premium Auto</th>
      <th>Months Since Last Claim</th>
      <th>Months Since Policy Inception</th>
      <th>Number of Open Complaints</th>
      <th>Number of Policies</th>
      <th>Total Claim Amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Customer Lifetime Value</th>
      <td>1.000000</td>
      <td>0.024366</td>
      <td>0.396262</td>
      <td>0.011517</td>
      <td>0.009418</td>
      <td>-0.036343</td>
      <td>0.021955</td>
      <td>0.226451</td>
    </tr>
    <tr>
      <th>Income</th>
      <td>0.024366</td>
      <td>1.000000</td>
      <td>-0.016665</td>
      <td>-0.026715</td>
      <td>-0.000875</td>
      <td>0.006408</td>
      <td>-0.008656</td>
      <td>-0.355254</td>
    </tr>
    <tr>
      <th>Monthly Premium Auto</th>
      <td>0.396262</td>
      <td>-0.016665</td>
      <td>1.000000</td>
      <td>0.005026</td>
      <td>0.020257</td>
      <td>-0.013122</td>
      <td>-0.011233</td>
      <td>0.632017</td>
    </tr>
    <tr>
      <th>Months Since Last Claim</th>
      <td>0.011517</td>
      <td>-0.026715</td>
      <td>0.005026</td>
      <td>1.000000</td>
      <td>-0.042959</td>
      <td>0.005354</td>
      <td>0.009136</td>
      <td>0.007563</td>
    </tr>
    <tr>
      <th>Months Since Policy Inception</th>
      <td>0.009418</td>
      <td>-0.000875</td>
      <td>0.020257</td>
      <td>-0.042959</td>
      <td>1.000000</td>
      <td>-0.001158</td>
      <td>-0.013333</td>
      <td>0.003335</td>
    </tr>
    <tr>
      <th>Number of Open Complaints</th>
      <td>-0.036343</td>
      <td>0.006408</td>
      <td>-0.013122</td>
      <td>0.005354</td>
      <td>-0.001158</td>
      <td>1.000000</td>
      <td>0.001498</td>
      <td>-0.014241</td>
    </tr>
    <tr>
      <th>Number of Policies</th>
      <td>0.021955</td>
      <td>-0.008656</td>
      <td>-0.011233</td>
      <td>0.009136</td>
      <td>-0.013333</td>
      <td>0.001498</td>
      <td>1.000000</td>
      <td>-0.002354</td>
    </tr>
    <tr>
      <th>Total Claim Amount</th>
      <td>0.226451</td>
      <td>-0.355254</td>
      <td>0.632017</td>
      <td>0.007563</td>
      <td>0.003335</td>
      <td>-0.014241</td>
      <td>-0.002354</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
sb.heatmap(df.corr(), xticklabels=df.corr().columns.values, yticklabels=df.corr().columns.values, annot=True)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a2d7df0b8>




![png](output_22_1.png)



```python
spearmanr(df["Customer Lifetime Value"], df["Total Claim Amount"])
```




    SpearmanrResult(correlation=0.2105979582520612, pvalue=4.351847002636142e-92)




```python
chi2_contingency
```




    <function scipy.stats.contingency.chi2_contingency>




```python
table = pd.crosstab(df.Gender,df["EmploymentStatus"])
```


```python
table
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>EmploymentStatus</th>
      <th>Disabled</th>
      <th>Employed</th>
      <th>Medical Leave</th>
      <th>Retired</th>
      <th>Unemployed</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>F</th>
      <td>244</td>
      <td>2937</td>
      <td>214</td>
      <td>128</td>
      <td>1135</td>
    </tr>
    <tr>
      <th>M</th>
      <td>161</td>
      <td>2761</td>
      <td>218</td>
      <td>154</td>
      <td>1182</td>
    </tr>
  </tbody>
</table>
</div>




```python
chi2, p, dof, expected = chi2_contingency(table.values)
```


```python
rcParams['figure.figsize'] = 14, 5
gb = df.groupby("Gender")["Education"].value_counts().to_frame().rename({"Education": "Numbers"}, axis = 1).reset_index()

```


```python
gb
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Gender</th>
      <th>Education</th>
      <th>Numbers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>F</td>
      <td>Bachelor</td>
      <td>1423</td>
    </tr>
    <tr>
      <th>1</th>
      <td>F</td>
      <td>College</td>
      <td>1352</td>
    </tr>
    <tr>
      <th>2</th>
      <td>F</td>
      <td>High School or Below</td>
      <td>1321</td>
    </tr>
    <tr>
      <th>3</th>
      <td>F</td>
      <td>Master</td>
      <td>393</td>
    </tr>
    <tr>
      <th>4</th>
      <td>F</td>
      <td>Doctor</td>
      <td>169</td>
    </tr>
    <tr>
      <th>5</th>
      <td>M</td>
      <td>College</td>
      <td>1329</td>
    </tr>
    <tr>
      <th>6</th>
      <td>M</td>
      <td>Bachelor</td>
      <td>1325</td>
    </tr>
    <tr>
      <th>7</th>
      <td>M</td>
      <td>High School or Below</td>
      <td>1301</td>
    </tr>
    <tr>
      <th>8</th>
      <td>M</td>
      <td>Master</td>
      <td>348</td>
    </tr>
    <tr>
      <th>9</th>
      <td>M</td>
      <td>Doctor</td>
      <td>173</td>
    </tr>
  </tbody>
</table>
</div>




```python
sb.barplot(x = "Gender", y = "Numbers", data = gb, hue = "Education", palette = sb.color_palette("hls", 9)).set_title("Education vs. Gender");
```


![png](output_30_0.png)



```python
rcParams['figure.figsize'] = 12, 5
gb = df.groupby("Vehicle Class")["Vehicle Size"].value_counts().to_frame().rename({"Vehicle Size": "Numbers"}, axis = 1).reset_index()
sb.barplot(x = "Vehicle Class", y = "Numbers", data = gb, hue = "Vehicle Size", palette = sb.color_palette("hls", 3)).set_title("Education vs. Gender");

```


![png](output_31_0.png)



```python
rcParams['figure.figsize'] = 12, 5
gb = df.groupby("Vehicle Class")["Coverage"].value_counts().to_frame().rename({"Coverage": "Numbers"}, axis = 1).reset_index()
sb.barplot(x = "Vehicle Class", y = "Numbers", data = gb, hue = "Coverage", palette = sb.color_palette("hls", 12)).set_title("Education vs. Gender");

```


![png](output_32_0.png)



```python
rcParams['figure.figsize'] = 12, 5
gb = df.groupby("Vehicle Size")["Coverage"].value_counts().to_frame().rename({"Coverage": "Numbers"}, axis = 1).reset_index()
sb.barplot(x = "Vehicle Size", y = "Numbers", data = gb, hue = "Coverage", palette = sb.color_palette("hls", 7)).set_title("Education vs. Gender");

```


![png](output_33_0.png)



```python
rcParams['figure.figsize'] = 10, 5
gb = df.groupby("Policy")["Policy Type"].value_counts().to_frame().rename({"Policy Type": "Numbers"}, axis = 1).reset_index()
sb.barplot(x = "Policy", y = "Numbers", data = gb, hue = "Policy Type", palette = sb.color_palette("hls", 5)).set_title("Education vs. Gender");

```


![png](output_34_0.png)



```python
gb = df.groupby("State").agg({"Customer Lifetime Value":'mean'}).rename({"Customer Lifetime Value": "Customer Lifetime Value Mean"}, axis = 1).reset_index()
```


```python
gb
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Customer Lifetime Value Mean</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Arizona</td>
      <td>7861.341309</td>
    </tr>
    <tr>
      <th>1</th>
      <td>California</td>
      <td>8003.647949</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Nevada</td>
      <td>8056.707031</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Oregon</td>
      <td>8077.901367</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Washington</td>
      <td>8021.472168</td>
    </tr>
  </tbody>
</table>
</div>




```python

sb.set(style="whitegrid")
sb.barplot(x = "State", y = "Customer Lifetime Value Mean", data = gb, hue = "Customer Lifetime Value Mean" , palette = sb.color_palette("hls", 5)).set_title("The Mean value of the Customer Lifetime Value in each state");
#plt.legend(loc='best')
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)

```




    <matplotlib.legend.Legend at 0x1a2ba6beb8>




![png](output_37_1.png)



```python
sb.catplot(x="Customer Lifetime Value",y="Policy",data=df)
```




    <seaborn.axisgrid.FacetGrid at 0x1a2b53e668>




![png](output_38_1.png)



```python
g = sb.relplot(x="Monthly Premium Auto", y="Customer Lifetime Value",size='Income', kind="scatter", data=df)
g.fig.autofmt_xdate()
```


![png](output_39_0.png)



```python
df["Customer Lifetime Value"]=df["Customer Lifetime Value"].astype('float32')
```


```python

CLVunder10000 = df["Customer Lifetime Value"][(df["Customer Lifetime Value"] <= 10000.0)]
CLV10000_20000 = df["Customer Lifetime Value"][(df["Customer Lifetime Value"] <= 20000.0) & (df["Customer Lifetime Value"] >= 10000.1)]
CLV20000_40000 = df["Customer Lifetime Value"][(df["Customer Lifetime Value"] <= 40000.0) & (df["Customer Lifetime Value"] >= 20000.1)]
CLV40000_60000 = df["Customer Lifetime Value"][(df["Customer Lifetime Value"] <= 60000.0) & (df["Customer Lifetime Value"] >= 40000.1)]
CLVabove60000 = df["Customer Lifetime Value"][df["Customer Lifetime Value"] >= 60000.1]

CLV_range = ["1898(min)_ 10000","10000_20000","20000_40000","40000_60000","60000-83325 (max)"]
CLV_count = [len(CLVunder10000.values),len(CLV10000_20000.values),len(CLV20000_40000.values),len(CLV40000_60000.values),len(CLVabove60000.values)]

plt.figure(figsize=(8,5))
plt.bar(CLV_range, CLV_count, width=0.7, color = "black", align='center')
plt.show()
```


![png](output_41_0.png)



```python
CLV_count
```




    [7248, 1311, 515, 51, 9]




```python
df["Customer Lifetime Value"].describe()
```




    count     9134.000000
    mean      8004.930176
    std       6870.965820
    min       1898.007690
    25%       3994.251831
    50%       5780.182129
    75%       8962.166992
    max      83325.382812
    Name: Customer Lifetime Value, dtype: float64




```python
sb.catplot(x="Customer Lifetime Value", y="Coverage", hue="State", kind="swarm", data=df);
```


![png](output_44_0.png)



```python
sb.catplot(x="Customer Lifetime Value",y="Marital Status",kind='box',data=df)
```




    <seaborn.axisgrid.FacetGrid at 0x1a2b7d52e8>




![png](output_45_1.png)



```python
sb.catplot(x="Customer Lifetime Value",y="Marital Status", hue = "Gender", kind='box',data=df)
```




    <seaborn.axisgrid.FacetGrid at 0x1a2b8c2940>




![png](output_46_1.png)



```python
sb.catplot(x="Customer Lifetime Value",y="EmploymentStatus",kind='violin',data=df)
```




    <seaborn.axisgrid.FacetGrid at 0x1a2c52f080>




![png](output_47_1.png)



```python
rcParams['figure.figsize'] = 14, 8
ax = sb.catplot(y='Monthly Premium Auto',x='Months Since Policy Inception',hue='Gender',kind='point',data=df)
ax.fig.autofmt_xdate()
ax.
```


![png](output_48_0.png)



```python
sb.catplot(x="Sales Channel", y="Customer Lifetime Value", hue="Renew Offer Type", kind="swarm", data=df);
```


![png](output_49_0.png)



```python
df2=df[['Monthly Premium Auto', 'Months Since Last Claim', 'Months Since Policy Inception', 'Number of Open Complaints', 'Number of Policies', 'Customer Lifetime Value', 'Income', 'Total Claim Amount' ]]
```


```python
number_of_rows=1
number_of_columns=8
l = df2.columns.values
plt.figure(figsize=(6*number_of_columns,8*number_of_rows))
for i in range(0,len(l)):
    plt.subplot(number_of_rows + 1,number_of_columns,i+1)
    sb.distplot(df[l[i]],kde=True) 
```


![png](output_51_0.png)



```python
l = df2.columns.values
number_of_columns=8
number_of_rows = 1
plt.figure()
for i in range(0,len(l)):
    plt.subplot(number_of_rows + 1,number_of_columns,i+1)
    sb.set_style('whitegrid')
    sb.boxplot(df2[l[i]],color='green',orient='v')
    plt.tight_layout()
```


![png](output_52_0.png)

