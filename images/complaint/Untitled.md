
## Introduction
Here, I will clean data from Financial Services Consumer Complaint Database which is publicly avalable in this link: https://catalog.data.gov/dataset/consumer-complaint-database#topic=consumer_navigation. As described in this webpage, it contains "data from the complaints received by the Consumer Financial Protection Bureau (CFPB) on financial products and services, including bank accounts, credit cards, credit reporting, debt collection, money transfers, mortgages, student loans, and other types of consumer credit. Data available about each complaint includes the name of the provider, type of complaint, date, zip code, and other information."


## Data cleaning
Here is the road map for what I will do to cleaning process.  
### Import the libraries to set up the envirenment and load the data


```python
import pandas as pd
import numpy as np
import os
# Load the raw data using the read_csv object
df = pd.read_csv("/Users/hossein/Consumer_Complaints/Consumer_Complaints.csv")
```

    /Users/hossein/anaconda3/lib/python3.6/site-packages/IPython/core/interactiveshell.py:2698: DtypeWarning:
    
    Columns (5,6,11,16) have mixed types. Specify dtype option on import or set low_memory=False.
    


### Investigate the data
Panda provides enough tools to get information about your raw data by just a brief line of code. Let's look at the top of our dataset by executing the following command:


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
      <th>Date received</th>
      <th>Product</th>
      <th>Sub-product</th>
      <th>Issue</th>
      <th>Sub-issue</th>
      <th>Consumer complaint narrative</th>
      <th>Company public response</th>
      <th>Company</th>
      <th>State</th>
      <th>ZIP code</th>
      <th>Tags</th>
      <th>Consumer consent provided?</th>
      <th>Submitted via</th>
      <th>Date sent to company</th>
      <th>Company response to consumer</th>
      <th>Timely response?</th>
      <th>Consumer disputed?</th>
      <th>Complaint ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>02/04/2019</td>
      <td>Credit card or prepaid card</td>
      <td>General-purpose credit card or charge card</td>
      <td>Getting a credit card</td>
      <td>Sent card you never applied for</td>
      <td>NaN</td>
      <td>Company believes it acted appropriately as aut...</td>
      <td>First Federal Credit Control</td>
      <td>FL</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Web</td>
      <td>02/04/2019</td>
      <td>Closed with explanation</td>
      <td>Yes</td>
      <td>NaN</td>
      <td>3141563</td>
    </tr>
    <tr>
      <th>1</th>
      <td>02/04/2019</td>
      <td>Debt collection</td>
      <td>Payday loan debt</td>
      <td>False statements or representation</td>
      <td>Attempted to collect wrong amount</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>ONEMAIN FINANCIAL HOLDINGS, LLC.</td>
      <td>MI</td>
      <td>NaN</td>
      <td>Older American</td>
      <td>NaN</td>
      <td>Web</td>
      <td>02/04/2019</td>
      <td>In progress</td>
      <td>Yes</td>
      <td>NaN</td>
      <td>3141667</td>
    </tr>
    <tr>
      <th>2</th>
      <td>02/04/2019</td>
      <td>Credit reporting, credit repair services, or o...</td>
      <td>Credit reporting</td>
      <td>Incorrect information on your report</td>
      <td>Information belongs to someone else</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NCC Business Services, Inc.</td>
      <td>TX</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Web</td>
      <td>02/04/2019</td>
      <td>In progress</td>
      <td>Yes</td>
      <td>NaN</td>
      <td>3142226</td>
    </tr>
    <tr>
      <th>3</th>
      <td>02/04/2019</td>
      <td>Mortgage</td>
      <td>Conventional home mortgage</td>
      <td>Struggling to pay mortgage</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>BAYVIEW LOAN SERVICING, LLC</td>
      <td>CA</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Other</td>
      <td>Web</td>
      <td>02/04/2019</td>
      <td>In progress</td>
      <td>Yes</td>
      <td>NaN</td>
      <td>3142610</td>
    </tr>
    <tr>
      <th>4</th>
      <td>02/04/2019</td>
      <td>Debt collection</td>
      <td>I do not know</td>
      <td>Attempts to collect debt not owed</td>
      <td>Debt was paid</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Diversified Consultants, Inc.</td>
      <td>OH</td>
      <td>45044</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Web</td>
      <td>02/04/2019</td>
      <td>In progress</td>
      <td>Yes</td>
      <td>NaN</td>
      <td>3141862</td>
    </tr>
  </tbody>
