
# Data Cleaning and Export to SQLite3
###  Using NHS England - Delayed Transfers of Care (DToCs) Datasets
---
This notebook describes and demonstrates the steps that I have taken to clean, combine and prepare a dataset ready for export into a SQLite3 Database.

This process has been undertaken using Python with the pandas and SQLite3 libraries. My approach is not Pythonic, however it does work.

The datasets I have used are from the [NHS England](https://www.england.nhs.uk/statistics/) statistical publications. The [National Health Service (England)](https://en.wikipedia.org/wiki/National_Health_Service_(England)), commonly abreviated to the NHS, is the publically funded healthcare system in England (with similar, but seperate systems for other parts of the UK). They collect large amounts of data about the provision of services across different providers (e.g. hospitals). One of these is in regards to [Delayed Transfers of Care (DToCs)](https://www.kingsfund.org.uk/topics/measurement-and-performance/delayed-transfers-care-quick-guide), often referred to in the media as bed blocking. DToCs are when an adult patient is ready for transfer or discharge, yet remain in a hospital bed. There can a range of reasons for this; for example a patient might have been treated for a fractured hip, and could be transfered into a nursing home, but there is no availabilty of beds in the nursing home for them to be transferred into. This is seen as a big challenge in the NHS, therefore the data is collected. Lets look at example of this dataset:


```python
import pandas as pd
```


```python
oct_16 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2016/06/Monthly-SITREPs-DTOC-Extracts-gf04L.csv', header=3)
oct_16.head(20)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Period Name</th>
      <th>Provider Parent Org Code</th>
      <th>Provider Parent Name</th>
      <th>Provider Org Code</th>
      <th>Provider Org Name</th>
      <th>Local Authority Code</th>
      <th>Local Authority Name</th>
      <th>Acute Or Non Acute Description</th>
      <th>Reason For Delay</th>
      <th>NHS A SUM</th>
      <th>NHS B SUM</th>
      <th>Social Care A SUM</th>
      <th>Social Care B SUM</th>
      <th>Both A SUM</th>
      <th>Both B SUM</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Non-Acute</td>
      <td>I_HOUSING</td>
      <td>0</td>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Non-Acute</td>
      <td>H_DISPUTES</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Non-Acute</td>
      <td>G_PATIENT_FAMILY_CHOICE</td>
      <td>3</td>
      <td>29</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Non-Acute</td>
      <td>F_COMMUNITY_EQUIP_ADAPT</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Non-Acute</td>
      <td>E_CARE_PACKAGE_IN_HOME</td>
      <td>6</td>
      <td>224</td>
      <td>8</td>
      <td>163</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Non-Acute</td>
      <td>DI_RESIDENTIAL_HOME</td>
      <td>1</td>
      <td>39</td>
      <td>0</td>
      <td>62</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Non-Acute</td>
      <td>DII_NURSING_HOME</td>
      <td>0</td>
      <td>21</td>
      <td>0</td>
      <td>37</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Non-Acute</td>
      <td>C_FURTHER_NON_ACUTE_NHS</td>
      <td>1</td>
      <td>24</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Non-Acute</td>
      <td>B_PUBLIC_FUNDING</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Non-Acute</td>
      <td>A_COMPLETION_ASSESSMENT</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Acute</td>
      <td>I_HOUSING</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Acute</td>
      <td>H_DISPUTES</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Acute</td>
      <td>G_PATIENT_FAMILY_CHOICE</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Acute</td>
      <td>F_COMMUNITY_EQUIP_ADAPT</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Acute</td>
      <td>E_CARE_PACKAGE_IN_HOME</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Acute</td>
      <td>DI_RESIDENTIAL_HOME</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Acute</td>
      <td>DII_NURSING_HOME</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Acute</td>
      <td>C_FURTHER_NON_ACUTE_NHS</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Acute</td>
      <td>B_PUBLIC_FUNDING</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2016-17</td>
      <td>OCTOBER</td>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Acute</td>
      <td>A_COMPLETION_ASSESSMENT</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



The aim of this exercise is to combine all the available datasets into a single dataset, to then insert into a SQLite table within a database that contains other NHS datasets.

To begin, we import all the datasets for 2016 into pandas dataframes using the `pd.read_csv` function. I have done this through a direct link to the files on the NHS England wesbite for the purposes of reproducability; note the use of the parameter `header=3` - this has been used as the csv header begins further down the page. 


```python
sep_16 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2016/06/Monthly-SITREPs-DTOC-Extracts-hfU0D.csv', header=3)
aug_16 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2016/06/Monthly-SITREPs-DTOC-Extracts-dj498.csv', header=3)
jul_16 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2016/06/Monthly-SITREPs-DTOC-Extracts-9H0jr.csv', header=3)
jun_16 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2016/06/Monthly-SITREPs-DTOC-Extracts-T25Kd.csv', header=3)
may_16 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2016/06/Monthly-SITREPs-DTOC-Extracts-kYNdw.csv', header=3)
apr_16 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2016/06/Monthly-SITREPs-DTOC-Extracts-nN06x.csv', header=3)
mar_16 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2015/05/Monthly-SITREPs-DTOC-Extracts-iFYJ2.csv', header=3)
feb_16 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2015/05/Monthly-SITREPs-DTOC-Extracts-gVela.csv', header=3)
jan_16 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2015/05/Monthly-SITREPs-DTOC-Extracts-9jMj2.csv', header=3)
```

We then combine the 2016 dataframes in a single dataframe assigned to `dtoc_16` using the `pd.concat()` function:


```python
frames = [oct_16, sep_16, jul_16, jun_16, may_16, apr_16, mar_16, feb_16, jan_16]

dtoc_16 = pd.concat(frames)
dtoc_16.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 172000 entries, 0 to 18459
    Data columns (total 16 columns):
    Year                              172000 non-null object
    Period Name                       172000 non-null object
    Provider Parent Org Code          172000 non-null object
    Provider Parent Name              172000 non-null object
    Provider Org Code                 172000 non-null object
    Provider Org Name                 172000 non-null object
    Local Authority Code              172000 non-null object
    Local Authority Name              172000 non-null object
    Acute Or Non Acute Description    172000 non-null object
    Reason For Delay                  172000 non-null object
    NHS A SUM                         171996 non-null float64
    NHS B SUM                         171995 non-null float64
    Social Care A SUM                 172000 non-null int64
    Social Care B SUM                 171998 non-null float64
    Both A SUM                        172000 non-null int64
    Both B SUM                        171998 non-null float64
    dtypes: float64(4), int64(2), object(10)
    memory usage: 22.3+ MB


Note the null values in the dataframe, for now we will ignore this and deal with it once we have combinded all the datasets.

Currently the date information is stored as strings in two different columns, with the year in the financial year format and the month as a capitalised name. It will be helpful once we have combinded all the dataframes to add a true datetime column, therefore we will do some preperation work by modifying these columns.

* Year
  * Convert to a integer value for the calender year.
* Month
  * Change the name of the 'Period Name' column to 'Month'
  * Convert the capatilised month names to an integer value.


```python
# Year
dtoc_16['Year'] = 2016

# Month
dtoc_16 = dtoc_16.rename(columns={'Period Name':'Month'})
date_str = ['JANUARY','FEBRUARY','MARCH','APRIL','MAY','JUNE','JULY','AUGUST','SEPTEMBER','OCTOBER','NOVEMBER','DECEMBER']
date_int = [1,2,3,4,5,6,7,8,9,10,11,12]
for i in range(0,12):
    dtoc_16['Month'] = dtoc_16['Month'].replace(date_str[i],date_int[i])    
```

Check the dataframe to see if the changes have been successful:


```python
dtoc_16.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 172000 entries, 0 to 18459
    Data columns (total 16 columns):
    Year                              172000 non-null int64
    Month                             172000 non-null int64
    Provider Parent Org Code          172000 non-null object
    Provider Parent Name              172000 non-null object
    Provider Org Code                 172000 non-null object
    Provider Org Name                 172000 non-null object
    Local Authority Code              172000 non-null object
    Local Authority Name              172000 non-null object
    Acute Or Non Acute Description    172000 non-null object
    Reason For Delay                  172000 non-null object
    NHS A SUM                         171996 non-null float64
    NHS B SUM                         171995 non-null float64
    Social Care A SUM                 172000 non-null int64
    Social Care B SUM                 171998 non-null float64
    Both A SUM                        172000 non-null int64
    Both B SUM                        171998 non-null float64
    dtypes: float64(4), int64(4), object(8)
    memory usage: 22.3+ MB


Repeat the above process for the 2015 datasets.


```python
dec_15 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2016/05/Monthly-SITREPs-DTOC-Extracts-6Gxr4.csv', header=3)
nov_15 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2016/05/Monthly-SITREPs-DTOC-Extracts-Ifn3m.csv', header=3)
oct_15 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2016/05/Monthly-SITREPs-DTOC-Extracts-ZPEX3.csv', header=3)
sep_15 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2016/05/Monthly-SITREPs-DTOC-Extracts-fKLcd-1.csv', header=3)
aug_15 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2016/05/Monthly-SITREPs-DTOC-Extracts-k00La.csv', header=3)
jul_15 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2016/05/Monthly-SITREPs-DTOC-Extracts-1IRZy.csv', header=3)
jun_15 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2015/05/Monthly-SITREPs-DTOC-Extracts-June-2015.csv', header=3)
may_15 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2015/05/Monthly-SITREPs-DTOC-Extracts-May-2015.csv', header=3)
apr_15 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2015/05/Monthly-SITREPs-DTOC-Extracts-April-2015.csv', header=3)
mar_15 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2014/05/Monthly-SITREPs-DTOC-Extracts-March-2015.csv', header=3)
feb_15 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2014/05/Monthly-SITREPs-DTOC-Extracts-February-2015.csv', header=3)
jan_15 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2014/05/Monthly-SITREPs-DTOC-Extracts-January-2015.csv', header=3)
```


```python
frames = [dec_15, nov_15, oct_15, sep_15, jul_15, jun_15, may_15, apr_15, mar_15, feb_15, jan_15]

dtoc_15 = pd.concat(frames)
dtoc_15.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 195620 entries, 0 to 19219
    Data columns (total 16 columns):
    Year                              195620 non-null object
    Period Name                       195620 non-null object
    Provider Parent Org Code          195620 non-null object
    Provider Parent Name              195620 non-null object
    Provider Org Code                 195620 non-null object
    Provider Org Name                 195620 non-null object
    Local Authority Code              195620 non-null object
    Local Authority Name              195620 non-null object
    Acute Or Non Acute Description    195620 non-null object
    Reason For Delay                  195620 non-null object
    NHS A SUM                         195615 non-null float64
    NHS B SUM                         195606 non-null float64
    Social Care A SUM                 195614 non-null float64
    Social Care B SUM                 195612 non-null float64
    Both A SUM                        195617 non-null float64
    Both B SUM                        195619 non-null float64
    dtypes: float64(6), object(10)
    memory usage: 25.4+ MB



```python
# Year
dtoc_15['Year'] = 2015

# Month
dtoc_15 = dtoc_15.rename(columns={'Period Name':'Month'})
for i in range(0,12):
    dtoc_15['Month'] = dtoc_15['Month'].replace(date_str[i],date_int[i])   
```

Repeat the above process for the 2014 datasets.

Note that datasets `mar_14` and `feb_14` use the parameter `header=4` rather than `3` as for these files the header starts further down.


```python
dec_14 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2014/05/Revised-CSV2.csv', header=3)
nov_14 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2014/05/Revised-CSV3.csv', header=3)
oct_14 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2014/05/Revised-CSV4.csv', header=3)
sep_14 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2014/05/Revised-CSV5.csv', header=3)
aug_14 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2014/05/Revised-CSV6.csv', header=3)
jul_14 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2014/05/Revised-CSV7.csv', header=3)
jun_14 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2014/05/Revised-CSV9.csv', header=3)
may_14 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2014/05/Revised-CSV10.csv', header=3)
apr_14 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2014/05/Revised-CSV11.csv', header=3)
mar_14 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/05/CSV.csv', header=4)
feb_14 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/05/csv.csv', header=4)
jan_14 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/05/Monthly-SITREPs-DTOC-Extracts-20tXE.csv', header=3)
```


```python
frames = [dec_14, nov_14, oct_14, sep_14, jul_14, jun_14, may_14, apr_14, mar_14, feb_14, jan_14]

dtoc_14 = pd.concat(frames)
dtoc_14.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 192500 entries, 0 to 17419
    Data columns (total 16 columns):
    Year                              192500 non-null object
    Period Name                       192500 non-null object
    Provider Parent Org Code          192500 non-null object
    Provider Parent Name              192500 non-null object
    Provider Org Code                 192500 non-null object
    Provider Org Name                 192500 non-null object
    Local Authority Code              192500 non-null object
    Local Authority Name              192500 non-null object
    Acute Or Non Acute Description    192500 non-null object
    Reason For Delay                  192500 non-null object
    NHS A SUM                         192492 non-null float64
    NHS B SUM                         192489 non-null float64
    Social Care A SUM                 192499 non-null float64
    Social Care B SUM                 192500 non-null int64
    Both A SUM                        192495 non-null float64
    Both B SUM                        192498 non-null float64
    dtypes: float64(5), int64(1), object(10)
    memory usage: 25.0+ MB



```python
# Year
dtoc_14['Year'] = 2014

# Month
dtoc_14 = dtoc_14.rename(columns={'Period Name':'Month'})
for i in range(0,12):
    dtoc_14['Month'] = dtoc_14['Month'].replace(date_str[i],date_int[i])   
```

Repeat the above process for the 2013 dataset


```python
dec_13 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/05/Monthly-SITREPs-DTOC-Extracts-xfx78q.csv', header=3)
nov_13 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/05/Monthly-SITREPs-DTOC-Extracts-75qzU.csv', header=3)
oct_13 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/05/Monthly-SITREPs-DTOC-Extracts-VBd38.csv', header=3)
sep_13 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/05/Monthly-SITREPs-DTOC-Extracts-23rBs.csv', header=3)
aug_13 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/05/Monthly-SITREPs-DTOC-Extracts-msdi2.csv', header=3)
jul_13 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/05/Monthly-SITREPs-DTOC-Extracts-K2kDF.csv', header=3)
jun_13 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/05/Monthly-SITREPs-DTOC-Extracts-8s0Pc.csv', header=3)
may_13 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/05/Monthly-SITREPs-DTOC-Extracts-1qPs8.csv', header=3)
apr_13 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/05/Monthly-SITREPs-DTOC-Extracts-Sy45S.csv', header=3)
mar_13 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/04/Monthly-SITREPs-DTOC-Extracts9GT7D.csv', header=3)
feb_13 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/04/Monthly-SITREPs-DTOC-Extracts3KH5E.csv', header=3)
jan_13 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/04/Monthly-SITREPs-DTOC-Extracts6TG8D.csv', header=3)
```


```python
frames = [dec_13, nov_13, oct_13, sep_13, jul_13, jun_13, may_13, apr_13, mar_13, feb_13, jan_13]

dtoc_13 = pd.concat(frames)
dtoc_13.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 185700 entries, 0 to 17179
    Data columns (total 16 columns):
    Year                              185700 non-null object
    Period Name                       185700 non-null object
    Provider Parent Org Code          185700 non-null object
    Provider Parent Name              185700 non-null object
    Provider Org Code                 185700 non-null object
    Provider Org Name                 185700 non-null object
    Local Authority Code              185700 non-null object
    Local Authority Name              185700 non-null object
    Acute Or Non Acute Description    185700 non-null object
    Reason For Delay                  185700 non-null object
    NHS A SUM                         185689 non-null float64
    NHS B SUM                         185691 non-null float64
    Social Care A SUM                 185696 non-null float64
    Social Care B SUM                 185690 non-null float64
    Both A SUM                        185700 non-null int64
    Both B SUM                        185700 non-null int64
    dtypes: float64(4), int64(2), object(10)
    memory usage: 24.1+ MB



```python
# Year
dtoc_13['Year'] = 2013

# Month
dtoc_13 = dtoc_13.rename(columns={'Period Name':'Month'})
for i in range(0,12):
    dtoc_13['Month'] = dtoc_13['Month'].replace(date_str[i],date_int[i])   
```

Repeat the above process for the 2012 dataset.


```python
dec_12 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/04/Monthly-SITREPs-DTOC-Extracts-December-2012-rev-April-2013.csv', header=3)
nov_12 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/04/Monthly-SITREPs-DTOC-Extracts-November-2012-rev-April-2013.csv', header=3)
oct_12 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/04/Monthly-SITREPs-DTOC-Extracts-October-2012-rev-April-2013.csv', header=3)
sep_12 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/04/Monthly-SITREPs-DTOC-Extracts-September-2012-rev-April-2013.csv', header=3)
aug_12 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/04/Monthly-SITREPs-Extracts-August-2012-rev-April-2013.csv', header=3)
jul_12 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/04/Monthly-SITREPs-DTOC-Extracts-July-2012-rev-April-2013.csv', header=3)
jun_12 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/04/Monthly-SITREPs-DTOC-Extracts-June-2012-rev-April-2013.csv', header=3)
may_12 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/04/Monthly-SITREPs-DTOC-Extracts-May-2012-rev-April-2013.csv', header=3)
apr_12 = pd.read_csv('https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2013/04/Monthly-SITREPs-DTOC-Extracts-April-2012-rev-April-2013.csv', header=3)
mar_12 = pd.read_csv('http://transparency.dh.gov.uk/files/2012/07/Monthly-SITREPs-DTOC-Extracts-March-2011-12-revised-October-2012.csv', header=3)
feb_12 = pd.read_csv('http://transparency.dh.gov.uk/files/2012/07/Monthly-SITREPs-DTOC-Extracts-February-2011-12-revised-October-2012.csv', header=3)
jan_12 = pd.read_csv('http://transparency.dh.gov.uk/files/2012/07/Monthly-SITREPs-DTOC-Extracts-January-2011-12-revised-October-2012.csv', header=3)
```


```python
frames = [dec_12, nov_12, oct_12, sep_12, jul_12, jun_12, may_12, apr_12, mar_12, feb_12, jan_12]

dtoc_12 = pd.concat(frames)
dtoc_12.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 176060 entries, 0 to 15919
    Data columns (total 16 columns):
    Year                              176060 non-null object
    Period Name                       176060 non-null object
    Provider SHA Code                 176060 non-null object
    Provider SHA Name                 176060 non-null object
    Provider Org Code                 176060 non-null object
    Provider Org Name                 176060 non-null object
    Local Authority Code              176060 non-null object
    Local Authority Name              176060 non-null object
    Acute Or Non Acute Description    176060 non-null object
    Reason For Delay                  176060 non-null object
    NHS A SUM                         176051 non-null float64
    NHS B SUM                         176044 non-null float64
    Social Care A SUM                 176052 non-null float64
    Social Care B SUM                 176048 non-null float64
    Both A SUM                        176060 non-null int64
    Both B SUM                        176059 non-null float64
    dtypes: float64(5), int64(1), object(10)
    memory usage: 22.8+ MB



```python
# Year
dtoc_12['Year'] = 2012

# Month
dtoc_12 = dtoc_12.rename(columns={'Period Name':'Month'})
for i in range(0,12):
    dtoc_12['Month'] = dtoc_12['Month'].replace(date_str[i],date_int[i])   
```

Repeat the above process for the 2011 dataset.


```python
dec_11 = pd.read_csv('http://webarchive.nationalarchives.gov.uk/20130227100129/http://www.dh.gov.uk/prod_consum_dh/groups/dh_digitalassets/@dh/@en/@ps/@sta/@perf/documents/digitalasset/dh_133801.csv', header=3)
nov_11 = pd.read_csv('http://webarchive.nationalarchives.gov.uk/20130227100129/http://www.dh.gov.uk/prod_consum_dh/groups/dh_digitalassets/@dh/@en/@ps/@sta/@perf/documents/digitalasset/dh_133800.csv', header=3)
oct_11 = pd.read_csv('http://webarchive.nationalarchives.gov.uk/20130227100129/http://www.dh.gov.uk/prod_consum_dh/groups/dh_digitalassets/@dh/@en/@ps/@sta/@perf/documents/digitalasset/dh_133799.csv', header=3)
sep_11 = pd.read_csv('http://webarchive.nationalarchives.gov.uk/20130227100129/http://www.dh.gov.uk/prod_consum_dh/groups/dh_digitalassets/@dh/@en/@ps/@sta/@perf/documents/digitalasset/dh_133798.csv', header=3)
aug_11 = pd.read_csv('http://webarchive.nationalarchives.gov.uk/20130227100129/http://www.dh.gov.uk/prod_consum_dh/groups/dh_digitalassets/@dh/@en/@ps/@sta/@perf/documents/digitalasset/dh_133797.csv', header=3)
jul_11 = pd.read_csv('http://webarchive.nationalarchives.gov.uk/20130227100129/http://www.dh.gov.uk/prod_consum_dh/groups/dh_digitalassets/@dh/@en/@ps/@sta/@perf/documents/digitalasset/dh_133796.csv', header=3)
jun_11 = pd.read_csv('http://webarchive.nationalarchives.gov.uk/20130227100129/http://www.dh.gov.uk/prod_consum_dh/groups/dh_digitalassets/@dh/@en/@ps/@sta/@perf/documents/digitalasset/dh_133795.csv', header=3)
may_11 = pd.read_csv('http://webarchive.nationalarchives.gov.uk/20130227100129/http://www.dh.gov.uk/prod_consum_dh/groups/dh_digitalassets/@dh/@en/@ps/@sta/@perf/documents/digitalasset/dh_133794.csv', header=3)
apr_11 = pd.read_csv('http://webarchive.nationalarchives.gov.uk/20130227100129/http://www.dh.gov.uk/prod_consum_dh/groups/dh_digitalassets/@dh/@en/@ps/@sta/@perf/documents/digitalasset/dh_133793.csv', header=3)
```


```python
frames = [dec_11, nov_11, oct_11, sep_11, jul_11, jun_11, may_11, apr_11]

dtoc_11 = pd.concat(frames)
dtoc_11.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 129560 entries, 0 to 16219
    Data columns (total 16 columns):
    Year                              129560 non-null object
    Period Name                       129560 non-null object
    Provider SHA Code                 129560 non-null object
    Provider SHA Name                 129560 non-null object
    Provider Org Code                 129560 non-null object
    Provider Org Name                 129560 non-null object
    Local Authority Code              129560 non-null object
    Local Authority Name              129560 non-null object
    Acute Or Non Acute Description    129560 non-null object
    Reason For Delay                  129560 non-null object
    NHS A SUM                         129547 non-null float64
    NHS B SUM                         129534 non-null float64
    Social Care A SUM                 129541 non-null float64
    Social Care B SUM                 129528 non-null float64
    Both A SUM                        129560 non-null int64
    Both B SUM                        129560 non-null int64
    dtypes: float64(4), int64(2), object(10)
    memory usage: 16.8+ MB



```python
# Year
dtoc_11['Year'] = 2011

# Month
dtoc_11 = dtoc_11.rename(columns={'Period Name':'Month'})
for i in range(0,12):
    dtoc_11['Month'] = dtoc_11['Month'].replace(date_str[i],date_int[i])   
```

Before combining all the datasets into a single dataframe, we need to rename two columns in `dtoc_11` and `dtoc_12` as they use they are named for the old Strategic Health Autorities (SHA). 

In this situation, these can be renamed to match the `Provider Parent Org Code` and `Name` as per the other dataframes, as the codes used are similar. 


```python
# dtoc_11 and dtoc_12, have different column names, therefore correct for this
dtoc_11 = dtoc_11.rename(columns={'Provider SHA Code':'Provider Parent Org Code', 'Provider SHA Name':'Provider Parent Name'})
dtoc_12 = dtoc_12.rename(columns={'Provider SHA Code':'Provider Parent Org Code', 'Provider SHA Name':'Provider Parent Name'})

print('dtoc_11\n---')
dtoc_11.info()
print('\ndtoc_12\n---')
dtoc_12.info()
```

    dtoc_11
    ---
    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 129560 entries, 0 to 16219
    Data columns (total 16 columns):
    Year                              129560 non-null int64
    Month                             129560 non-null int64
    Provider Parent Org Code          129560 non-null object
    Provider Parent Name              129560 non-null object
    Provider Org Code                 129560 non-null object
    Provider Org Name                 129560 non-null object
    Local Authority Code              129560 non-null object
    Local Authority Name              129560 non-null object
    Acute Or Non Acute Description    129560 non-null object
    Reason For Delay                  129560 non-null object
    NHS A SUM                         129547 non-null float64
    NHS B SUM                         129534 non-null float64
    Social Care A SUM                 129541 non-null float64
    Social Care B SUM                 129528 non-null float64
    Both A SUM                        129560 non-null int64
    Both B SUM                        129560 non-null int64
    dtypes: float64(4), int64(4), object(8)
    memory usage: 16.8+ MB
    
    dtoc_12
    ---
    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 176060 entries, 0 to 15919
    Data columns (total 16 columns):
    Year                              176060 non-null int64
    Month                             176060 non-null int64
    Provider Parent Org Code          176060 non-null object
    Provider Parent Name              176060 non-null object
    Provider Org Code                 176060 non-null object
    Provider Org Name                 176060 non-null object
    Local Authority Code              176060 non-null object
    Local Authority Name              176060 non-null object
    Acute Or Non Acute Description    176060 non-null object
    Reason For Delay                  176060 non-null object
    NHS A SUM                         176051 non-null float64
    NHS B SUM                         176044 non-null float64
    Social Care A SUM                 176052 non-null float64
    Social Care B SUM                 176048 non-null float64
    Both A SUM                        176060 non-null int64
    Both B SUM                        176059 non-null float64
    dtypes: float64(5), int64(3), object(8)
    memory usage: 22.8+ MB


Combine all of these dataframes into a single dataframe, ready for upload to SQLite Database.


```python
frames = [dtoc_16, dtoc_15, dtoc_14, dtoc_13, dtoc_12]
dtoc = pd.concat(frames)

dtoc.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 921880 entries, 0 to 15919
    Data columns (total 16 columns):
    Year                              921880 non-null int64
    Month                             921880 non-null int64
    Provider Parent Org Code          921880 non-null object
    Provider Parent Name              921880 non-null object
    Provider Org Code                 921880 non-null object
    Provider Org Name                 921880 non-null object
    Local Authority Code              921880 non-null object
    Local Authority Name              921880 non-null object
    Acute Or Non Acute Description    921880 non-null object
    Reason For Delay                  921880 non-null object
    NHS A SUM                         921843 non-null float64
    NHS B SUM                         921825 non-null float64
    Social Care A SUM                 921861 non-null float64
    Social Care B SUM                 921848 non-null float64
    Both A SUM                        921872 non-null float64
    Both B SUM                        921874 non-null float64
    dtypes: float64(6), int64(2), object(8)
    memory usage: 119.6+ MB


As noted above, we have a number of null values. There are two approches to dealing with this problem:
* Drop the observation (row) entirely
* Replace the null with a value

In this case as we are dealing with numeric (float) values, therefore we are going to replace the null value with 0. So that other values in that observation are not lost. This is achieved using the `df.fillna()` function:


```python
dtoc = dtoc.fillna(0)
```

Finally we are going to add the datetime column. 

We are going to make a pragmatic decision about the choice of date for each observation. If we look at the [definitions and guidance document](https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/2015/10/mnth-Sitreps-def-dtoc-v1.09.pdf) for this dataset; it shows that "NHS, Social Care and Both A SUM" are the number of patients classed as being in DToC on the **last Thursday of every month**. Whilst "NHS, Social Care and Both B SUM" are the total number of DToC days for all patients **during the entire month**.

Therefore we are going to make a pragmatic decision to assign the day to the 1st of every month. Whilst this is technically incorrect, it won't impact on our future analysis purposes. Therefore we create a column called `Day`, which `=1` for all rows.

We then combine the `Year`, `Month` and `Day` column into a temporary column called `Period`. We can then use `pd.to_datetime(dtoc['Period'])` assigned to a new column called `Date`.

Finally we can drop `Year`, `Month`, `Day` and `Period` columns, as we no longer require these as they have been replaced by the `Date` column.


```python
# Day
dtoc['Day'] = 1

# Date
dtoc['Period'] = dtoc['Year'].map(str) + '-' + dtoc['Month'].map(str) + '-' + dtoc['Day'].map(str)
dtoc['Date'] = pd.to_datetime(dtoc['Period'])
dtoc = dtoc.drop(['Period','Year','Month','Day'], axis=1)

dtoc.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 921880 entries, 0 to 15919
    Data columns (total 15 columns):
    Provider Parent Org Code          921880 non-null object
    Provider Parent Name              921880 non-null object
    Provider Org Code                 921880 non-null object
    Provider Org Name                 921880 non-null object
    Local Authority Code              921880 non-null object
    Local Authority Name              921880 non-null object
    Acute Or Non Acute Description    921880 non-null object
    Reason For Delay                  921880 non-null object
    NHS A SUM                         921880 non-null float64
    NHS B SUM                         921880 non-null float64
    Social Care A SUM                 921880 non-null float64
    Social Care B SUM                 921880 non-null float64
    Both A SUM                        921880 non-null float64
    Both B SUM                        921880 non-null float64
    Date                              921880 non-null datetime64[ns]
    dtypes: datetime64[ns](1), float64(6), object(8)
    memory usage: 112.5+ MB



```python
dtoc.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Provider Parent Org Code</th>
      <th>Provider Parent Name</th>
      <th>Provider Org Code</th>
      <th>Provider Org Name</th>
      <th>Local Authority Code</th>
      <th>Local Authority Name</th>
      <th>Acute Or Non Acute Description</th>
      <th>Reason For Delay</th>
      <th>NHS A SUM</th>
      <th>NHS B SUM</th>
      <th>Social Care A SUM</th>
      <th>Social Care B SUM</th>
      <th>Both A SUM</th>
      <th>Both B SUM</th>
      <th>Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Non-Acute</td>
      <td>I_HOUSING</td>
      <td>0.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2016-10-01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Non-Acute</td>
      <td>H_DISPUTES</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2016-10-01</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Non-Acute</td>
      <td>G_PATIENT_FAMILY_CHOICE</td>
      <td>3.0</td>
      <td>29.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2016-10-01</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Non-Acute</td>
      <td>F_COMMUNITY_EQUIP_ADAPT</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2016-10-01</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Q82</td>
      <td>NHS ENGLAND SOUTH (SOUTH CENTRAL)</td>
      <td>AXG</td>
      <td>WILTSHIRE HEALTH &amp; CARE LLP</td>
      <td>817</td>
      <td>WILTSHIRE</td>
      <td>Non-Acute</td>
      <td>E_CARE_PACKAGE_IN_HOME</td>
      <td>6.0</td>
      <td>224.0</td>
      <td>8.0</td>
      <td>163.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2016-10-01</td>
    </tr>
  </tbody>
</table>
</div>



The dataframe is almost ready to be exported to SQLite, however currently the column names are inappropriate and don't match [SQL naming conventions](https://launchbylunch.com/posts/2014/Feb/16/sql-naming-conventions/). Therefore we are going to rename all the columns to something more appropriate for a SQL database.


```python
dtoc = dtoc.rename(columns={
        'Provider Parent Org Code':'area_team_code',
        'Provider Parent Name':'area_team_name',
        'Provider Org Code':'provider_code',
        'Provider Org Name':'provider_name',
        'Local Authority Code':'la_code',
        'Local Authority Name':'la_name',
        'Acute Or Non Acute Description':'acute_nonacute',
        'Reason For Delay':'reason',
        'NHS A SUM':'nhs_a','NHS B SUM':'nhs_b',
        'Social Care A SUM':'socialcare_a',
        'Social Care B SUM':'socialcare_b',
        'Both A SUM':'both_a',
        'Both B SUM':'both_b',
        'Date':'date'
    })
```


```python
dtoc.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 921880 entries, 0 to 15919
    Data columns (total 15 columns):
    area_team_code    921880 non-null object
    area_team_name    921880 non-null object
    provider_code     921880 non-null object
    provider_name     921880 non-null object
    la_code           921880 non-null object
    la_name           921880 non-null object
    acute_nonacute    921880 non-null object
    reason            921880 non-null object
    nhs_a             921880 non-null float64
    nhs_b             921880 non-null float64
    socialcare_a              921880 non-null float64
    socialcare_b              921880 non-null float64
    both_a            921880 non-null float64
    both_b            921880 non-null float64
    date              921880 non-null datetime64[ns]
    dtypes: datetime64[ns](1), float64(6), object(8)
    memory usage: 112.5+ MB


Finally we create a connection to the SQLite database `nhsdatasets.db`; and use the `df.to_sql()` funtion to insert the `dtoc` dataframe into the table also titled  `dtoc`.


```python
import sqlite3

conn = sqlite3.connect('nhsdatasets.db')
```


```python
dtoc.to_sql('dtoc', con=conn, if_exists='replace')
```

This successfully inserts the data into a SQLite table in my nhsdatasets database, as demonstrated here in SQLiteStudio:

![alt text](http://www.richardonhealth.com/wp-content/uploads/2017/01/Database-Example-1024x497.png "Example from SQLiteStudio")

Finally, we need to close the connection to the database.


```python
conn.close()
```

---
Guide by Richard Betteridge -- [@rlionheart92](https://www.twitter.com/rlionheart92) -- [richardonhealth.com](http://www.richardonhealth.com) 
