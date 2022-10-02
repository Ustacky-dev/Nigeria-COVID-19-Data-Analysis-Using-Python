### Task 1 - Data Collection
Here you will obtain the required data for the analysis. As described in the project instructions, you will perform a web scrap to obtain data from the NCDC website, import data from the John Hopkins repository, and import the provided external data.


Data Collection from the Nigerian Center for Disease Control

### A - NCDC Website scrap
Website - https://covid19.ncdc.gov.ng/


```python
# Write Your Code Below
# Import all libraries in this cell
import requests
import numpy as np
import urllib.request
import pandas as pd
import csv
from bs4 import BeautifulSoup
import seaborn as sns
sns.set_style("darkgrid")
import matplotlib.pyplot as plt
%matplotlib inline
plt.style.use('fivethirtyeight')  
import warnings
warnings.filterwarnings('ignore')
import time
```


```python
# Beautiful Soup intialization of url data to a DataFrame object.
start = time.time()
page_content = requests.get('https://covid19.ncdc.gov.ng/')
soup = BeautifulSoup(page_content.content, 'html.parser')
table = soup.find('table')
table_headers, table_rows = [], []
for i in table.thead.find_all('th'):
    title = i.text.strip()
    table_headers.append(title)
for j in table.tbody.find_all('tr'):
    for k in j.find_all('td'):
     table_rows.append(k.text.strip())
table_rows = np.array(table_rows).reshape(37, 5)
ncdc_data_from_soup = pd.DataFrame(table_rows, columns = table_headers)
print('Runtime speed for Beautiful Soup parsing: ', time.time()- start)
```

    Runtime speed for Beautiful Soup parsing:  3.0302469730377197
    


```python
""" start = time.time()
ncdc_data_from_pandas = pd.read_html('https://covid19.ncdc.gov.ng/')[0]
print('Runtime speed for Pandas parsing: ', time.time() - start)"""
```




    " start = time.time()\nncdc_data_from_pandas = pd.read_html('https://covid19.ncdc.gov.ng/')[0]\nprint('Runtime speed for Pandas parsing: ', time.time() - start)"




```python
#for Web Scrapping, the Beautiful Soup parsing is suitable given its functionality in Pandas
ncdc_data = ncdc_data_from_soup
```