</table>
</div>



A quick look at this dataset,  we can see that in some of our columns, we have NaN values. I will get back to this but first get some information about the structure of our data bu executing _df.dtypes_ and _df.info()_. 


```python
df.shape
```




    (1210607, 18)




```python
df.dtypes.to_frame(name='Type') # df.info()  is also return back data type plus other useful info. 
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
      <th>Type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Date received</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Product</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Sub-product</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Issue</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Sub-issue</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Consumer complaint narrative</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Company public response</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Company</th>
      <td>object</td>
    </tr>
    <tr>
      <th>State</th>
      <td>object</td>
    </tr>
    <tr>
      <th>ZIP code</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Tags</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Consumer consent provided?</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Submitted via</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Date sent to company</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Company response to consumer</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Timely response?</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Consumer disputed?</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Complaint ID</th>
      <td>int64</td>
    </tr>
  </tbody>
</table>
</div>



Looking at the output of these two commands, we can see we have 18 columns (features) and over 1.2M rows. So this is a pretty huge dataset. Another important thing that we need to consider is understanding data types. A data type is essentially an inner construct that a programming language recruits to understand how to store and manipulate data. Here we can see that all of our features are objects (strings) but the Complaint ID being an integer type. later on, I will convert some of these data types to other ones as needed. 

Ok, now let's see how many missing (NAN) values we have in each colum. The following command provides this information for us. It sounds like we have considarable missing values in 9 columns. I will fill them mostly by a "Not given" str type in next section.  



```python
df.isnull().sum().to_frame(name='missing counts')
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
      <th>missing counts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Date received</th>
      <td>0</td>
    </tr>
    <tr>
      <th>Product</th>
      <td>0</td>
    </tr>
    <tr>
      <th>Sub-product</th>
      <td>235166</td>
    </tr>
    <tr>
      <th>Issue</th>
      <td>0</td>
    </tr>
    <tr>
      <th>Sub-issue</th>
      <td>521887</td>
    </tr>
    <tr>
      <th>Consumer complaint narrative</th>
      <td>846404</td>
    </tr>
    <tr>
      <th>Company public response</th>
      <td>797586</td>
    </tr>
    <tr>
      <th>Company</th>
      <td>0</td>
    </tr>
    <tr>
      <th>State</th>
      <td>17098</td>
    </tr>
    <tr>
      <th>ZIP code</th>
      <td>104875</td>
    </tr>
    <tr>
      <th>Tags</th>
      <td>1045378</td>
    </tr>
    <tr>
      <th>Consumer consent provided?</th>
      <td>564853</td>
    </tr>
    <tr>
      <th>Submitted via</th>
      <td>0</td>
    </tr>
    <tr>
      <th>Date sent to company</th>
      <td>0</td>
    </tr>
    <tr>
      <th>Company response to consumer</th>
      <td>6</td>
    </tr>
    <tr>
      <th>Timely response?</th>
      <td>0</td>
    </tr>
    <tr>
      <th>Consumer disputed?</th>
      <td>442087</td>
    </tr>
    <tr>
      <th>Complaint ID</th>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### Clean up the data
Let's start with "date columns". We have two colums that we need to convert themfrom strings to datetime objects. Pandas to_datetime command can easyly do this job for us: 


```python
df["Date received"] = pd.to_datetime(df["Date received"])
df['Date sent to company'] = pd.to_datetime(df['Date sent to company'])

```

There are three columns in our dataset that can be considered as Boolean type.  The following three lines of commands will execute this conversion by recruiting Numpy's "where" module. The idea is to convert all "Yes" values  to True and everything else will be assigned to False (in case of the third line, "Consent not provided" to False and the rest to True ). 

The basic idea is to use the np.where() function to convert all “Y” values to True and everything else assigned False





```python
df["Timely response?"] = np.where(df["Timely response?"] == "Yes", True, False)
df["Consumer disputed?"] = np.where(df["Consumer disputed?"] == "Yes", True, False)
df["Consumer consent provided?"] = np.where(df["Consumer consent provided?"] == "Consent not provided", False, True)
```

The "ZIP code" colum contains lots of sings and also an exta letter attached to some of Zip codes in this colum. This function written below will strip all these out from the column.  


```python
def replace_ZIP_code_XX_USMOI(x):
    '''
    Replace XX in Zip code to 00
    '''
    try: 
        return x.replace('XX', '00').replace('(','').replace('"','').replace('-','').replace('$','').replace('.','').replace('!','').replace('+','').replace('*','').replace('`','').replace('/','').replace('UNITED STATES MINOR OUTLYING ISLANDS','USMOI')
    except AttributeError:
        return np.NaN

df['ZIP code'] = df['ZIP code'].map(replace_ZIP_code_XX_USMOI)
df["State"] = df["State"].map(replace_ZIP_code_XX_USMOI)

```

Now Let's fill in all the missing values with appropriate values. 
In "ZIP code" case, I will fill them in with "00000" and convert its type to integer. For the other cases, I will fill them in with "Not given".


```python
df['ZIP code'] = df['ZIP code'].fillna("00000").astype(int)
df['State'] = df['State'].fillna("Not given")
df['Product'] = df['Product'].fillna("Not given")
df['Sub-product'] = df['Sub-product'].fillna("Not given")
df['Issue'] = df['Issue'].fillna("Not given")
df['Sub-issue'] = df['Sub-issue'].fillna("Not given")
df['Consumer complaint narrative'] = df['Consumer complaint narrative'].fillna("Not given")
df['Company public response'] = df['Company public response'].fillna("Not given")
df['Company response to consumer'] = df['Company response to consumer'].fillna("Not given")
df['Tags'] = df['Tags'].fillna("Not given")
```


```python
#lets see How our data types look like. 
df.dtypes.to_frame(name='Type')
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
      <th>Type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Date received</th>
      <td>datetime64[ns]</td>
    </tr>
    <tr>
      <th>Product</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Sub-product</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Issue</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Sub-issue</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Consumer complaint narrative</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Company public response</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Company</th>
      <td>object</td>
    </tr>
    <tr>
      <th>State</th>
      <td>object</td>
    </tr>
    <tr>
      <th>ZIP code</th>
      <td>int64</td>
    </tr>
    <tr>
      <th>Tags</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Consumer consent provided?</th>
      <td>bool</td>
    </tr>
    <tr>
      <th>Submitted via</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Date sent to company</th>
      <td>datetime64[ns]</td>
    </tr>
    <tr>
      <th>Company response to consumer</th>
      <td>object</td>
    </tr>
    <tr>
      <th>Timely response?</th>
      <td>bool</td>
    </tr>
    <tr>
      <th>Consumer disputed?</th>
      <td>bool</td>
    </tr>
    <tr>
      <th>Complaint ID</th>
      <td>int64</td>
    </tr>
  </tbody>
</table>
</div>



Ok, now I will just save the cleaned data set into the current working directory (cwd) by the following command line. 


```python
df.to_csv("Customer_Complaints_cleaned.csv", sep='\t', encoding='utf-8') # cwd = os.getcwd()
```

## Data vizualization


```python
from IPython.display import display
pd.options.display.max_columns = None
import matplotlib.pyplot as plt
import seaborn as sns
sns.set()
import pylab as pl
from matplotlib import cm
import plotly.graph_objs as go
import plotly.offline as py
import os
import plotly.tools as tls
import plotly.figure_factory as ff
import matplotlib.ticker as mtick
```