### B - John Hopkins Data Repository
Here you will obtain data from the John Hopkins repository. Your task here involves saving the data from the GitHub repo link to DataFrame for further analysis. Find the links below. 
* Global Daily Confirmed Cases - Click [Here](https://github.com/CSSEGISandData/COVID-19/blob/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv)
* Global Daily Recovered Cases - Click [Here](https://github.com/CSSEGISandData/COVID-19/blob/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_recovered_global.csv)
* Global Daily Death Cases - Click [Here](https://github.com/CSSEGISandData/COVID-19/blob/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_deaths_global.csv)


```python
daily_confirmed = pd.read_csv(r"https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv")
daily_recovered = pd.read_csv(r"https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_recovered_global.csv")
daily_death     = pd.read_csv(r"https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_deaths_global.csv")

```

### C - External Data 
* Save the external data to a DataFrame
* External Data includes but not limited to: `covid_external.csv`, `Budget data.csv`, `RealGDP.csv`


```python
vulner_index = pd.read_csv(r"C:\Users\User\Desktop\covid_external.csv")
budget_data = pd.read_csv(r"C:\Users\User\Desktop\Budget data.csv")
gdp_data = pd.read_csv(r"C:\Users\User\Desktop\RealGDP.csv")
```

### Task 2 - View the data
Obtain basic information about the data using the `head()` and `info()` method.


```python
print(ncdc_data.info())
ncdc_data.head(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 37 entries, 0 to 36
    Data columns (total 5 columns):
     #   Column                        Non-Null Count  Dtype 
    ---  ------                        --------------  ----- 
     0   States Affected               37 non-null     object
     1   No. of Cases (Lab Confirmed)  37 non-null     object
     2   No. of Cases (on admission)   37 non-null     object
     3   No. Discharged                37 non-null     object
     4   No. of Deaths                 37 non-null     object
    dtypes: object(5)
    memory usage: 1.6+ KB
    None
    




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
      <th>States Affected</th>
      <th>No. of Cases (Lab Confirmed)</th>
      <th>No. of Cases (on admission)</th>
      <th>No. Discharged</th>
      <th>No. of Deaths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lagos</td>
      <td>103,835</td>
      <td>692</td>
      <td>102,372</td>
      <td>771</td>
    </tr>
    <tr>
      <th>1</th>
      <td>FCT</td>
      <td>29,360</td>
      <td>220</td>
      <td>28,891</td>
      <td>249</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Rivers</td>
      <td>17,976</td>
      <td>180</td>
      <td>17,641</td>
      <td>155</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kaduna</td>
      <td>11,553</td>
      <td>12</td>
      <td>11,452</td>
      <td>89</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Oyo</td>
      <td>10,329</td>
      <td>0</td>
      <td>10,127</td>
      <td>202</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Plateau</td>
      <td>10,326</td>
      <td>9</td>
      <td>10,242</td>
      <td>75</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Edo</td>
      <td>7,914</td>
      <td>31</td>
      <td>7,561</td>
      <td>322</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Ogun</td>
      <td>5,810</td>
      <td>11</td>
      <td>5,717</td>
      <td>82</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Delta</td>
      <td>5,707</td>
      <td>425</td>
      <td>5,170</td>
      <td>112</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Kano</td>
      <td>5,337</td>
      <td>170</td>
      <td>5,040</td>
      <td>127</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(daily_confirmed.info())
daily_confirmed.head(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 289 entries, 0 to 288
    Columns: 978 entries, Province/State to 9/21/22
    dtypes: float64(2), int64(974), object(2)
    memory usage: 2.2+ MB
    None
    




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
      <th>Province/State</th>
      <th>Country/Region</th>
      <th>Lat</th>
      <th>Long</th>
      <th>1/22/20</th>
      <th>1/23/20</th>
      <th>1/24/20</th>
      <th>1/25/20</th>
      <th>1/26/20</th>
      <th>1/27/20</th>
      <th>...</th>
      <th>9/12/22</th>
      <th>9/13/22</th>
      <th>9/14/22</th>
      <th>9/15/22</th>
      <th>9/16/22</th>
      <th>9/17/22</th>
      <th>9/18/22</th>
      <th>9/19/22</th>
      <th>9/20/22</th>
      <th>9/21/22</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>Afghanistan</td>
      <td>33.93911</td>
      <td>67.709953</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>196182</td>
      <td>196404</td>
      <td>196751</td>
      <td>196870</td>
      <td>196992</td>
      <td>197066</td>
      <td>197240</td>
      <td>197434</td>
      <td>197608</td>
      <td>197788</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>Albania</td>
      <td>41.15330</td>
      <td>20.168300</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>331053</td>
      <td>331191</td>
      <td>331295</td>
      <td>331384</td>
      <td>331459</td>
      <td>331540</td>
      <td>331583</td>
      <td>331601</td>
      <td>331715</td>
      <td>331810</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>Algeria</td>
      <td>28.03390</td>
      <td>1.659600</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>270551</td>
      <td>270551</td>
      <td>270570</td>
      <td>270584</td>
      <td>270599</td>
      <td>270606</td>
      <td>270609</td>
      <td>270612</td>
      <td>270612</td>
      <td>270619</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>Andorra</td>
      <td>42.50630</td>
      <td>1.521800</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>46113</td>
      <td>46113</td>
      <td>46147</td>
      <td>46147</td>
      <td>46147</td>
      <td>46147</td>
      <td>46147</td>
      <td>46147</td>
      <td>46147</td>
      <td>46147</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>Angola</td>
      <td>-11.20270</td>
      <td>17.873900</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>103131</td>
      <td>103131</td>
      <td>103131</td>
      <td>103131</td>
      <td>103131</td>
      <td>103131</td>
      <td>103131</td>
      <td>103131</td>
      <td>103131</td>
      <td>103131</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>Antarctica</td>
      <td>-71.94990</td>
      <td>23.347000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>11</td>
      <td>11</td>
      <td>11</td>
      <td>11</td>
      <td>11</td>
      <td>11</td>
      <td>11</td>
      <td>11</td>
      <td>11</td>
      <td>11</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>Antigua and Barbuda</td>
      <td>17.06080</td>
      <td>-61.796400</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>8974</td>
      <td>8974</td>
      <td>9008</td>
      <td>9008</td>
      <td>9008</td>
      <td>9008</td>
      <td>9008</td>
      <td>9008</td>
      <td>9008</td>
      <td>9008</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NaN</td>
      <td>Argentina</td>
      <td>-38.41610</td>
      <td>-63.616700</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>9697763</td>
      <td>9697763</td>
      <td>9697763</td>
      <td>9697763</td>
      <td>9697763</td>
      <td>9697763</td>
      <td>9703938</td>
      <td>9703938</td>
      <td>9703938</td>
      <td>9703938</td>
    </tr>
    <tr>
      <th>8</th>
      <td>NaN</td>
      <td>Armenia</td>
      <td>40.06910</td>
      <td>45.038200</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>439302</td>
      <td>439302</td>
      <td>439302</td>
      <td>439302</td>
      <td>439302</td>
      <td>439302</td>
      <td>439302</td>
      <td>441444</td>
      <td>441444</td>
      <td>441444</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Australian Capital Territory</td>
      <td>Australia</td>
      <td>-35.47350</td>
      <td>149.012400</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>203680</td>
      <td>203680</td>
      <td>203680</td>
      <td>204471</td>
      <td>204397</td>
      <td>204397</td>
      <td>204397</td>
      <td>204397</td>
      <td>204397</td>
      <td>204397</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 978 columns</p>
</div>




```python
print(daily_confirmed.info())
daily_confirmed.tail(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 289 entries, 0 to 288
    Columns: 978 entries, Province/State to 9/21/22
    dtypes: float64(2), int64(974), object(2)
    memory usage: 2.2+ MB
    None
    




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
      <th>Province/State</th>
      <th>Country/Region</th>
      <th>Lat</th>
      <th>Long</th>
      <th>1/22/20</th>
      <th>1/23/20</th>
      <th>1/24/20</th>
      <th>1/25/20</th>
      <th>1/26/20</th>
      <th>1/27/20</th>
      <th>...</th>
      <th>9/12/22</th>
      <th>9/13/22</th>
      <th>9/14/22</th>
      <th>9/15/22</th>
      <th>9/16/22</th>
      <th>9/17/22</th>
      <th>9/18/22</th>
      <th>9/19/22</th>
      <th>9/20/22</th>
      <th>9/21/22</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>279</th>
      <td>NaN</td>
      <td>Uruguay</td>
      <td>-32.522800</td>
      <td>-55.765800</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>982846</td>
      <td>982846</td>
      <td>982846</td>
      <td>982846</td>
      <td>982846</td>
      <td>982846</td>
      <td>982846</td>
      <td>984152</td>
      <td>984152</td>
      <td>984152</td>
    </tr>
    <tr>
      <th>280</th>
      <td>NaN</td>
      <td>Uzbekistan</td>
      <td>41.377491</td>
      <td>64.585262</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>243970</td>
      <td>243981</td>
      <td>243993</td>
      <td>244006</td>
      <td>244023</td>
      <td>244023</td>
      <td>244023</td>
      <td>244060</td>
      <td>244072</td>
      <td>244084</td>
    </tr>
    <tr>
      <th>281</th>
      <td>NaN</td>
      <td>Vanuatu</td>
      <td>-15.376700</td>
      <td>166.959200</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>11921</td>
      <td>11921</td>
      <td>11930</td>
      <td>11930</td>
      <td>11937</td>
      <td>11937</td>
      <td>11937</td>
      <td>11950</td>
      <td>11950</td>
      <td>11950</td>
    </tr>
    <tr>
      <th>282</th>
      <td>NaN</td>
      <td>Venezuela</td>
      <td>6.423800</td>
      <td>-66.589700</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>543832</td>
      <td>543832</td>
      <td>543930</td>
      <td>544057</td>
      <td>544090</td>
      <td>544090</td>
      <td>544210</td>
      <td>544248</td>
      <td>544270</td>
      <td>544310</td>
    </tr>
    <tr>
      <th>283</th>
      <td>NaN</td>
      <td>Vietnam</td>
      <td>14.058324</td>
      <td>108.277199</td>
      <td>0</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>...</td>
      <td>11441626</td>
      <td>11444927</td>
      <td>11448034</td>
      <td>11450999</td>
      <td>11454079</td>
      <td>11456558</td>
      <td>11458449</td>
      <td>11460227</td>
      <td>11463404</td>
      <td>11465691</td>
    </tr>
    <tr>
      <th>284</th>
      <td>NaN</td>
      <td>West Bank and Gaza</td>
      <td>31.952200</td>
      <td>35.233200</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>702591</td>
      <td>702591</td>
      <td>702591</td>
      <td>702768</td>
      <td>702768</td>
      <td>702768</td>
      <td>702768</td>
      <td>702768</td>
      <td>702768</td>
      <td>702768</td>
    </tr>
    <tr>
      <th>285</th>
      <td>NaN</td>
      <td>Winter Olympics 2022</td>
      <td>39.904200</td>
      <td>116.407400</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>535</td>
      <td>535</td>
      <td>535</td>
      <td>535</td>
      <td>535</td>
      <td>535</td>
      <td>535</td>
      <td>535</td>
      <td>535</td>
      <td>535</td>
    </tr>
    <tr>
      <th>286</th>
      <td>NaN</td>
      <td>Yemen</td>
      <td>15.552727</td>
      <td>48.516388</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>11932</td>
      <td>11932</td>
      <td>11932</td>
      <td>11932</td>
      <td>11932</td>
      <td>11932</td>
      <td>11932</td>
      <td>11932</td>
      <td>11932</td>
      <td>11932</td>
    </tr>
    <tr>
      <th>287</th>
      <td>NaN</td>
      <td>Zambia</td>
      <td>-13.133897</td>
      <td>27.849332</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>333234</td>
      <td>333274</td>
      <td>333313</td>
      <td>333337</td>
      <td>333363</td>
      <td>333375</td>
      <td>333382</td>
      <td>333387</td>
      <td>333387</td>
      <td>333439</td>
    </tr>
    <tr>
      <th>288</th>
      <td>NaN</td>
      <td>Zimbabwe</td>
      <td>-19.015438</td>
      <td>29.154857</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>256888</td>
      <td>256904</td>
      <td>256939</td>
      <td>256939</td>
      <td>256939</td>
      <td>256988</td>
      <td>256996</td>
      <td>257090</td>
      <td>257156</td>
      <td>257156</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 978 columns</p>
</div>




```python
print(daily_recovered.info())
daily_recovered.head(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 274 entries, 0 to 273
    Columns: 978 entries, Province/State to 9/21/22
    dtypes: float64(2), int64(974), object(2)
    memory usage: 2.0+ MB
    None
    




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
      <th>Province/State</th>
      <th>Country/Region</th>
      <th>Lat</th>
      <th>Long</th>
      <th>1/22/20</th>
      <th>1/23/20</th>
      <th>1/24/20</th>
      <th>1/25/20</th>
      <th>1/26/20</th>
      <th>1/27/20</th>
      <th>...</th>
      <th>9/12/22</th>
      <th>9/13/22</th>
      <th>9/14/22</th>
      <th>9/15/22</th>
      <th>9/16/22</th>
      <th>9/17/22</th>
      <th>9/18/22</th>
      <th>9/19/22</th>
      <th>9/20/22</th>
      <th>9/21/22</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>Afghanistan</td>
      <td>33.93911</td>
      <td>67.709953</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>Albania</td>
      <td>41.15330</td>
      <td>20.168300</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>Algeria</td>
      <td>28.03390</td>
      <td>1.659600</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>Andorra</td>
      <td>42.50630</td>
      <td>1.521800</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>Angola</td>
      <td>-11.20270</td>
      <td>17.873900</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>Antarctica</td>
      <td>-71.94990</td>
      <td>23.347000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>Antigua and Barbuda</td>
      <td>17.06080</td>
      <td>-61.796400</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NaN</td>
      <td>Argentina</td>
      <td>-38.41610</td>
      <td>-63.616700</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>NaN</td>
      <td>Armenia</td>
      <td>40.06910</td>
      <td>45.038200</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Australian Capital Territory</td>
      <td>Australia</td>
      <td>-35.47350</td>
      <td>149.012400</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 978 columns</p>
</div>




```python
print(daily_recovered.info())
daily_recovered.tail(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 274 entries, 0 to 273
    Columns: 978 entries, Province/State to 9/21/22
    dtypes: float64(2), int64(974), object(2)
    memory usage: 2.0+ MB
    None
    




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
      <th>Province/State</th>
      <th>Country/Region</th>
      <th>Lat</th>
      <th>Long</th>
      <th>1/22/20</th>
      <th>1/23/20</th>
      <th>1/24/20</th>
      <th>1/25/20</th>
      <th>1/26/20</th>
      <th>1/27/20</th>
      <th>...</th>
      <th>9/12/22</th>
      <th>9/13/22</th>
      <th>9/14/22</th>
      <th>9/15/22</th>
      <th>9/16/22</th>
      <th>9/17/22</th>
      <th>9/18/22</th>
      <th>9/19/22</th>
      <th>9/20/22</th>
      <th>9/21/22</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>264</th>
      <td>NaN</td>
      <td>Uruguay</td>
      <td>-32.522800</td>
      <td>-55.765800</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>265</th>
      <td>NaN</td>
      <td>Uzbekistan</td>
      <td>41.377491</td>
      <td>64.585262</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>266</th>
      <td>NaN</td>
      <td>Vanuatu</td>
      <td>-15.376700</td>
      <td>166.959200</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>267</th>
      <td>NaN</td>
      <td>Venezuela</td>
      <td>6.423800</td>
      <td>-66.589700</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>268</th>
      <td>NaN</td>
      <td>Vietnam</td>
      <td>14.058324</td>
      <td>108.277199</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>269</th>
      <td>NaN</td>
      <td>West Bank and Gaza</td>
      <td>31.952200</td>
      <td>35.233200</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>270</th>
      <td>NaN</td>
      <td>Winter Olympics 2022</td>
      <td>39.904200</td>
      <td>116.407400</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>271</th>
      <td>NaN</td>
      <td>Yemen</td>
      <td>15.552727</td>
      <td>48.516388</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>272</th>
      <td>NaN</td>
      <td>Zambia</td>
      <td>-13.133897</td>
      <td>27.849332</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>273</th>
      <td>NaN</td>
      <td>Zimbabwe</td>
      <td>-19.015438</td>
      <td>29.154857</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 978 columns</p>
</div>




```python
print(daily_death.info())
daily_death.head(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 289 entries, 0 to 288
    Columns: 978 entries, Province/State to 9/21/22
    dtypes: float64(2), int64(974), object(2)
    memory usage: 2.2+ MB
    None
    




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
      <th>Province/State</th>
      <th>Country/Region</th>
      <th>Lat</th>
      <th>Long</th>
      <th>1/22/20</th>
      <th>1/23/20</th>
      <th>1/24/20</th>
      <th>1/25/20</th>
      <th>1/26/20</th>
      <th>1/27/20</th>
      <th>...</th>
      <th>9/12/22</th>
      <th>9/13/22</th>
      <th>9/14/22</th>
      <th>9/15/22</th>
      <th>9/16/22</th>
      <th>9/17/22</th>
      <th>9/18/22</th>
      <th>9/19/22</th>
      <th>9/20/22</th>
      <th>9/21/22</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>Afghanistan</td>
      <td>33.93911</td>
      <td>67.709953</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>7789</td>
      <td>7791</td>
      <td>7792</td>
      <td>7792</td>
      <td>7794</td>
      <td>7794</td>
      <td>7795</td>
      <td>7796</td>
      <td>7796</td>
      <td>7796</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>Albania</td>
      <td>41.15330</td>
      <td>20.168300</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>3585</td>
      <td>3586</td>
      <td>3586</td>
      <td>3586</td>
      <td>3586</td>
      <td>3586</td>
      <td>3586</td>
      <td>3588</td>
      <td>3589</td>
      <td>3589</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>Algeria</td>
      <td>28.03390</td>
      <td>1.659600</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>6879</td>
      <td>6879</td>
      <td>6879</td>
      <td>6879</td>
      <td>6879</td>
      <td>6879</td>
      <td>6879</td>
      <td>6879</td>
      <td>6879</td>
      <td>6879</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>Andorra</td>
      <td>42.50630</td>
      <td>1.521800</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>155</td>
      <td>155</td>
      <td>155</td>
      <td>155</td>
      <td>155</td>
      <td>155</td>
      <td>155</td>
      <td>155</td>
      <td>155</td>
      <td>155</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>Angola</td>
      <td>-11.20270</td>
      <td>17.873900</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>1917</td>
      <td>1917</td>
      <td>1917</td>
      <td>1917</td>
      <td>1917</td>
      <td>1917</td>
      <td>1917</td>
      <td>1917</td>
      <td>1917</td>
      <td>1917</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>Antarctica</td>
      <td>-71.94990</td>
      <td>23.347000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>Antigua and Barbuda</td>
      <td>17.06080</td>
      <td>-61.796400</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>145</td>
      <td>145</td>
      <td>145</td>
      <td>145</td>
      <td>145</td>
      <td>145</td>
      <td>145</td>
      <td>145</td>
      <td>145</td>
      <td>145</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NaN</td>
      <td>Argentina</td>
      <td>-38.41610</td>
      <td>-63.616700</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>129830</td>
      <td>129830</td>
      <td>129830</td>
      <td>129830</td>
      <td>129830</td>
      <td>129830</td>
      <td>129855</td>
      <td>129855</td>
      <td>129855</td>
      <td>129855</td>
    </tr>
    <tr>
      <th>8</th>
      <td>NaN</td>
      <td>Armenia</td>
      <td>40.06910</td>
      <td>45.038200</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>8669</td>
      <td>8669</td>
      <td>8669</td>
      <td>8669</td>
      <td>8669</td>
      <td>8669</td>
      <td>8669</td>
      <td>8679</td>
      <td>8679</td>
      <td>8679</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Australian Capital Territory</td>
      <td>Australia</td>
      <td>-35.47350</td>
      <td>149.012400</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>125</td>
      <td>125</td>
      <td>125</td>
      <td>125</td>
      <td>125</td>
      <td>125</td>
      <td>125</td>
      <td>125</td>
      <td>125</td>
      <td>125</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 978 columns</p>
</div>




```python
print(daily_death.info())
daily_death.tail(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 289 entries, 0 to 288
    Columns: 978 entries, Province/State to 9/21/22
    dtypes: float64(2), int64(974), object(2)
    memory usage: 2.2+ MB
    None
    




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
      <th>Province/State</th>
      <th>Country/Region</th>
      <th>Lat</th>
      <th>Long</th>
      <th>1/22/20</th>
      <th>1/23/20</th>
      <th>1/24/20</th>
      <th>1/25/20</th>
      <th>1/26/20</th>
      <th>1/27/20</th>
      <th>...</th>
      <th>9/12/22</th>
      <th>9/13/22</th>
      <th>9/14/22</th>
      <th>9/15/22</th>
      <th>9/16/22</th>
      <th>9/17/22</th>
      <th>9/18/22</th>
      <th>9/19/22</th>
      <th>9/20/22</th>
      <th>9/21/22</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>279</th>
      <td>NaN</td>
      <td>Uruguay</td>
      <td>-32.522800</td>
      <td>-55.765800</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>7462</td>
      <td>7462</td>
      <td>7462</td>
      <td>7462</td>
      <td>7462</td>
      <td>7462</td>
      <td>7462</td>
      <td>7473</td>
      <td>7473</td>
      <td>7473</td>
    </tr>
    <tr>
      <th>280</th>
      <td>NaN</td>
      <td>Uzbekistan</td>
      <td>41.377491</td>
      <td>64.585262</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>1637</td>
      <td>1637</td>
      <td>1637</td>
      <td>1637</td>
      <td>1637</td>
      <td>1637</td>
      <td>1637</td>
      <td>1637</td>
      <td>1637</td>
      <td>1637</td>
    </tr>
    <tr>
      <th>281</th>
      <td>NaN</td>
      <td>Vanuatu</td>
      <td>-15.376700</td>
      <td>166.959200</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>282</th>
      <td>NaN</td>
      <td>Venezuela</td>
      <td>6.423800</td>
      <td>-66.589700</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>5809</td>
      <td>5809</td>
      <td>5809</td>
      <td>5809</td>
      <td>5809</td>
      <td>5809</td>
      <td>5811</td>
      <td>5812</td>
      <td>5814</td>
      <td>5814</td>
    </tr>
    <tr>
      <th>283</th>
      <td>NaN</td>
      <td>Vietnam</td>
      <td>14.058324</td>
      <td>108.277199</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>43130</td>
      <td>43132</td>
      <td>43132</td>
      <td>43137</td>
      <td>43137</td>
      <td>43138</td>
      <td>43139</td>
      <td>43141</td>
      <td>43142</td>
      <td>43146</td>
    </tr>
    <tr>
      <th>284</th>
      <td>NaN</td>
      <td>West Bank and Gaza</td>
      <td>31.952200</td>
      <td>35.233200</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>5706</td>
      <td>5706</td>
      <td>5706</td>
      <td>5707</td>
      <td>5707</td>
      <td>5707</td>
      <td>5707</td>
      <td>5707</td>
      <td>5707</td>
      <td>5707</td>
    </tr>
    <tr>
      <th>285</th>
      <td>NaN</td>
      <td>Winter Olympics 2022</td>
      <td>39.904200</td>
      <td>116.407400</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>286</th>
      <td>NaN</td>
      <td>Yemen</td>
      <td>15.552727</td>
      <td>48.516388</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>2155</td>
      <td>2155</td>
      <td>2155</td>
      <td>2155</td>
      <td>2155</td>
      <td>2155</td>
      <td>2155</td>
      <td>2155</td>
      <td>2155</td>
      <td>2155</td>
    </tr>
    <tr>
      <th>287</th>
      <td>NaN</td>
      <td>Zambia</td>
      <td>-13.133897</td>
      <td>27.849332</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>4017</td>
      <td>4017</td>
      <td>4017</td>
      <td>4017</td>
      <td>4017</td>
      <td>4017</td>
      <td>4017</td>
      <td>4017</td>
      <td>4017</td>
      <td>4017</td>
    </tr>
    <tr>
      <th>288</th>
      <td>NaN</td>
      <td>Zimbabwe</td>
      <td>-19.015438</td>
      <td>29.154857</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>5596</td>
      <td>5596</td>
      <td>5596</td>
      <td>5596</td>
      <td>5596</td>
      <td>5598</td>
      <td>5598</td>
      <td>5598</td>
      <td>5598</td>
      <td>5598</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 978 columns</p>
</div>




```python
print(vulner_index.info())
vulner_index.head(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 37 entries, 0 to 36
    Data columns (total 12 columns):
     #   Column                   Non-Null Count  Dtype  
    ---  ------                   --------------  -----  
     0   states                   37 non-null     object 
     1   region                   37 non-null     object 
     2   Population               37 non-null     int64  
     3   Overall CCVI Index       37 non-null     float64
     4   Age                      37 non-null     float64
     5   Epidemiological          37 non-null     float64
     6   Fragility                37 non-null     float64
     7   Health System            37 non-null     float64
     8   Population Density       37 non-null     float64
     9   Socio-Economic           37 non-null     float64
     10   Transport Availability  37 non-null     float64
     11  Acute IHR                37 non-null     float64
    dtypes: float64(9), int64(1), object(2)
    memory usage: 3.6+ KB
    None
    




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
      <th>states</th>
      <th>region</th>
      <th>Population</th>
      <th>Overall CCVI Index</th>
      <th>Age</th>
      <th>Epidemiological</th>
      <th>Fragility</th>
      <th>Health System</th>
      <th>Population Density</th>
      <th>Socio-Economic</th>
      <th>Transport Availability</th>
      <th>Acute IHR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>FCT</td>
      <td>North Central</td>
      <td>4865000</td>
      <td>0.3</td>
      <td>0.0</td>
      <td>0.9</td>
      <td>0.4</td>
      <td>0.6</td>
      <td>0.9</td>
      <td>0.6</td>
      <td>0.2</td>
      <td>0.79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Plateau</td>
      <td>North Central</td>
      <td>4766000</td>
      <td>0.4</td>
      <td>0.5</td>
      <td>0.4</td>
      <td>0.8</td>
      <td>0.3</td>
      <td>0.3</td>
      <td>0.5</td>
      <td>0.3</td>
      <td>0.93</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Kwara</td>
      <td>North Central</td>
      <td>3524000</td>
      <td>0.3</td>
      <td>0.4</td>
      <td>0.3</td>
      <td>0.2</td>
      <td>0.4</td>
      <td>0.2</td>
      <td>0.6</td>
      <td>0.7</td>
      <td>0.93</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Nassarawa</td>
      <td>North Central</td>
      <td>2783000</td>
      <td>0.1</td>
      <td>0.3</td>
      <td>0.5</td>
      <td>0.9</td>
      <td>0.0</td>
      <td>0.1</td>
      <td>0.6</td>
      <td>0.5</td>
      <td>0.85</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Niger</td>
      <td>North Central</td>
      <td>6260000</td>
      <td>0.6</td>
      <td>0.0</td>
      <td>0.6</td>
      <td>0.3</td>
      <td>0.7</td>
      <td>0.1</td>
      <td>0.8</td>
      <td>0.8</td>
      <td>0.84</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Benue</td>
      <td>North Central</td>
      <td>6376000</td>
      <td>0.5</td>
      <td>0.7</td>
      <td>0.5</td>
      <td>0.7</td>
      <td>0.4</td>
      <td>0.4</td>
      <td>0.3</td>
      <td>0.5</td>
      <td>0.91</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Kogi</td>
      <td>North Central</td>
      <td>4970000</td>
      <td>0.1</td>
      <td>0.3</td>
      <td>0.2</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.4</td>
      <td>0.3</td>
      <td>0.6</td>
      <td>0.87</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Bauchi</td>
      <td>North East</td>
      <td>7270000</td>
      <td>0.8</td>
      <td>0.1</td>
      <td>0.2</td>
      <td>0.8</td>
      <td>0.8</td>
      <td>0.2</td>
      <td>0.8</td>
      <td>0.8</td>
      <td>0.85</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Gombe</td>
      <td>North East</td>
      <td>3692000</td>
      <td>1.0</td>
      <td>0.4</td>
      <td>0.4</td>
      <td>0.9</td>
      <td>0.9</td>
      <td>0.3</td>
      <td>0.8</td>
      <td>0.7</td>
      <td>0.83</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Borno</td>
      <td>North East</td>
      <td>6651000</td>
      <td>0.9</td>
      <td>0.3</td>
      <td>0.1</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.1</td>
      <td>0.7</td>
      <td>0.9</td>
      <td>0.89</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(vulner_index.info())
vulner_index.tail(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 37 entries, 0 to 36
    Data columns (total 12 columns):
     #   Column                   Non-Null Count  Dtype  
    ---  ------                   --------------  -----  
     0   states                   37 non-null     object 
     1   region                   37 non-null     object 
     2   Population               37 non-null     int64  
     3   Overall CCVI Index       37 non-null     float64
     4   Age                      37 non-null     float64
     5   Epidemiological          37 non-null     float64
     6   Fragility                37 non-null     float64
     7   Health System            37 non-null     float64
     8   Population Density       37 non-null     float64
     9   Socio-Economic           37 non-null     float64
     10   Transport Availability  37 non-null     float64
     11  Acute IHR                37 non-null     float64
    dtypes: float64(9), int64(1), object(2)
    memory usage: 3.6+ KB
    None
    




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
      <th>states</th>
      <th>region</th>
      <th>Population</th>
      <th>Overall CCVI Index</th>
      <th>Age</th>
      <th>Epidemiological</th>
      <th>Fragility</th>
      <th>Health System</th>
      <th>Population Density</th>
      <th>Socio-Economic</th>
      <th>Transport Availability</th>
      <th>Acute IHR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>27</th>
      <td>Delta</td>
      <td>South South</td>
      <td>6303000</td>
      <td>0.4</td>
      <td>0.6</td>
      <td>0.7</td>
      <td>0.2</td>
      <td>1.0</td>
      <td>0.6</td>
      <td>0.5</td>
      <td>0.4</td>
      <td>1.08</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Ebonyi</td>
      <td>South South</td>
      <td>3192000</td>
      <td>0.6</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.6</td>
      <td>0.1</td>
      <td>0.7</td>
      <td>0.3</td>
      <td>0.6</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Bayelsa</td>
      <td>South South</td>
      <td>2606000</td>
      <td>0.5</td>
      <td>0.8</td>
      <td>0.6</td>
      <td>0.1</td>
      <td>0.9</td>
      <td>0.5</td>
      <td>0.2</td>
      <td>0.7</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Cross river</td>
      <td>South South</td>
      <td>4272000</td>
      <td>0.5</td>
      <td>0.4</td>
      <td>0.7</td>
      <td>0.8</td>
      <td>0.1</td>
      <td>0.4</td>
      <td>0.4</td>
      <td>0.6</td>
      <td>0.98</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Lagos</td>
      <td>South West</td>
      <td>13992000</td>
      <td>0.0</td>
      <td>0.1</td>
      <td>1.0</td>
      <td>0.3</td>
      <td>0.1</td>
      <td>1.0</td>
      <td>0.1</td>
      <td>0.4</td>
      <td>0.93</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Oyo</td>
      <td>South West</td>
      <td>8737000</td>
      <td>0.2</td>
      <td>0.7</td>
      <td>0.8</td>
      <td>0.2</td>
      <td>0.8</td>
      <td>0.6</td>
      <td>0.2</td>
      <td>0.3</td>
      <td>1.06</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Ogun</td>
      <td>South West</td>
      <td>5878000</td>
      <td>0.3</td>
      <td>0.6</td>
      <td>0.7</td>
      <td>0.5</td>
      <td>0.6</td>
      <td>0.6</td>
      <td>0.0</td>
      <td>0.2</td>
      <td>1.07</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Ondo</td>
      <td>South West</td>
      <td>5185000</td>
      <td>0.1</td>
      <td>0.8</td>
      <td>0.5</td>
      <td>0.1</td>
      <td>0.3</td>
      <td>0.6</td>
      <td>0.3</td>
      <td>0.3</td>
      <td>1.04</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Osun</td>
      <td>South West</td>
      <td>5252000</td>
      <td>0.0</td>
      <td>0.7</td>
      <td>0.4</td>
      <td>0.4</td>
      <td>0.0</td>
      <td>0.8</td>
      <td>0.1</td>
      <td>0.2</td>
      <td>1.06</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Ekiti</td>
      <td>South West</td>
      <td>3593000</td>
      <td>0.3</td>
      <td>0.8</td>
      <td>0.3</td>
      <td>0.5</td>
      <td>0.2</td>
      <td>0.8</td>
      <td>0.1</td>
      <td>0.4</td>
      <td>1.03</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(gdp_data.info())
gdp_data.head(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 7 entries, 0 to 6
    Data columns (total 5 columns):
     #   Column  Non-Null Count  Dtype  
    ---  ------  --------------  -----  
     0   Year    7 non-null      int64  
     1   Q1      7 non-null      float64
     2   Q2      7 non-null      float64
     3   Q3      7 non-null      float64
     4   Q4      7 non-null      float64
    dtypes: float64(4), int64(1)
    memory usage: 408.0 bytes
    None
    




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
      <th>Year</th>
      <th>Q1</th>
      <th>Q2</th>
      <th>Q3</th>
      <th>Q4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2014</td>
      <td>15438679.50</td>
      <td>16084622.31</td>
      <td>17479127.58</td>
      <td>18150356.45</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2015</td>
      <td>16050601.38</td>
      <td>16463341.91</td>
      <td>17976234.59</td>
      <td>18533752.07</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016</td>
      <td>15943714.54</td>
      <td>16218542.41</td>
      <td>17555441.69</td>
      <td>18213537.29</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2017</td>
      <td>15797965.83</td>
      <td>16334719.27</td>
      <td>17760228.17</td>
      <td>18598067.07</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018</td>
      <td>16096654.19</td>
      <td>16580508.07</td>
      <td>18081342.10</td>
      <td>19041437.59</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2019</td>
      <td>16434552.65</td>
      <td>16931434.89</td>
      <td>18494114.17</td>
      <td>19530000.00</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2020</td>
      <td>16740000.00</td>
      <td>15890000.00</td>
      <td>17820000.00</td>
      <td>0.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(gdp_data.info())
gdp_data.tail(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 7 entries, 0 to 6
    Data columns (total 5 columns):
     #   Column  Non-Null Count  Dtype  
    ---  ------  --------------  -----  
     0   Year    7 non-null      int64  
     1   Q1      7 non-null      float64
     2   Q2      7 non-null      float64
     3   Q3      7 non-null      float64
     4   Q4      7 non-null      float64
    dtypes: float64(4), int64(1)
    memory usage: 408.0 bytes
    None
    




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
      <th>Year</th>
      <th>Q1</th>
      <th>Q2</th>
      <th>Q3</th>
      <th>Q4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2014</td>
      <td>15438679.50</td>
      <td>16084622.31</td>
      <td>17479127.58</td>
      <td>18150356.45</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2015</td>
      <td>16050601.38</td>
      <td>16463341.91</td>
      <td>17976234.59</td>
      <td>18533752.07</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016</td>
      <td>15943714.54</td>
      <td>16218542.41</td>
      <td>17555441.69</td>
      <td>18213537.29</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2017</td>
      <td>15797965.83</td>
      <td>16334719.27</td>
      <td>17760228.17</td>
      <td>18598067.07</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018</td>
      <td>16096654.19</td>
      <td>16580508.07</td>
      <td>18081342.10</td>
      <td>19041437.59</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2019</td>
      <td>16434552.65</td>
      <td>16931434.89</td>
      <td>18494114.17</td>
      <td>19530000.00</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2020</td>
      <td>16740000.00</td>
      <td>15890000.00</td>
      <td>17820000.00</td>
      <td>0.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(budget_data.info())
budget_data.head(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 37 entries, 0 to 36
    Data columns (total 3 columns):
     #   Column               Non-Null Count  Dtype  
    ---  ------               --------------  -----  
     0   states               37 non-null     object 
     1   Initial_budget (Bn)  37 non-null     float64
     2   Revised_budget (Bn)  37 non-null     float64
    dtypes: float64(2), object(1)
    memory usage: 1016.0+ bytes
    None
    




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
      <th>states</th>
      <th>Initial_budget (Bn)</th>
      <th>Revised_budget (Bn)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Abia</td>
      <td>136.60</td>
      <td>102.70</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Adamawa</td>
      <td>183.30</td>
      <td>139.31</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Akwa-Ibom</td>
      <td>597.73</td>
      <td>366.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Anambra</td>
      <td>137.10</td>
      <td>112.80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bauchi</td>
      <td>167.20</td>
      <td>128.00</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Bayelsa</td>
      <td>242.18</td>
      <td>183.15</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Benue</td>
      <td>189.00</td>
      <td>119.00</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Borno</td>
      <td>146.80</td>
      <td>108.80</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Cross River</td>
      <td>1100.00</td>
      <td>147.10</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Delta</td>
      <td>395.50</td>
      <td>282.30</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(budget_data.info())
budget_data.tail(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 37 entries, 0 to 36
    Data columns (total 3 columns):
     #   Column               Non-Null Count  Dtype  
    ---  ------               --------------  -----  
     0   states               37 non-null     object 
     1   Initial_budget (Bn)  37 non-null     float64
     2   Revised_budget (Bn)  37 non-null     float64
    dtypes: float64(2), object(1)
    memory usage: 1016.0+ bytes
    None
    




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
      <th>states</th>
      <th>Initial_budget (Bn)</th>
      <th>Revised_budget (Bn)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>27</th>
      <td>Ondo</td>
      <td>187.80</td>
      <td>151.4</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Osun</td>
      <td>119.60</td>
      <td>82.2</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Oyo</td>
      <td>213.00</td>
      <td>174.0</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Plateau</td>
      <td>177.30</td>
      <td>122.0</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Rivers</td>
      <td>530.80</td>
      <td>300.4</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Sokoto</td>
      <td>202.40</td>
      <td>153.0</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Taraba</td>
      <td>215.00</td>
      <td>150.5</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Yobe</td>
      <td>108.00</td>
      <td>86.0</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Zamfara</td>
      <td>188.50</td>
      <td>127.3</td>
    </tr>
    <tr>
      <th>36</th>
      <td>FCT</td>
      <td>278.78</td>
      <td>199.0</td>
    </tr>
  </tbody>
</table>
</div>



### Task 3 - Data Cleaning and Preparation
From the information obtained above, you will need to fix the data format. 
<br>
Examples: 
* Convert to appropriate data type.
* Rename the columns of the scraped data.
* Remove comma(,) in numerical data
* Extract daily data for Nigeria from the Global daily cases data

TODO A - Clean the scraped data


```python
ncdc_data.head(4)
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
      <th>States Affected</th>
      <th>No. of Cases (Lab Confirmed)</th>
      <th>No. of Cases (on admission)</th>
      <th>No. Discharged</th>
      <th>No. of Deaths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lagos</td>
      <td>103,835</td>
      <td>692</td>
      <td>102,372</td>
      <td>771</td>
    </tr>
    <tr>
      <th>1</th>
      <td>FCT</td>
      <td>29,360</td>
      <td>220</td>
      <td>28,891</td>
      <td>249</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Rivers</td>
      <td>17,976</td>
      <td>180</td>
      <td>17,641</td>
      <td>155</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kaduna</td>
      <td>11,553</td>
      <td>12</td>
      <td>11,452</td>
      <td>89</td>
    </tr>
  </tbody>
</table>
</div>




```python
ncdc_data = ncdc_data.rename(columns = {'States Affected': 'State', 'No. of Cases (Lab Confirmed)': 'Confirmed', 
                 'No. of Cases (on admission)': 'Admitted', 'No. Discharged': 'Discharged',
                 'No. of Deaths': 'Dead'})
```


```python
ncdc_data
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
      <th>Confirmed</th>
      <th>Admitted</th>
      <th>Discharged</th>
      <th>Dead</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lagos</td>
      <td>103,835</td>
      <td>692</td>
      <td>102,372</td>
      <td>771</td>
    </tr>
    <tr>
      <th>1</th>
      <td>FCT</td>
      <td>29,360</td>
      <td>220</td>
      <td>28,891</td>
      <td>249</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Rivers</td>
      <td>17,976</td>
      <td>180</td>
      <td>17,641</td>
      <td>155</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kaduna</td>
      <td>11,553</td>
      <td>12</td>
      <td>11,452</td>
      <td>89</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Oyo</td>
      <td>10,329</td>
      <td>0</td>
      <td>10,127</td>
      <td>202</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Plateau</td>
      <td>10,326</td>
      <td>9</td>
      <td>10,242</td>
      <td>75</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Edo</td>
      <td>7,914</td>
      <td>31</td>
      <td>7,561</td>
      <td>322</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Ogun</td>
      <td>5,810</td>
      <td>11</td>
      <td>5,717</td>
      <td>82</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Delta</td>
      <td>5,707</td>
      <td>425</td>
      <td>5,170</td>
      <td>112</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Kano</td>
      <td>5,337</td>
      <td>170</td>
      <td>5,040</td>
      <td>127</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Ondo</td>
      <td>5,173</td>
      <td>315</td>
      <td>4,749</td>
      <td>109</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Akwa Ibom</td>
      <td>5,004</td>
      <td>7</td>
      <td>4,953</td>
      <td>44</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Kwara</td>
      <td>4,691</td>
      <td>452</td>
      <td>4,175</td>
      <td>64</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Gombe</td>
      <td>3,313</td>
      <td>8</td>
      <td>3,239</td>
      <td>66</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Osun</td>
      <td>3,311</td>
      <td>29</td>
      <td>3,190</td>
      <td>92</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Enugu</td>
      <td>2,952</td>
      <td>13</td>
      <td>2,910</td>
      <td>29</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Anambra</td>
      <td>2,825</td>
      <td>46</td>
      <td>2,760</td>
      <td>19</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Nasarawa</td>
      <td>2,777</td>
      <td>393</td>
      <td>2,345</td>
      <td>39</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Imo</td>
      <td>2,655</td>
      <td>7</td>
      <td>2,590</td>
      <td>58</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Ekiti</td>
      <td>2,464</td>
      <td>6</td>
      <td>2,430</td>
      <td>28</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Katsina</td>
      <td>2,418</td>
      <td>0</td>
      <td>2,381</td>
      <td>37</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Abia</td>
      <td>2,259</td>
      <td>3</td>
      <td>2,222</td>
      <td>34</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Benue</td>
      <td>2,129</td>
      <td>340</td>
      <td>1,764</td>
      <td>25</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Ebonyi</td>
      <td>2,064</td>
      <td>28</td>
      <td>2,004</td>
      <td>32</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Bauchi</td>
      <td>1,990</td>
      <td>18</td>
      <td>1,948</td>
      <td>24</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Borno</td>
      <td>1,629</td>
      <td>5</td>
      <td>1,580</td>
      <td>44</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Taraba</td>
      <td>1,474</td>
      <td>63</td>
      <td>1,377</td>
      <td>34</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Bayelsa</td>
      <td>1,363</td>
      <td>10</td>
      <td>1,325</td>
      <td>28</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Adamawa</td>
      <td>1,312</td>
      <td>134</td>
      <td>1,140</td>
      <td>38</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Niger</td>
      <td>1,183</td>
      <td>165</td>
      <td>998</td>
      <td>20</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Cross River</td>
      <td>886</td>
      <td>4</td>
      <td>857</td>
      <td>25</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Sokoto</td>
      <td>822</td>
      <td>0</td>
      <td>794</td>
      <td>28</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Jigawa</td>
      <td>669</td>
      <td>2</td>
      <td>649</td>
      <td>18</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Yobe</td>
      <td>638</td>
      <td>4</td>
      <td>625</td>
      <td>9</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Kebbi</td>
      <td>480</td>
      <td>10</td>
      <td>454</td>
      <td>16</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Zamfara</td>
      <td>375</td>
      <td>0</td>
      <td>366</td>
      <td>9</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Kogi</td>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
for column in ncdc_data.columns:
    if column != 'State':
        ncdc_data[column] = ncdc_data[column].apply(lambda x: int(x.replace(',', '')))
```


```python
ncdc_data.head(5)
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
      <th>Confirmed</th>
      <th>Admitted</th>
      <th>Discharged</th>
      <th>Dead</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lagos</td>
      <td>103835</td>
      <td>692</td>
      <td>102372</td>
      <td>771</td>
    </tr>
    <tr>
      <th>1</th>
      <td>FCT</td>
      <td>29360</td>
      <td>220</td>
      <td>28891</td>
      <td>249</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Rivers</td>
      <td>17976</td>
      <td>180</td>
      <td>17641</td>
      <td>155</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kaduna</td>
      <td>11553</td>
      <td>12</td>
      <td>11452</td>
      <td>89</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Oyo</td>
      <td>10329</td>
      <td>0</td>
      <td>10127</td>
      <td>202</td>
    </tr>
  </tbody>
</table>
</div>



TODO B - Get a Pandas DataFrame for Daily Confirmed Cases in Nigeria. Columns are Date and Cases


```python
ng_daily_confirmed = daily_confirmed[daily_confirmed['Country/Region']=='Nigeria'].drop(['Province/State', 'Country/Region', 'Lat', 'Long'], axis =1)
ng_daily_confirmed = ng_daily_confirmed.melt(var_name = 'Date', value_name = 'Cases')
ng_daily_confirmed['Date'] = pd.to_datetime(ng_daily_confirmed['Date'])
```


```python
ng_daily_confirmed[100:103]
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
      <th>Date</th>
      <th>Cases</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>100</th>
      <td>2020-05-01</td>
      <td>2170</td>
    </tr>
    <tr>
      <th>101</th>
      <td>2020-05-02</td>
      <td>2388</td>
    </tr>
    <tr>
      <th>102</th>
      <td>2020-05-03</td>
      <td>2558</td>
    </tr>
  </tbody>
</table>
</div>



TODO C - Get a Pandas DataFrame for Daily Recovered Cases in Nigeria. Columns are Date and Cases


```python
ng_daily_recovered = daily_recovered[daily_confirmed['Country/Region']=='Nigeria'].drop(['Province/State', 'Country/Region', 'Lat', 'Long'], axis =1)
ng_daily_recovered = ng_daily_recovered.melt(var_name = 'Date', value_name = 'Cases')
ng_daily_recovered['Date'] = pd.to_datetime(ng_daily_recovered['Date'])
```


```python
ng_daily_recovered[100:103]
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
      <th>Date</th>
      <th>Cases</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>100</th>
      <td>2020-05-01</td>
      <td>13220</td>
    </tr>
    <tr>
      <th>101</th>
      <td>2020-05-02</td>
      <td>15013</td>
    </tr>
    <tr>
      <th>102</th>
      <td>2020-05-03</td>
      <td>16639</td>
    </tr>
  </tbody>
</table>
</div>



TODO D - Get a Pandas DataFrame for Daily Death Cases in Nigeria. Columns are Date and Cases


```python
ng_daily_death = daily_recovered[daily_death['Country/Region']=='Nigeria'].drop(['Province/State', 'Country/Region', 'Lat', 'Long'], axis =1)
ng_daily_death = ng_daily_death.melt(var_name = 'Date', value_name = 'Cases')
ng_daily_death['Date'] = pd.to_datetime(ng_daily_death['Date'])
```


```python
ng_daily_death[100:103]
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
      <th>Date</th>
      <th>Cases</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>100</th>
      <td>2020-05-01</td>
      <td>13220</td>
    </tr>
    <tr>
      <th>101</th>
      <td>2020-05-02</td>
      <td>15013</td>
    </tr>
    <tr>
      <th>102</th>
      <td>2020-05-03</td>
      <td>16639</td>
    </tr>
  </tbody>
</table>
</div>



### Task 4 - Analysis
Here you will perform some analyses on the datasets. You are welcome to communicate findings in charts and summary. 
<br>
We have included a few TODOs to help with your analysis. However, do not let this limit your approach, feel free to include more, and be sure to support your findings with chart and summary 

TODO A - Generate a plot that shows the Top 10 states in terms of Confirmed Covid cases by Laboratory test


```python
# outcome from testing, nlargest runs faster than sort_values()
max_confirmed = ncdc_data.iloc[ncdc_data['Confirmed'].nlargest(10).index] 
```


```python
# theme setting for testing plots
theme = sns.color_palette('Greens_r', 20)
paper_rc = {'lines.linewidth': 1, 'lines.markersize': 15}
sns.set_context('paper', rc = paper_rc)
```


```python
plt.figure(figsize = (10, 8))

plt.title('States with Highest Confirmed Cases', fontsize = 18)

sns.barplot(x = 'State', y = 'Confirmed', data = max_confirmed, palette = theme)

plt.xlabel('State', fontsize = 12)
plt.ylabel('No. of Cases', fontsize = 12)

#plt.savefig('States with Highest Confirmed Cases.png', dpi = 300, transparent = True)

plt.show()
```


    
![png](output_45_0.png)
    



```python
#The above plot indicates that Lagos has the highest number of cases in comparison to other states
```

TODO B - Generate a plot that shows the Top 10 states in terms of Discharged Covid cases. Hint - Sort the values


```python
#testing outcome, nlargest runs faster than sort sort_values()
max_discharged = ncdc_data.iloc[ncdc_data['Discharged'].nlargest(10).index]
```


```python
plt.figure(figsize = (10, 8))

plt.title('States with Highest Discharged Cases', fontsize = 18)

sns.barplot(x = 'State', y = 'Discharged', data = max_discharged, palette = theme)

plt.xlabel('State', fontsize = 12)
plt.ylabel('No. of Cases', fontsize = 12)

#plt.savefig('States with Highest Discharged Cases.png', dpi = 300, transparent = True)

plt.show()
```


    
![png](output_49_0.png)
    



```python
#The above plot indicates that Lagos has the highest number of discharged cases in comparison to other states
```

TODO D - Plot the top 10 Death cases


```python
# After testing, nlargest runs faster than sort sort_values()
max_deaths = ncdc_data.iloc[ncdc_data['Dead'].nlargest(10).index]
```


```python
plt.figure(figsize = (10, 8))

plt.title('States with Highest Death Cases', fontsize = 18)

sns.barplot(x = 'State', y = 'Dead', data = max_deaths, palette = theme)

plt.xlabel('State', fontsize = 12)
plt.ylabel('No. of Cases', fontsize = 12)

#plt.savefig('States with Highest Discharged Cases.png', dpi = 300, transparent = True)

plt.show()
```


    
![png](output_53_0.png)
    



```python
#The above plot indicates that Lagos has the highest number of discharged cases in comparison to other states
```

TODO E - Generate a line plot for the total daily confirmed, recovered and death cases in Nigeria


```python
plt.figure(figsize = (12, 10))
plt.xticks(rotation = 90)
plt.ylabel('No. of cases', fontsize = 12)
plt.title('Daily report of confirmed, recovered and death cases', fontsize = 25)

sns.lineplot(x = 'Date', y = 'Cases', data = ng_daily_confirmed, label = 'Confirmed', linewidth = 2)
sns.lineplot(x = 'Date', y = 'Cases', data = ng_daily_recovered, label = 'Recovered', linewidth = 2)
sns.lineplot(x = 'Date', y = 'Cases', data = ng_daily_death, label = 'Death', linewidth = 2)

#plt.savefig('Daily report of Cases. png', dpi = 300, transparent = True)

plt.show()
```


    
![png](output_56_0.png)
    



```python
#The above plot shows
#1. a steady rise in the number of confirmed case
#2. a spontaneous decline in the number of death from September
#3. a decline in the number of recorvered given the decline in confirmed cases 
```

TODO F - 
* Determine the daily infection rate, you can use the Pandas `diff` method to find the derivate of the total cases.
* Generate a line plot for the above


```python
daily_infection_rate = ng_daily_confirmed['Cases']. diff()
```


```python
plt.figure(figsize = (15, 10))
plt.xticks(rotation = 90)
plt.ylabel('Infection Rate', fontsize = 12)
plt.xlabel('Date', fontsize = 12)
plt.title('Daily infection Rate', fontsize = 20)

sns.lineplot(x = ng_daily_confirmed.Date, y = daily_infection_rate, linewidth = 1, color = 'darkgreen', palette = theme)


#plt.savefig('Daily Infection Rate. png', dpi = 300, transparebt = True)

plt.show()
```


    
![png](output_60_0.png)
    


TODO G - 
* Calculate maximum infection rate for a day (Number of new cases)
* Find the date


```python
print('Maximum infectionrate: ', daily_infection_rate.max())
```

    Maximum infectionrate:  6158.0
    


```python
print('Maximum infection rate was recorde on: ', ng_daily_confirmed.max())
```

    Maximum infection rate was recorde on:  Date     2022-09-21 00:00:00
    Cases                 265008
    dtype: object
    

TODO H - Determine the relationship between the external dataset and the NCDC COVID-19 dataset. 
Here you will generate a line plot of top 10 confirmed cases and the overall community vulnerability index on the same axis. From the graph, explain your observation.
<br>
Steps
* Combine the two dataset together on a common column(states)
* Create a new dataframe for plotting. This DataFrame will contain top 10 states in terms of confirmed cases i.e sort by confirmed cases. ** Hint: Check out Pandas [nlargest](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.nlargest.html) function. This [tutorial](https://cmdlinetips.com/2019/03/how-to-select-top-n-rows-with-the-largest-values-in-a-columns-in-pandas/) can help out ** 
* Plot both variable on the same axis. Check out this [tutorial](http://kitchingroup.cheme.cmu.edu/blog/2013/09/13/Plotting-two-datasets-with-very-different-scales/)


```python
vulner_index = vulner_index.rename(columns = {'states' : 'State'})

vulner_index['States'] = vulner_index['State'].replace('Abuja', 'Abuja')
vulner_index['State'].replace('Cross river', 'Cross River')

merged_data = ncdc_data.merge(vulner_index, on = 'State', how = 'left')
```


```python
mini_merged_data = merged_data.iloc[merged_data['Confirmed'].nlargest(10).index]
```


```python
plt.figure(figsize = (12, 10))
plt.title('Relationship between Confirmed Cases and Vulnerability Index', fontsize = 21)

axis_1 = sns.pointplot(x = 'State', y = 'Confirmed', data = mini_merged_data, color = 'green', markers ='>')

plt.xlabel('State', fontsize =11)
plt.ylabel('No. of cases', fontsize = 11)

axis_2 = axis_1.twinx()

sns.pointplot(x = 'State', y = 'Overall CCVI Index', data = mini_merged_data, ax = axis_2, color = 'red', markers = '>')
 
plt.ylabel('COVID 19 Commuinty Vulnerability Index', fontsize = 11)

#plt.savefig('Confirmed Cases and Vulnerability Index.png', dpi = 300, transparent = True)

plt.show()
```


    
![png](output_67_0.png)
    



```python
# Observation: The plot indicates an insignificant relationship between CCVI Index and COVID19 case confirmation. 
#By this, the confirmation of cases has not be proven to be the reason behind the index position by state
# For instance, states with the highest number of confirmed cases tend to be the least vulnerable as depicted in the plot above
# the question will now be what makes the correlation insignificant.

```

TODO I - Determine the relationship between the external dataset and the NCDC COVID-19 dataset. 
* Here you will generate a regression plot between two variables to visualize the linear relationships - Confirmed Cases and Population Density.
Hint: Check out Seaborn [Regression Plot](https://seaborn.pydata.org/generated/seaborn.regplot.html).
* Provide a summary of your observation


```python
plt.figure(figsize=(10, 8))
plt.title('Line that best fits the relation between Population Density and Confirmed Cases', fontsize = 18)

sns.regplot(x = 'Population Density', y = 'Confirmed', data = mini_merged_data, color = 'darkgreen')

plt.ylabel('No. of Cases', fontsize = 12)
plt.xlabel('Population Density', fontsize = 12)

#plt.savefig('Regplot.png', dpi = 300, transparent = True)

plt.show()

```


    
![png](output_70_0.png)
    



```python
# This plot shows a positive correlation between population density and confirmed cases of an area.
# Studied carefully though, the 'line of best fit' that sns.regplot() returns captures perfectly only one
# data point. This shows that the two features have at best a moderate correlation.
# This means that a city with high population per area will fairly likely have high COVID cases. 
```

TODO J - 
* Provide more analyses by extending TODO G & H. Meaning, determine relationships between more features.
* Provide a detailed summary of your findings. 
* Note that you can have as many as possible.


```python
plt.figure(figsize = (15, 12))
plt.title('Correlation between numerical features', fontsize = 19)

sns.heatmap(merged_data.corr(), annot = True, cmap = theme, center = 0.1)

#plt.savefig('Correlation between numerical features.png', dpi = 300, transparent = True)

plt.show()

```


    
![png](output_73_0.png)
    



```python
# The above plot shows the correlation degree between variables and inferences from this is as thus;

#1 Acute IHR and confirmed have a weak correlation
# The weak correlation indicates that a progression of either variable has no effect on the other

#2 socio-economic and age have a positive correlation
#  By this, the socio-economic advancement indicates the viability of a population with regards to the age of the populace

#3 The health system has a positive correlation with overall CCVI index
#  Here, the evidence of development in affects health facilitation and the overall well-being of the populace.

#4Transport availabity has positive correlation with overall CCVI index

# 5. Transport and Population Density (Very strong and negative)
#    This says transport systems get clogged when there is a higher concentration of people to an area.

# 6. Number of Admitted Cases and Transport Availability (Very weak and negative)
#    This says there is no relation between the quality of transport of a place to admitted cases.

# 7. Number of Confirmed Cases and Population (Strong and positive) 
#    The number of confirmed cases will definitely grow higher with population increase.

```


```python
figure, axes = plt.subplots(1, 2, figsize = (15, 8))

xticklabels = axes[0].get_xticklabels()

axes[0].set_xticklabels(xticklabels, rotation = 90)
axes[0].set_title('Correlation between Overall CCVI Index and other features', fontsize = 12)
axes[0].set_ylabel('Overall CCVI Index', fontsize = 12)

axes[1].set_xticklabels(xticklabels, rotation = 90)
axes[1].set_title('Correlation between Confirmed Cases and other features', fontsize = 12)
axes[1].set_ylabel('No. of Cases', fontsize = 12)
sns.barplot(x = merged_data.corr().columns, y = merged_data.corr()['Overall CCVI Index'], ax = axes[0], palette = theme)
sns.barplot(x = merged_data.corr().columns, y = merged_data.corr()['Confirmed'], ax = axes[1], palette = theme)

#plt.savefig('Correlation between main features and others.png', dpi = 300, transparent = True)

plt.show()

```


    
![png](output_75_0.png)
    



```python
# This plot shows the correlation between all other variables and the two main varibles (Vulnerability Index and Number of Confirmed Cases)# From it, we get the features that drive our main variables the most.

# For Overall CCVI Index we have Socio-Economic status, Transport, Fragility and Health System as the main drivers. This will be proven below
# by plotting their relationship.

# As for Number of Confirmed Cases, Population seems to be the biggest driver (This is not taking into consideration obvious drivers like
# Number of Admitted, Discharged and Dead patients). 

```


```python
def plot_relation(m_feature, features):
    for feature in features:
        if feature != m_feature:                                                
            
            plt.figure(figsize = (15, 12))
            plt.title('Relationship between ' + m_feature + ' and ' + feature, fontsize = 20)

            axis_1 = sns.pointplot(x = 'State', y = m_feature, data = merged_data, color = 'darkorange')

            plt.xlabel('State', fontsize = 12)
            plt.ylabel(m_feature, fontsize = 12)
            plt.xticks(rotation = 90)

            axis_2 = axis_1.twinx()

            sns.pointplot(x = 'State', y = feature, data = merged_data, ax = axis_2, color = 'darkblue')
 
            #plt.savefig(str('Relationship between ' + m_feature + ' and ' + feature + '.png'), dpi = 300, transparent = True)
            
            plt.show()

```


```python
top_relating_features = merged_data.corr()['Overall CCVI Index'].abs().nlargest(5)
plot_relation('Overall CCVI Index', top_relating_features.index)

```


    
![png](output_78_0.png)
    



    
![png](output_78_1.png)
    



    
![png](output_78_2.png)
    



    
![png](output_78_3.png)
    



```python
# The plot points closely tracking each other show very strong correlation to Vulnerability Index
```


```python
plt.figure(figsize = (15, 12))
plt.title('Relationship between Age and Acute IHR', fontsize = 25)

axis_1 = sns.pointplot(x = 'State', y = 'Acute IHR', data = merged_data, color = 'darkorange', markers = 'o')

plt.xlabel('State', fontsize = 12)
plt.ylabel('Acute IHR', fontsize = 12)
plt.xticks(rotation = 90)

axis_2 = axis_1.twinx()

sns.pointplot(x = 'State', y = 'Age', data = merged_data, ax = axis_2, color = 'darkblue', markers = 'o')

plt.ylabel('Age', fontsize = 12)

#plt.savefig('Relationship between Age and Acute IHR.png', dpi = 300, transparent = True)

plt.show()

```


    
![png](output_80_0.png)
    



```python
# It is interesting to see the data conform strongly with the fact that the elderly in any place generally need greater health care than the youngsters. 
```


```python
budget_data = budget_data.melt(id_vars = 'states', value_vars = ['Initial_budget (Bn)', 'Revised_budget (Bn)'], var_name = 'Budget Type', value_name = 'Budget Value')
```


```python
plt.figure(figsize = (12, 8))
plt.xticks(rotation = 90)
plt.title('COVID 19 effect on State Budgets', fontsize = 18)

sns.barplot(x = 'states', y = 'Budget Value', hue = 'Budget Type', data = budget_data, palette = theme)

plt.ylabel('Budget Value (Bn)', fontsize = 12)
plt.xlabel('State', fontsize = 12)

#plt.savefig('COVID 19 effect on State Budgets.png', dpi = 300, transparent = True)

plt.show()

```


    
![png](output_83_0.png)
    



```python
# This plot shows how every state has had to revise its budget and probably allocate more to mitigating the effect of COVID 19.
```


```python
percentages = merged_data['Confirmed'] / sum(merged_data['Confirmed'])
```


```python
explode_mask = np.array([merged_data['Confirmed'] < 3000]).astype(int).reshape(37,) 
```


```python
explode = []
for i in range(len(explode_mask)):
    if i > 0:
        explode.append(2 * (i / 30.0))
    else: explode.append(0)

```


```python
plt.figure(figsize = (12, 10))
plt.title('Most Affected States by Percentage', fontsize = 18)
plt.pie(percentages, labels = merged_data['State'], colors = theme)
plt.legend()
#plt.savefig('Most Affected States by Percentage', dpi = 500, transparent = True)
plt.show()

```


    
![png](output_88_0.png)
    



```python
# This shows that Lagos State undoubtedly saw the biggest outburst of the virus. Followed by FCT, Rivers and Kaduna. It's no surprise that
# these cities are the most industrial in the country.

```

### TODO L - 
Determine the effect of the Pandemic on the economy. To do this, you will compare the Real GDP value Pre-COVID-19 with Real GDP in 2020 (COVID-19 Period, especially Q2 2020)
<br>
Steps
* From the Real GDP Data, generate a `barplot` using the GDP values for each year & quarters. For example: On x-axis you will have year 2017 and the bars will be values of each quarters(Q1-Q4). You expected to have subplots of each quarters on one graph.
<br>
Hint: Use [Pandas.melt](https://pandas.pydata.org/docs/reference/api/pandas.melt.html) to create your plot DataFrame 
* Set your quarter legend to lower left.
* Using `axhline`, draw a horizontal line through the graph at the value of Q2 2020.
* Write out your observation


```python
gdp_data = gdp_data.melt(id_vars = 'Year', value_vars = ['Q1', 'Q2', 'Q3', 'Q4'], var_name = 'Quarter', value_name = 'GDP Value')
```


```python
plt.figure(figsize = (10, 8))
plt.ylim(1.2e7, 2.0e7)
plt.title('GDP for Every Quarter Since 2014', fontsize = 18)
plt.axhline(gdp_data[gdp_data['Year'] == 2020][gdp_data['Quarter'] == 'Q2']['GDP Value'].values, color = 'black')

sns.barplot(x = 'Year', y = 'GDP Value', hue = 'Quarter', data = gdp_data, palette = theme)

plt.legend(loc = 'lower left')
#plt.savefig('GDP For Every Quarter Since 2014.png', dpi = 300, transparent = True)
plt.show()

```


    
![png](output_92_0.png)
    



```python
# The plot shows a general trend of rising GDP over the quarters of every year except for the year 2020.
# In 2020, we see what seemed like it would be a great year given that it had the highest GDP in the last 6 years.
# This GDP took a massive hit in the beginning of the second quarter when the virus was first confirmed in the country (~ April, 2020)
# The second quarter GDP of 2020 was at an all time low.

```



### Note: Do not limit your analysis to the provided TODOs. Perform more analyses e.g 
* Check for more external dataset
* Ask more questions & find the right answers by exploring the data


```python
vaccinations = pd.read_csv('https://raw.githubusercontent.com/owid/covid-19-data/master/public/data/vaccinations/vaccinations.csv')
```


```python
ng_vaccinations = vaccinations[vaccinations['location'] == 'Nigeria']
```


```python
ng_vaccinations = ng_vaccinations[['date', 'daily_vaccinations']]
```


```python
ng_vaccinations[100:103]
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
      <th>date</th>
      <th>daily_vaccinations</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>83303</th>
      <td>2021-06-12</td>
      <td>39399.0</td>
    </tr>
    <tr>
      <th>83304</th>
      <td>2021-06-13</td>
      <td>39399.0</td>
    </tr>
    <tr>
      <th>83305</th>
      <td>2021-06-14</td>
      <td>39399.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize = (12, 10))
plt.title('Daily Vaccinations Effect on Rate of Infection', fontsize = 18)

axis_1 = sns.lineplot(x = pd.to_datetime(ng_vaccinations['date']), y = ng_vaccinations['daily_vaccinations'], color = 'darkorange', markers = 'o')
axis_2 = axis_1.twinx()

sns.lineplot(x = ng_daily_confirmed.Date, y = daily_infection_rate, color = 'darkblue', markers = 'X')
#plt.savefig('Daily Vaccinations Effect on Rate of Infection', dpi = 300, transparent = True)

plt.show()

```


    
![png](output_100_0.png)
    



```python
# The daily vaccinations rolled out (in orange) shows that vaccination is effective for curbing the spread of the virus. The massive spike in 
# vaccination caused a flattening of the curve which was the goal.


```