```python
%matplotlib inline
#rcParams['figure.figsize'] = 28, 4
sb.set_style('whitegrid')
```


```python
Disputed_cunsumer_rate = (df['Consumer disputed?'].value_counts()*100.0 /len(df)).plot(kind='pie',\
        labels = ['No', 'Yes'], figsize = (6,6) , colors = ['grey','red'])

Disputed_cunsumer_rate.set_title('Consumer disputed?')
Disputed_cunsumer_rate.legend(labels=['No','Yes']);
```


![png](output_24_0.png)



```python
Timely_response_rate = (df['Timely response?'].value_counts()*100.0 /len(df)).plot(kind='pie',\
        labels = ['Yes', 'No'], figsize = (7,7) , colors = ['grey','green'])

Timely_response_rate.set_title('Timely response?')
Timely_response_rate.legend(labels=['Yes','No']);
```


![png](output_25_0.png)



```python
Consumer_consent_provided_rate = (df['Consumer consent provided?'].value_counts()*100.0 /len(df)).plot(kind='pie',\
        labels = ['Yes', 'No'], figsize = (7,7) , colors = ['grey','blue'])

Consumer_consent_provided_rate.set_title('Consumer consent provided?')
Consumer_consent_provided_rate.legend(labels=['Yes','No']);
```


![png](output_26_0.png)



```python
rcParams['figure.figsize'] = 28, 4
gb = df.groupby("State")["Submitted via"].value_counts().to_frame().rename({"Submitted via": "Number of complaints"}, axis = 1).reset_index()
sns.barplot(x = "State", y = "Number of complaints", data = gb, hue = "Submitted via", palette = sns.color_palette("hls", 8)).set_title("Number of complaints in each state based on submission method");
plt.xticks(rotation=-90);



```

    /Users/hossein/anaconda3/lib/python3.6/site-packages/seaborn/categorical.py:1508: FutureWarning:
    
    remove_na is deprecated and is a private function. Do not use.
    



![png](output_27_1.png)



```python


```


```python
submission = (df['Submitted via'].value_counts()*100.0 /len(df)).plot(kind='pie',\
        figsize = (8,8) , colors = ['grey','purple', 'orange', 'green', 'blue', 'red'], fontsize = 15)

submission.set_title('Submission method ')
submission.legend(labels=['Web', 'Referral', 'Phone', 'Postal mail', 'Fax', 'Email']);
```


![png](output_29_0.png)



```python
product_info = (df['Product'].value_counts()*100.0 /len(df2)).plot(kind='bar', stacked = True,\
                                                rot = 0, color = ['red','lightblue','green', 'blue', 'yellow'])
  
product_info.yaxis.set_major_formatter(mtick.PercentFormatter())
product_info.set_ylabel('Complaints')
product_info.set_xlabel('Product')
product_info.set_title('Percentage of complaints for each product');
plt.xticks(rotation=-90);
```


![png](output_30_0.png)



```python
df["Company public response"] = np.where(df["Company public response"] == "Not given", False, True)
```


```python

Company_public_response_rate = (df['Company public response'].value_counts()*100.0 /len(df2)).plot(kind='pie',\
        labels = ['No', 'Yes'], figsize = (7,7) , colors = ['grey','blue'])

Company_public_response_rate.set_title('Company public response provided?')
Company_public_response_rate.legend(labels=['No','Yes']);

```


![png](output_32_0.png)



```python
date_delay = (df['Date sent to company']- df['Date received']).astype('timedelta64[D]')
```


```python
rcParams['figure.figsize'] = 8, 6
plt.hist(date_delay, normed=False, bins=1000)
plt.ylabel('Number of complaints');
plt.xlabel('Time difference (days)');
plt.title('The difference between date a complaint received and the date it sent to company')
plt.xlim((0, 40))

```




    (0, 40)




![png](output_34_1.png)



```python


```
