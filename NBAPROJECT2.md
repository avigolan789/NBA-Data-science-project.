```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
import matplotlib.pyplot as plt

import warnings
warnings.filterwarnings('ignore')
```

# Crawling data using BeautifulSoup package


```python
players=[]
for year in range(1980,2022):
    url = 'https://www.basketball-reference.com/leagues/NBA_{}_per_game.html'.format(year)
    
    r = requests.get(url)
    r_html = r.text
    soup = BeautifulSoup(r_html,'html.parser')

    table=soup.find_all(class_="full_table")
    
    """ Extracting List of column names"""
    head=soup.find(class_="thead")
    column_names_raw=[head.text for item in head][0]
    column_names_polished=column_names_raw.replace("\n",",").split(",")[2:-1]
    column_names_polished.insert(0, 'year')
    """Extracting full list of player_data"""
        
    for i in range(len(table)):
        
        player_=[year]        
        for td in table[i].find_all("td"):
            player_.append(td.text)


        players.append(player_)

```


```python
df1=pd.DataFrame(players, columns=column_names_polished)
#cleaning the player's name from occasional special characters
df1['Player']=df1['Player'].replace('*', '')
df1.head(10)

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
      <th>year</th>
      <th>Player</th>
      <th>Pos</th>
      <th>Age</th>
      <th>Tm</th>
      <th>G</th>
      <th>GS</th>
      <th>MP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>...</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1980</td>
      <td>Kareem Abdul-Jabbar*</td>
      <td>C</td>
      <td>32</td>
      <td>LAL</td>
      <td>82</td>
      <td></td>
      <td>38.3</td>
      <td>10.2</td>
      <td>16.9</td>
      <td>...</td>
      <td>.765</td>
      <td>2.3</td>
      <td>8.5</td>
      <td>10.8</td>
      <td>4.5</td>
      <td>1.0</td>
      <td>3.4</td>
      <td>3.6</td>
      <td>2.6</td>
      <td>24.8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1980</td>
      <td>Tom Abernethy</td>
      <td>PF</td>
      <td>25</td>
      <td>GSW</td>
      <td>67</td>
      <td></td>
      <td>18.2</td>
      <td>2.3</td>
      <td>4.7</td>
      <td>...</td>
      <td>.683</td>
      <td>0.9</td>
      <td>1.9</td>
      <td>2.9</td>
      <td>1.3</td>
      <td>0.5</td>
      <td>0.2</td>
      <td>0.6</td>
      <td>1.8</td>
      <td>5.4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1980</td>
      <td>Alvan Adams</td>
      <td>C</td>
      <td>25</td>
      <td>PHO</td>
      <td>75</td>
      <td></td>
      <td>28.9</td>
      <td>6.2</td>
      <td>11.7</td>
      <td>...</td>
      <td>.797</td>
      <td>2.1</td>
      <td>6.0</td>
      <td>8.1</td>
      <td>4.3</td>
      <td>1.4</td>
      <td>0.7</td>
      <td>2.9</td>
      <td>3.2</td>
      <td>14.9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1980</td>
      <td>Tiny Archibald*</td>
      <td>PG</td>
      <td>31</td>
      <td>BOS</td>
      <td>80</td>
      <td>80</td>
      <td>35.8</td>
      <td>4.8</td>
      <td>9.9</td>
      <td>...</td>
      <td>.830</td>
      <td>0.7</td>
      <td>1.7</td>
      <td>2.5</td>
      <td>8.4</td>
      <td>1.3</td>
      <td>0.1</td>
      <td>3.0</td>
      <td>2.7</td>
      <td>14.1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1980</td>
      <td>Dennis Awtrey</td>
      <td>C</td>
      <td>31</td>
      <td>CHI</td>
      <td>26</td>
      <td></td>
      <td>21.5</td>
      <td>1.0</td>
      <td>2.3</td>
      <td>...</td>
      <td>.640</td>
      <td>1.1</td>
      <td>3.3</td>
      <td>4.4</td>
      <td>1.5</td>
      <td>0.5</td>
      <td>0.6</td>
      <td>1.0</td>
      <td>2.5</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1980</td>
      <td>Gus Bailey</td>
      <td>SG</td>
      <td>28</td>
      <td>WSB</td>
      <td>20</td>
      <td></td>
      <td>9.0</td>
      <td>0.8</td>
      <td>1.8</td>
      <td>...</td>
      <td>.385</td>
      <td>0.3</td>
      <td>1.1</td>
      <td>1.4</td>
      <td>1.3</td>
      <td>0.4</td>
      <td>0.2</td>
      <td>0.6</td>
      <td>0.9</td>
      <td>1.9</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1980</td>
      <td>James Bailey</td>
      <td>PF</td>
      <td>22</td>
      <td>SEA</td>
      <td>67</td>
      <td></td>
      <td>10.8</td>
      <td>1.8</td>
      <td>4.0</td>
      <td>...</td>
      <td>.673</td>
      <td>1.1</td>
      <td>1.9</td>
      <td>2.9</td>
      <td>0.4</td>
      <td>0.3</td>
      <td>0.8</td>
      <td>1.2</td>
      <td>1.7</td>
      <td>4.7</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1980</td>
      <td>Greg Ballard</td>
      <td>SF</td>
      <td>25</td>
      <td>WSB</td>
      <td>82</td>
      <td></td>
      <td>29.7</td>
      <td>6.6</td>
      <td>13.4</td>
      <td>...</td>
      <td>.753</td>
      <td>2.9</td>
      <td>4.9</td>
      <td>7.8</td>
      <td>1.9</td>
      <td>1.1</td>
      <td>0.4</td>
      <td>1.6</td>
      <td>2.4</td>
      <td>15.6</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1980</td>
      <td>Mike Bantom</td>
      <td>SF</td>
      <td>28</td>
      <td>IND</td>
      <td>77</td>
      <td></td>
      <td>30.3</td>
      <td>5.0</td>
      <td>9.9</td>
      <td>...</td>
      <td>.665</td>
      <td>2.5</td>
      <td>3.4</td>
      <td>5.9</td>
      <td>3.6</td>
      <td>1.1</td>
      <td>0.6</td>
      <td>2.5</td>
      <td>3.5</td>
      <td>11.8</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1980</td>
      <td>Marvin Barnes</td>
      <td>PF</td>
      <td>27</td>
      <td>SDC</td>
      <td>20</td>
      <td></td>
      <td>14.4</td>
      <td>1.2</td>
      <td>3.0</td>
      <td>...</td>
      <td>.500</td>
      <td>1.7</td>
      <td>2.2</td>
      <td>3.9</td>
      <td>0.9</td>
      <td>0.3</td>
      <td>0.6</td>
      <td>0.9</td>
      <td>2.6</td>
      <td>3.2</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 30 columns</p>
</div>




```python
df1.shape
```




    (17683, 30)




```python
df1.to_csv('NBA.csv')
```

# Pre-processing

*Remove rows with missing values*


```python
df = df1[df1 != '']
df = df.dropna()
df = df.reset_index(drop = True)
df.head(10)

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
      <th>year</th>
      <th>Player</th>
      <th>Pos</th>
      <th>Age</th>
      <th>Tm</th>
      <th>G</th>
      <th>GS</th>
      <th>MP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>...</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1980</td>
      <td>Tiny Archibald*</td>
      <td>PG</td>
      <td>31</td>
      <td>BOS</td>
      <td>80</td>
      <td>80</td>
      <td>35.8</td>
      <td>4.8</td>
      <td>9.9</td>
      <td>...</td>
      <td>.830</td>
      <td>0.7</td>
      <td>1.7</td>
      <td>2.5</td>
      <td>8.4</td>
      <td>1.3</td>
      <td>0.1</td>
      <td>3.0</td>
      <td>2.7</td>
      <td>14.1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1980</td>
      <td>Henry Bibby</td>
      <td>PG</td>
      <td>30</td>
      <td>PHI</td>
      <td>82</td>
      <td>8</td>
      <td>24.8</td>
      <td>3.1</td>
      <td>7.6</td>
      <td>...</td>
      <td>.790</td>
      <td>0.8</td>
      <td>1.7</td>
      <td>2.5</td>
      <td>3.7</td>
      <td>0.8</td>
      <td>0.1</td>
      <td>1.8</td>
      <td>2.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1980</td>
      <td>Larry Bird*</td>
      <td>PF</td>
      <td>23</td>
      <td>BOS</td>
      <td>82</td>
      <td>82</td>
      <td>36.0</td>
      <td>8.5</td>
      <td>17.8</td>
      <td>...</td>
      <td>.836</td>
      <td>2.6</td>
      <td>7.8</td>
      <td>10.4</td>
      <td>4.5</td>
      <td>1.7</td>
      <td>0.6</td>
      <td>3.2</td>
      <td>3.4</td>
      <td>21.3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1980</td>
      <td>M.L. Carr</td>
      <td>SF</td>
      <td>29</td>
      <td>BOS</td>
      <td>82</td>
      <td>7</td>
      <td>24.3</td>
      <td>4.4</td>
      <td>9.3</td>
      <td>...</td>
      <td>.739</td>
      <td>1.3</td>
      <td>2.7</td>
      <td>4.0</td>
      <td>1.9</td>
      <td>1.5</td>
      <td>0.4</td>
      <td>1.7</td>
      <td>2.6</td>
      <td>11.1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1980</td>
      <td>Don Chaney</td>
      <td>SG</td>
      <td>33</td>
      <td>BOS</td>
      <td>60</td>
      <td>0</td>
      <td>8.7</td>
      <td>1.1</td>
      <td>3.2</td>
      <td>...</td>
      <td>.762</td>
      <td>0.5</td>
      <td>0.7</td>
      <td>1.2</td>
      <td>0.6</td>
      <td>0.5</td>
      <td>0.2</td>
      <td>0.6</td>
      <td>1.3</td>
      <td>2.8</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1980</td>
      <td>Maurice Cheeks*</td>
      <td>PG</td>
      <td>23</td>
      <td>PHI</td>
      <td>79</td>
      <td>79</td>
      <td>33.2</td>
      <td>4.5</td>
      <td>8.4</td>
      <td>...</td>
      <td>.779</td>
      <td>0.9</td>
      <td>2.5</td>
      <td>3.5</td>
      <td>7.0</td>
      <td>2.3</td>
      <td>0.4</td>
      <td>2.7</td>
      <td>2.5</td>
      <td>11.4</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1980</td>
      <td>Doug Collins</td>
      <td>SG</td>
      <td>28</td>
      <td>PHI</td>
      <td>36</td>
      <td>17</td>
      <td>26.8</td>
      <td>5.3</td>
      <td>11.4</td>
      <td>...</td>
      <td>.911</td>
      <td>0.8</td>
      <td>1.8</td>
      <td>2.6</td>
      <td>2.8</td>
      <td>0.8</td>
      <td>0.2</td>
      <td>2.3</td>
      <td>2.1</td>
      <td>13.8</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1980</td>
      <td>Dave Cowens*</td>
      <td>C</td>
      <td>31</td>
      <td>BOS</td>
      <td>66</td>
      <td>55</td>
      <td>32.7</td>
      <td>6.4</td>
      <td>14.1</td>
      <td>...</td>
      <td>.779</td>
      <td>1.9</td>
      <td>6.2</td>
      <td>8.1</td>
      <td>3.1</td>
      <td>1.0</td>
      <td>0.9</td>
      <td>1.6</td>
      <td>3.3</td>
      <td>14.2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1980</td>
      <td>Darryl Dawkins</td>
      <td>C</td>
      <td>23</td>
      <td>PHI</td>
      <td>80</td>
      <td>80</td>
      <td>31.8</td>
      <td>6.2</td>
      <td>11.8</td>
      <td>...</td>
      <td>.653</td>
      <td>2.5</td>
      <td>6.2</td>
      <td>8.7</td>
      <td>1.9</td>
      <td>0.6</td>
      <td>1.8</td>
      <td>2.9</td>
      <td>4.1</td>
      <td>14.7</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1980</td>
      <td>Julius Erving*</td>
      <td>SF</td>
      <td>29</td>
      <td>PHI</td>
      <td>78</td>
      <td>78</td>
      <td>36.1</td>
      <td>10.7</td>
      <td>20.7</td>
      <td>...</td>
      <td>.787</td>
      <td>2.8</td>
      <td>4.6</td>
      <td>7.4</td>
      <td>4.6</td>
      <td>2.2</td>
      <td>1.8</td>
      <td>3.6</td>
      <td>2.7</td>
      <td>26.9</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 30 columns</p>
</div>




```python
df.shape
```




    (14301, 30)



*Convert type to float*


```python
float_colname = ['G','GS','MP','FG','FGA','FG%','3P','3PA','3P%','2P','2PA','2P%','eFG%','FT','FTA','FT%','ORB','DRB','TRB','AST','STL','BLK','TOV','PF','PTS']
df[float_colname] = df[float_colname].astype(float)
```

# EDA

*Number of teams and positions*


```python
print('Number of teams:', len(df['Tm'].unique()))
print('Number of positions:', len(df['Pos'].unique()))
```

    Number of teams: 41
    Number of positions: 16
    


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
      <th>year</th>
      <th>G</th>
      <th>GS</th>
      <th>MP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>...</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>...</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
      <td>14301.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2003.385428</td>
      <td>58.162716</td>
      <td>29.116495</td>
      <td>22.078260</td>
      <td>3.515698</td>
      <td>7.746801</td>
      <td>0.442536</td>
      <td>0.547899</td>
      <td>1.581554</td>
      <td>0.259179</td>
      <td>...</td>
      <td>0.741934</td>
      <td>1.033515</td>
      <td>2.680421</td>
      <td>3.712663</td>
      <td>2.180092</td>
      <td>0.747165</td>
      <td>0.412628</td>
      <td>1.361737</td>
      <td>1.996350</td>
      <td>9.319530</td>
    </tr>
    <tr>
      <th>std</th>
      <td>11.478930</td>
      <td>22.427356</td>
      <td>29.980798</td>
      <td>9.621785</td>
      <td>2.259826</td>
      <td>4.650659</td>
      <td>0.067952</td>
      <td>0.667103</td>
      <td>1.746331</td>
      <td>0.167024</td>
      <td>...</td>
      <td>0.123698</td>
      <td>0.856679</td>
      <td>1.799926</td>
      <td>2.528378</td>
      <td>1.930771</td>
      <td>0.471741</td>
      <td>0.488424</td>
      <td>0.822177</td>
      <td>0.808547</td>
      <td>6.065372</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1980.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>0.300000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1994.000000</td>
      <td>44.000000</td>
      <td>2.000000</td>
      <td>14.200000</td>
      <td>1.700000</td>
      <td>4.000000</td>
      <td>0.407000</td>
      <td>0.000000</td>
      <td>0.100000</td>
      <td>0.154000</td>
      <td>...</td>
      <td>0.685000</td>
      <td>0.400000</td>
      <td>1.400000</td>
      <td>1.900000</td>
      <td>0.800000</td>
      <td>0.400000</td>
      <td>0.100000</td>
      <td>0.700000</td>
      <td>1.400000</td>
      <td>4.500000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2004.000000</td>
      <td>65.000000</td>
      <td>16.000000</td>
      <td>21.800000</td>
      <td>3.000000</td>
      <td>6.700000</td>
      <td>0.444000</td>
      <td>0.300000</td>
      <td>0.900000</td>
      <td>0.306000</td>
      <td>...</td>
      <td>0.760000</td>
      <td>0.800000</td>
      <td>2.300000</td>
      <td>3.100000</td>
      <td>1.600000</td>
      <td>0.700000</td>
      <td>0.300000</td>
      <td>1.200000</td>
      <td>2.000000</td>
      <td>7.900000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2014.000000</td>
      <td>77.000000</td>
      <td>58.000000</td>
      <td>30.300000</td>
      <td>4.900000</td>
      <td>10.700000</td>
      <td>0.481000</td>
      <td>0.900000</td>
      <td>2.600000</td>
      <td>0.366000</td>
      <td>...</td>
      <td>0.820000</td>
      <td>1.400000</td>
      <td>3.500000</td>
      <td>4.900000</td>
      <td>2.900000</td>
      <td>1.000000</td>
      <td>0.500000</td>
      <td>1.800000</td>
      <td>2.600000</td>
      <td>12.900000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2021.000000</td>
      <td>85.000000</td>
      <td>83.000000</td>
      <td>43.700000</td>
      <td>13.400000</td>
      <td>27.800000</td>
      <td>0.769000</td>
      <td>5.300000</td>
      <td>13.200000</td>
      <td>1.000000</td>
      <td>...</td>
      <td>1.000000</td>
      <td>6.900000</td>
      <td>12.300000</td>
      <td>18.700000</td>
      <td>14.500000</td>
      <td>3.700000</td>
      <td>5.000000</td>
      <td>5.700000</td>
      <td>5.000000</td>
      <td>37.100000</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 26 columns</p>
</div>




```python
df.shape
```




    (14301, 30)




```python
dfx = df.groupby('Tm').sum()
df10 = dfx.nlargest(10, 'PTS')
df10['PTS']
```




    Tm
    TOT    11526.3
    LAL     4698.8
    GSW     4527.0
    HOU     4499.0
    DEN     4470.6
    PHI     4464.9
    IND     4449.0
    DAL     4445.8
    BOS     4429.5
    SAS     4419.1
    Name: PTS, dtype: float64



## Top PTS by team


```python
dfx = df.groupby('Tm').sum()
df10 = dfx.nlargest(10, 'PTS')
df10[['PTS']].groupby(['Tm']).sum().plot.bar(figsize=(10, 5))
```




    <AxesSubplot:xlabel='Tm'>




    
![png](output_19_1.png)
    


## Top years that has the biggest amount of players


```python
dfx = df.groupby('year').count()
dfx
df10 = dfx.nlargest(10, 'Player')
# df10['PTS']
df10.plot.bar(y = 'Player', figsize=(10, 5))
```




    <AxesSubplot:xlabel='year'>




    
![png](output_21_1.png)
    


## Top PTS by Pos


```python
dfx = df.groupby('Pos').mean()
df10 = dfx.nlargest(10, 'PTS')
df10[['PTS']].groupby(['Pos']).sum().plot.bar(figsize=(10, 5))
```




    <AxesSubplot:xlabel='Pos'>




    
![png](output_23_1.png)
    


*Remove string columns*


```python
df_numerical = df.drop(['Pos', 'Tm'], axis = 1)
df_numerical.head()
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
      <th>year</th>
      <th>Player</th>
      <th>Age</th>
      <th>G</th>
      <th>GS</th>
      <th>MP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>...</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1980</td>
      <td>Tiny Archibald*</td>
      <td>31</td>
      <td>80.0</td>
      <td>80.0</td>
      <td>35.8</td>
      <td>4.8</td>
      <td>9.9</td>
      <td>0.482</td>
      <td>0.1</td>
      <td>...</td>
      <td>0.830</td>
      <td>0.7</td>
      <td>1.7</td>
      <td>2.5</td>
      <td>8.4</td>
      <td>1.3</td>
      <td>0.1</td>
      <td>3.0</td>
      <td>2.7</td>
      <td>14.1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1980</td>
      <td>Henry Bibby</td>
      <td>30</td>
      <td>82.0</td>
      <td>8.0</td>
      <td>24.8</td>
      <td>3.1</td>
      <td>7.6</td>
      <td>0.401</td>
      <td>0.1</td>
      <td>...</td>
      <td>0.790</td>
      <td>0.8</td>
      <td>1.7</td>
      <td>2.5</td>
      <td>3.7</td>
      <td>0.8</td>
      <td>0.1</td>
      <td>1.8</td>
      <td>2.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1980</td>
      <td>Larry Bird*</td>
      <td>23</td>
      <td>82.0</td>
      <td>82.0</td>
      <td>36.0</td>
      <td>8.5</td>
      <td>17.8</td>
      <td>0.474</td>
      <td>0.7</td>
      <td>...</td>
      <td>0.836</td>
      <td>2.6</td>
      <td>7.8</td>
      <td>10.4</td>
      <td>4.5</td>
      <td>1.7</td>
      <td>0.6</td>
      <td>3.2</td>
      <td>3.4</td>
      <td>21.3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1980</td>
      <td>M.L. Carr</td>
      <td>29</td>
      <td>82.0</td>
      <td>7.0</td>
      <td>24.3</td>
      <td>4.4</td>
      <td>9.3</td>
      <td>0.474</td>
      <td>0.1</td>
      <td>...</td>
      <td>0.739</td>
      <td>1.3</td>
      <td>2.7</td>
      <td>4.0</td>
      <td>1.9</td>
      <td>1.5</td>
      <td>0.4</td>
      <td>1.7</td>
      <td>2.6</td>
      <td>11.1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1980</td>
      <td>Don Chaney</td>
      <td>33</td>
      <td>60.0</td>
      <td>0.0</td>
      <td>8.7</td>
      <td>1.1</td>
      <td>3.2</td>
      <td>0.354</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.762</td>
      <td>0.5</td>
      <td>0.7</td>
      <td>1.2</td>
      <td>0.6</td>
      <td>0.5</td>
      <td>0.2</td>
      <td>0.6</td>
      <td>1.3</td>
      <td>2.8</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>




```python
df_numerical.shape
```




    (14301, 28)



# Regression Models

## Correlation Analysis


```python
# df2 =  df1[['rating', 'm_score', 'runtime', 'votes', 'gross']]
corr = df_numerical.corr()
corr.style.background_gradient(cmap='coolwarm').set_precision(2)
```




<style type="text/css">
#T_778ff_row0_col0, #T_778ff_row1_col1, #T_778ff_row2_col2, #T_778ff_row3_col3, #T_778ff_row4_col4, #T_778ff_row5_col5, #T_778ff_row6_col6, #T_778ff_row7_col7, #T_778ff_row8_col8, #T_778ff_row9_col9, #T_778ff_row10_col10, #T_778ff_row11_col11, #T_778ff_row12_col12, #T_778ff_row13_col13, #T_778ff_row14_col14, #T_778ff_row15_col15, #T_778ff_row16_col16, #T_778ff_row17_col17, #T_778ff_row18_col18, #T_778ff_row19_col19, #T_778ff_row20_col20, #T_778ff_row21_col21, #T_778ff_row22_col22, #T_778ff_row23_col23, #T_778ff_row24_col24, #T_778ff_row25_col25 {
  background-color: #b40426;
  color: #f1f1f1;
}
#T_778ff_row0_col1, #T_778ff_row0_col2, #T_778ff_row0_col3, #T_778ff_row0_col4, #T_778ff_row0_col5, #T_778ff_row0_col10, #T_778ff_row0_col11, #T_778ff_row0_col14, #T_778ff_row0_col15, #T_778ff_row0_col20, #T_778ff_row0_col21, #T_778ff_row0_col23, #T_778ff_row0_col24, #T_778ff_row0_col25, #T_778ff_row8_col6, #T_778ff_row9_col12, #T_778ff_row9_col17, #T_778ff_row9_col18, #T_778ff_row9_col19, #T_778ff_row9_col22, #T_778ff_row11_col0, #T_778ff_row16_col13, #T_778ff_row17_col7, #T_778ff_row17_col8, #T_778ff_row17_col9, #T_778ff_row17_col16 {
  background-color: #3b4cc0;
  color: #f1f1f1;
}
#T_778ff_row0_col6, #T_778ff_row7_col22 {
  background-color: #4b64d5;
  color: #f1f1f1;
}
#T_778ff_row0_col7, #T_778ff_row3_col7, #T_778ff_row17_col3, #T_778ff_row24_col6 {
  background-color: #e2dad5;
  color: #000000;
}
#T_778ff_row0_col8, #T_778ff_row5_col17, #T_778ff_row10_col20, #T_778ff_row10_col22, #T_778ff_row23_col17, #T_778ff_row24_col21, #T_778ff_row25_col7 {
  background-color: #e6d7cf;
  color: #000000;
}
#T_778ff_row0_col9, #T_778ff_row2_col7, #T_778ff_row2_col8, #T_778ff_row13_col19, #T_778ff_row20_col24, #T_778ff_row22_col3 {
  background-color: #c4d5f3;
  color: #000000;
}
#T_778ff_row0_col12, #T_778ff_row8_col19, #T_778ff_row11_col13, #T_778ff_row12_col16 {
  background-color: #5e7de7;
  color: #f1f1f1;
}
#T_778ff_row0_col13, #T_778ff_row8_col12 {
  background-color: #4a63d3;
  color: #f1f1f1;
}
#T_778ff_row0_col16, #T_778ff_row15_col13, #T_778ff_row20_col22 {
  background-color: #6788ee;
  color: #f1f1f1;
}
#T_778ff_row0_col17, #T_778ff_row15_col0 {
  background-color: #536edd;
  color: #f1f1f1;
}
#T_778ff_row0_col18, #T_778ff_row20_col12 {
  background-color: #6180e9;
  color: #f1f1f1;
}
#T_778ff_row0_col19, #T_778ff_row9_col15 {
  background-color: #5d7ce6;
  color: #f1f1f1;
}
#T_778ff_row0_col22, #T_778ff_row18_col16 {
  background-color: #6687ed;
  color: #f1f1f1;
}
#T_778ff_row1_col0, #T_778ff_row7_col6, #T_778ff_row22_col20 {
  background-color: #485fd1;
  color: #f1f1f1;
}
#T_778ff_row1_col2, #T_778ff_row19_col23, #T_778ff_row21_col15, #T_778ff_row25_col20 {
  background-color: #f3c7b1;
  color: #000000;
}
#T_778ff_row1_col3, #T_778ff_row14_col21, #T_778ff_row14_col24, #T_778ff_row20_col11, #T_778ff_row21_col14, #T_778ff_row22_col24 {
  background-color: #f3c8b2;
  color: #000000;
}
#T_778ff_row1_col4 {
  background-color: #e5d8d1;
  color: #000000;
}
#T_778ff_row1_col5, #T_778ff_row1_col19 {
  background-color: #dcdddd;
  color: #000000;
}
#T_778ff_row1_col6, #T_778ff_row1_col17, #T_778ff_row4_col6, #T_778ff_row6_col1 {
  background-color: #dddcdc;
  color: #000000;
}
#T_778ff_row1_col7, #T_778ff_row15_col7, #T_778ff_row16_col25 {
  background-color: #a9c6fd;
  color: #000000;
}
#T_778ff_row1_col8, #T_778ff_row8_col2, #T_778ff_row12_col23, #T_778ff_row13_col0, #T_778ff_row16_col4 {
  background-color: #a3c2fe;
  color: #000000;
}
#T_778ff_row1_col9, #T_778ff_row7_col15, #T_778ff_row22_col21 {
  background-color: #92b4fe;
  color: #000000;
}
#T_778ff_row1_col10, #T_778ff_row1_col11 {
  background-color: #edd2c3;
  color: #000000;
}
#T_778ff_row1_col12, #T_778ff_row6_col5, #T_778ff_row7_col2, #T_778ff_row8_col14, #T_778ff_row13_col23, #T_778ff_row16_col20, #T_778ff_row16_col23, #T_778ff_row21_col6 {
  background-color: #a5c3fe;
  color: #000000;
}
#T_778ff_row1_col13, #T_778ff_row12_col2, #T_778ff_row21_col22, #T_778ff_row22_col12 {
  background-color: #a1c0ff;
  color: #000000;
}
#T_778ff_row1_col14, #T_778ff_row6_col4 {
  background-color: #d3dbe7;
  color: #000000;
}
#T_778ff_row1_col15, #T_778ff_row7_col5, #T_778ff_row18_col21, #T_778ff_row20_col7, #T_778ff_row25_col6 {
  background-color: #d4dbe6;
  color: #000000;
}
#T_778ff_row1_col16, #T_778ff_row2_col12, #T_778ff_row3_col13, #T_778ff_row15_col16, #T_778ff_row18_col7, #T_778ff_row20_col19 {
  background-color: #97b8ff;
  color: #000000;
}
#T_778ff_row1_col18, #T_778ff_row17_col1, #T_778ff_row25_col22 {
  background-color: #d2dbe8;
  color: #000000;
}
#T_778ff_row1_col20, #T_778ff_row12_col4, #T_778ff_row14_col7, #T_778ff_row14_col16 {
  background-color: #b9d0f9;
  color: #000000;
}
#T_778ff_row1_col21, #T_778ff_row2_col22, #T_778ff_row3_col22, #T_778ff_row4_col8, #T_778ff_row14_col1, #T_778ff_row15_col1 {
  background-color: #d9dce1;
  color: #000000;
}
#T_778ff_row1_col22, #T_778ff_row6_col14, #T_778ff_row9_col0, #T_778ff_row13_col8, #T_778ff_row13_col25 {
  background-color: #bed2f6;
  color: #000000;
}
#T_778ff_row1_col23, #T_778ff_row7_col0, #T_778ff_row23_col1 {
  background-color: #e0dbd8;
  color: #000000;
}
#T_778ff_row1_col24, #T_778ff_row4_col17, #T_778ff_row4_col20 {
  background-color: #f2cbb7;
  color: #000000;
}
#T_778ff_row1_col25, #T_778ff_row6_col11 {
  background-color: #dedcdb;
  color: #000000;
}
#T_778ff_row2_col0, #T_778ff_row9_col2, #T_778ff_row16_col6, #T_778ff_row20_col0 {
  background-color: #6282ea;
  color: #f1f1f1;
}
#T_778ff_row2_col1, #T_778ff_row5_col18, #T_778ff_row11_col21, #T_778ff_row17_col11, #T_778ff_row19_col2 {
  background-color: #f6bea4;
  color: #000000;
}
#T_778ff_row2_col3, #T_778ff_row5_col15, #T_778ff_row6_col12, #T_778ff_row14_col5 {
  background-color: #e46e56;
  color: #f1f1f1;
}
#T_778ff_row2_col4, #T_778ff_row2_col10, #T_778ff_row4_col2 {
  background-color: #f29072;
  color: #f1f1f1;
}
#T_778ff_row2_col5, #T_778ff_row19_col24 {
  background-color: #f4987a;
  color: #000000;
}
#T_778ff_row2_col6, #T_778ff_row8_col3, #T_778ff_row12_col19, #T_778ff_row21_col7 {
  background-color: #cbd8ee;
  color: #000000;
}
#T_778ff_row2_col9, #T_778ff_row2_col16 {
  background-color: #93b5fe;
  color: #000000;
}
#T_778ff_row2_col11, #T_778ff_row14_col3, #T_778ff_row15_col3, #T_778ff_row23_col20 {
  background-color: #f29274;
  color: #f1f1f1;
}
#T_778ff_row2_col13, #T_778ff_row15_col9, #T_778ff_row16_col0 {
  background-color: #7ea1fa;
  color: #f1f1f1;
}
#T_778ff_row2_col14, #T_778ff_row21_col5, #T_778ff_row23_col24, #T_778ff_row25_col18 {
  background-color: #f7b497;
  color: #000000;
}
#T_778ff_row2_col15, #T_778ff_row4_col19, #T_778ff_row10_col17, #T_778ff_row11_col24, #T_778ff_row15_col19, #T_778ff_row18_col15, #T_778ff_row25_col21 {
  background-color: #f7b194;
  color: #000000;
}
#T_778ff_row2_col17, #T_778ff_row8_col9, #T_778ff_row24_col25 {
  background-color: #f1ccb8;
  color: #000000;
}
#T_778ff_row2_col18, #T_778ff_row2_col24, #T_778ff_row3_col20, #T_778ff_row4_col24, #T_778ff_row15_col2, #T_778ff_row18_col2, #T_778ff_row19_col4, #T_778ff_row25_col19 {
  background-color: #f7b89c;
  color: #000000;
}
#T_778ff_row2_col19, #T_778ff_row15_col18, #T_778ff_row17_col10, #T_778ff_row18_col25, #T_778ff_row21_col4 {
  background-color: #f7b79b;
  color: #000000;
}
#T_778ff_row2_col20, #T_778ff_row4_col1, #T_778ff_row6_col10, #T_778ff_row17_col6, #T_778ff_row20_col2, #T_778ff_row21_col24 {
  background-color: #edd1c2;
  color: #000000;
}
#T_778ff_row2_col21, #T_778ff_row3_col1, #T_778ff_row20_col3 {
  background-color: #f7bca1;
  color: #000000;
}
#T_778ff_row2_col23, #T_778ff_row21_col20 {
  background-color: #f5a081;
  color: #000000;
}
#T_778ff_row2_col25, #T_778ff_row5_col2 {
  background-color: #f39778;
  color: #000000;
}
#T_778ff_row3_col0, #T_778ff_row9_col10, #T_778ff_row22_col13, #T_778ff_row25_col0 {
  background-color: #6a8bef;
  color: #f1f1f1;
}
#T_778ff_row3_col2, #T_778ff_row3_col10, #T_778ff_row4_col23, #T_778ff_row23_col11 {
  background-color: #e36c55;
  color: #f1f1f1;
}
#T_778ff_row3_col4, #T_778ff_row3_col25, #T_778ff_row4_col3, #T_778ff_row15_col25, #T_778ff_row25_col3 {
  background-color: #d85646;
  color: #f1f1f1;
}
#T_778ff_row3_col5, #T_778ff_row5_col3 {
  background-color: #d75445;
  color: #f1f1f1;
}
#T_778ff_row3_col6, #T_778ff_row7_col25, #T_778ff_row16_col8, #T_778ff_row21_col19 {
  background-color: #cfdaea;
  color: #000000;
}
#T_778ff_row3_col8, #T_778ff_row8_col0, #T_778ff_row15_col20 {
  background-color: #e4d9d2;
  color: #000000;
}
#T_778ff_row3_col9, #T_778ff_row16_col10, #T_778ff_row21_col9, #T_778ff_row24_col12, #T_778ff_row25_col12 {
  background-color: #a6c4fe;
  color: #000000;
}
#T_778ff_row3_col11, #T_778ff_row5_col14, #T_778ff_row10_col14, #T_778ff_row11_col14, #T_778ff_row25_col23 {
  background-color: #e26952;
  color: #f1f1f1;
}
#T_778ff_row3_col12, #T_778ff_row16_col1 {
  background-color: #9fbfff;
  color: #000000;
}
#T_778ff_row3_col14 {
  background-color: #f08b6e;
  color: #f1f1f1;
}
#T_778ff_row3_col15, #T_778ff_row20_col23 {
  background-color: #f08a6c;
  color: #f1f1f1;
}
#T_778ff_row3_col16, #T_778ff_row13_col2, #T_778ff_row16_col11 {
  background-color: #aec9fc;
  color: #000000;
}
#T_778ff_row3_col17, #T_778ff_row6_col17, #T_778ff_row9_col7, #T_778ff_row15_col21, #T_778ff_row20_col25, #T_778ff_row23_col18, #T_778ff_row23_col19, #T_778ff_row24_col22 {
  background-color: #f2c9b4;
  color: #000000;
}
#T_778ff_row3_col18, #T_778ff_row18_col24, #T_778ff_row23_col21, #T_778ff_row24_col17 {
  background-color: #f6a283;
  color: #000000;
}
#T_778ff_row3_col19, #T_778ff_row18_col11 {
  background-color: #f7a688;
  color: #000000;
}
#T_778ff_row3_col21 {
  background-color: #f39577;
  color: #000000;
}
#T_778ff_row3_col23, #T_778ff_row15_col23, #T_778ff_row23_col4, #T_778ff_row23_col5, #T_778ff_row23_col10 {
  background-color: #e67259;
  color: #f1f1f1;
}
#T_778ff_row3_col24 {
  background-color: #f49a7b;
  color: #000000;
}
#T_778ff_row4_col0, #T_778ff_row7_col19 {
  background-color: #5f7fe8;
  color: #f1f1f1;
}
#T_778ff_row4_col5, #T_778ff_row5_col4, #T_778ff_row5_col25, #T_778ff_row18_col19, #T_778ff_row25_col5 {
  background-color: #ba162b;
  color: #f1f1f1;
}
#T_778ff_row4_col7, #T_778ff_row8_col5, #T_778ff_row11_col6, #T_778ff_row22_col6 {
  background-color: #d7dce3;
  color: #000000;
}
#T_778ff_row4_col9, #T_778ff_row7_col14, #T_778ff_row12_col3, #T_778ff_row12_col14, #T_778ff_row13_col5, #T_778ff_row13_col14 {
  background-color: #a2c1ff;
  color: #000000;
}
#T_778ff_row4_col10 {
  background-color: #c12b30;
  color: #f1f1f1;
}
#T_778ff_row4_col11, #T_778ff_row10_col4 {
  background-color: #c32e31;
  color: #f1f1f1;
}
#T_778ff_row4_col12, #T_778ff_row4_col16, #T_778ff_row17_col12 {
  background-color: #afcafc;
  color: #000000;
}
#T_778ff_row4_col13, #T_778ff_row18_col20 {
  background-color: #9dbdff;
  color: #000000;
}
#T_778ff_row4_col14, #T_778ff_row10_col15, #T_778ff_row13_col6, #T_778ff_row14_col11 {
  background-color: #df634e;
  color: #f1f1f1;
}
#T_778ff_row4_col15, #T_778ff_row11_col15, #T_778ff_row12_col6, #T_778ff_row14_col10 {
  background-color: #e0654f;
  color: #f1f1f1;
}
#T_778ff_row4_col18, #T_778ff_row19_col3, #T_778ff_row24_col11, #T_778ff_row24_col18 {
  background-color: #f7b093;
  color: #000000;
}
#T_778ff_row4_col21, #T_778ff_row19_col15, #T_778ff_row21_col11 {
  background-color: #f7b396;
  color: #000000;
}
#T_778ff_row4_col22, #T_778ff_row22_col15 {
  background-color: #d8dce2;
  color: #000000;
}
#T_778ff_row4_col25, #T_778ff_row10_col11, #T_778ff_row11_col10, #T_778ff_row25_col4 {
  background-color: #b70d28;
  color: #f1f1f1;
}
#T_778ff_row5_col0, #T_778ff_row8_col13, #T_778ff_row9_col14 {
  background-color: #688aef;
  color: #f1f1f1;
}
#T_778ff_row5_col1, #T_778ff_row5_col7 {
  background-color: #e9d5cb;
  color: #000000;
}
#T_778ff_row5_col6, #T_778ff_row6_col23, #T_778ff_row8_col21, #T_778ff_row14_col8, #T_778ff_row23_col6, #T_778ff_row23_col7 {
  background-color: #bcd2f7;
  color: #000000;
}
#T_778ff_row5_col8, #T_778ff_row25_col1 {
  background-color: #ebd3c6;
  color: #000000;
}
#T_778ff_row5_col9, #T_778ff_row15_col8 {
  background-color: #adc9fd;
  color: #000000;
}
#T_778ff_row5_col10 {
  background-color: #cd423b;
  color: #f1f1f1;
}
#T_778ff_row5_col11 {
  background-color: #c83836;
  color: #f1f1f1;
}
#T_778ff_row5_col12, #T_778ff_row9_col20, #T_778ff_row19_col13 {
  background-color: #8badfd;
  color: #000000;
}
#T_778ff_row5_col13, #T_778ff_row9_col4, #T_778ff_row9_col23, #T_778ff_row12_col9, #T_778ff_row19_col7 {
  background-color: #779af7;
  color: #f1f1f1;
}
#T_778ff_row5_col16, #T_778ff_row7_col21, #T_778ff_row19_col12 {
  background-color: #b7cff9;
  color: #000000;
}
#T_778ff_row5_col19, #T_778ff_row10_col21, #T_778ff_row14_col19, #T_778ff_row15_col17, #T_778ff_row19_col25 {
  background-color: #f5c1a9;
  color: #000000;
}
#T_778ff_row5_col20, #T_778ff_row14_col18, #T_778ff_row19_col14, #T_778ff_row24_col4 {
  background-color: #f5c4ac;
  color: #000000;
}
#T_778ff_row5_col21, #T_778ff_row11_col19 {
  background-color: #f7ad90;
  color: #000000;
}
#T_778ff_row5_col22 {
  background-color: #c9d7f0;
  color: #000000;
}
#T_778ff_row5_col23 {
  background-color: #e36b54;
  color: #f1f1f1;
}
#T_778ff_row5_col24, #T_778ff_row15_col24, #T_778ff_row18_col14, #T_778ff_row18_col23 {
  background-color: #f6bfa6;
  color: #000000;
}
#T_778ff_row6_col0, #T_778ff_row6_col7, #T_778ff_row6_col16, #T_778ff_row19_col9, #T_778ff_row23_col13 {
  background-color: #5a78e4;
  color: #f1f1f1;
}
#T_778ff_row6_col2, #T_778ff_row6_col3, #T_778ff_row7_col20, #T_778ff_row12_col18, #T_778ff_row13_col3, #T_778ff_row13_col18, #T_778ff_row22_col1, #T_778ff_row22_col25 {
  background-color: #bbd1f8;
  color: #000000;
}
#T_778ff_row6_col8, #T_778ff_row7_col12, #T_778ff_row22_col9 {
  background-color: #4e68d8;
  color: #f1f1f1;
}
#T_778ff_row6_col9, #T_778ff_row9_col3, #T_778ff_row9_col11, #T_778ff_row9_col13, #T_778ff_row19_col0 {
  background-color: #7295f4;
  color: #f1f1f1;
}
#T_778ff_row6_col13, #T_778ff_row14_col23, #T_778ff_row23_col15 {
  background-color: #e8765c;
  color: #f1f1f1;
}
#T_778ff_row6_col15, #T_778ff_row7_col3, #T_778ff_row20_col1, #T_778ff_row22_col2, #T_778ff_row22_col4 {
  background-color: #cad8ef;
  color: #000000;
}
#T_778ff_row6_col18, #T_778ff_row12_col17 {
  background-color: #d6dce4;
  color: #000000;
}
#T_778ff_row6_col19, #T_778ff_row11_col1, #T_778ff_row22_col10, #T_778ff_row25_col8 {
  background-color: #e8d6cc;
  color: #000000;
}
#T_778ff_row6_col20, #T_778ff_row22_col0 {
  background-color: #7093f3;
  color: #f1f1f1;
}
#T_778ff_row6_col21, #T_778ff_row16_col3, #T_778ff_row25_col13 {
  background-color: #9abbff;
  color: #000000;
}
#T_778ff_row6_col22, #T_778ff_row20_col8 {
  background-color: #dadce0;
  color: #000000;
}
#T_778ff_row6_col24 {
  background-color: #e7d7ce;
  color: #000000;
}
#T_778ff_row6_col25, #T_778ff_row7_col4, #T_778ff_row8_col4, #T_778ff_row8_col20 {
  background-color: #c1d4f4;
  color: #000000;
}
#T_778ff_row7_col1, #T_778ff_row11_col16, #T_778ff_row20_col18 {
  background-color: #9bbcff;
  color: #000000;
}
#T_778ff_row7_col8, #T_778ff_row8_col7, #T_778ff_row14_col15, #T_778ff_row15_col14 {
  background-color: #b8122a;
  color: #f1f1f1;
}
#T_778ff_row7_col9, #T_778ff_row20_col5, #T_778ff_row24_col15 {
  background-color: #f4c6af;
  color: #000000;
}
#T_778ff_row7_col10, #T_778ff_row9_col21 {
  background-color: #85a8fc;
  color: #f1f1f1;
}
#T_778ff_row7_col11, #T_778ff_row14_col12 {
  background-color: #8db0fe;
  color: #000000;
}
#T_778ff_row7_col13, #T_778ff_row12_col7, #T_778ff_row20_col17, #T_778ff_row23_col12, #T_778ff_row24_col7, #T_778ff_row24_col8 {
  background-color: #82a6fb;
  color: #f1f1f1;
}
#T_778ff_row7_col16, #T_778ff_row12_col1, #T_778ff_row12_col22 {
  background-color: #c0d4f5;
  color: #000000;
}
#T_778ff_row7_col17, #T_778ff_row16_col12 {
  background-color: #4257c9;
  color: #f1f1f1;
}
#T_778ff_row7_col18, #T_778ff_row8_col18 {
  background-color: #6f92f3;
  color: #f1f1f1;
}
#T_778ff_row7_col23, #T_778ff_row20_col16, #T_778ff_row22_col5 {
  background-color: #b1cbfc;
  color: #000000;
}
#T_778ff_row7_col24 {
  background-color: #7da0f9;
  color: #f1f1f1;
}
#T_778ff_row8_col1, #T_778ff_row11_col8, #T_778ff_row15_col12 {
  background-color: #94b6ff;
  color: #000000;
}
#T_778ff_row8_col10, #T_778ff_row19_col20 {
  background-color: #86a9fc;
  color: #f1f1f1;
}
#T_778ff_row8_col11, #T_778ff_row10_col8, #T_778ff_row11_col7, #T_778ff_row11_col12, #T_778ff_row18_col13 {
  background-color: #90b2fe;
  color: #000000;
}
#T_778ff_row8_col15, #T_778ff_row10_col16, #T_778ff_row16_col15, #T_778ff_row16_col21 {
  background-color: #96b7ff;
  color: #000000;
}
#T_778ff_row8_col16 {
  background-color: #bfd3f6;
  color: #000000;
}
#T_778ff_row8_col17, #T_778ff_row10_col0, #T_778ff_row22_col16 {
  background-color: #3f53c6;
  color: #f1f1f1;
}
#T_778ff_row8_col22, #T_778ff_row17_col0, #T_778ff_row23_col0 {
  background-color: #4961d2;
  color: #f1f1f1;
}
#T_778ff_row8_col23, #T_778ff_row16_col14 {
  background-color: #b6cefa;
  color: #000000;
}
#T_778ff_row8_col24 {
  background-color: #7a9df8;
  color: #f1f1f1;
}
#T_778ff_row8_col25, #T_778ff_row12_col10, #T_778ff_row14_col22, #T_778ff_row16_col7, #T_778ff_row21_col8 {
  background-color: #d1dae9;
  color: #000000;
}
#T_778ff_row9_col1, #T_778ff_row9_col5, #T_778ff_row11_col9 {
  background-color: #7b9ff9;
  color: #f1f1f1;
}
#T_778ff_row9_col6, #T_778ff_row16_col19, #T_778ff_row21_col13 {
  background-color: #5977e3;
  color: #f1f1f1;
}
#T_778ff_row9_col8, #T_778ff_row17_col15, #T_778ff_row20_col10, #T_778ff_row24_col1 {
  background-color: #f0cdbb;
  color: #000000;
}
#T_778ff_row9_col16, #T_778ff_row12_col15, #T_778ff_row25_col9 {
  background-color: #abc8fd;
  color: #000000;
}
#T_778ff_row9_col24, #T_778ff_row19_col16, #T_778ff_row21_col0 {
  background-color: #5673e0;
  color: #f1f1f1;
}
#T_778ff_row9_col25, #T_778ff_row10_col9, #T_778ff_row13_col20, #T_778ff_row19_col8, #T_778ff_row21_col12 {
  background-color: #799cf8;
  color: #f1f1f1;
}
#T_778ff_row10_col1, #T_778ff_row10_col6, #T_778ff_row14_col20, #T_778ff_row19_col6, #T_778ff_row20_col15 {
  background-color: #ead4c8;
  color: #000000;
}
#T_778ff_row10_col2, #T_778ff_row19_col10, #T_778ff_row20_col21, #T_778ff_row21_col3, #T_778ff_row21_col23 {
  background-color: #f59d7e;
  color: #000000;
}
#T_778ff_row10_col3 {
  background-color: #e9785d;
  color: #f1f1f1;
}
#T_778ff_row10_col5 {
  background-color: #d1493f;
  color: #f1f1f1;
}
#T_778ff_row10_col7, #T_778ff_row14_col9 {
  background-color: #8caffe;
  color: #000000;
}
#T_778ff_row10_col12, #T_778ff_row18_col12 {
  background-color: #b3cdfb;
  color: #000000;
}
#T_778ff_row10_col13, #T_778ff_row12_col8, #T_778ff_row13_col16 {
  background-color: #81a4fb;
  color: #f1f1f1;
}
#T_778ff_row10_col18 {
  background-color: #f7ac8e;
  color: #000000;
}
#T_778ff_row10_col19, #T_778ff_row18_col3, #T_778ff_row18_col22, #T_778ff_row22_col17 {
  background-color: #f6a385;
  color: #000000;
}
#T_778ff_row10_col23, #T_778ff_row11_col3, #T_778ff_row15_col5, #T_778ff_row18_col17 {
  background-color: #e7745b;
  color: #f1f1f1;
}
#T_778ff_row10_col24, #T_778ff_row18_col4, #T_778ff_row22_col18, #T_778ff_row24_col10 {
  background-color: #f7af91;
  color: #000000;
}
#T_778ff_row10_col25 {
  background-color: #cf453c;
  color: #f1f1f1;
}
#T_778ff_row11_col2, #T_778ff_row18_col10, #T_778ff_row22_col19, #T_778ff_row24_col19 {
  background-color: #f59f80;
  color: #000000;
}
#T_778ff_row11_col4 {
  background-color: #c53334;
  color: #f1f1f1;
}
#T_778ff_row11_col5, #T_778ff_row25_col11 {
  background-color: #cc403a;
  color: #f1f1f1;
}
#T_778ff_row11_col17 {
  background-color: #f7ba9f;
  color: #000000;
}
#T_778ff_row11_col18 {
  background-color: #f7b599;
  color: #000000;
}
#T_778ff_row11_col20 {
  background-color: #ead5c9;
  color: #000000;
}
#T_778ff_row11_col22, #T_778ff_row15_col22, #T_778ff_row18_col1, #T_778ff_row19_col1, #T_778ff_row21_col1 {
  background-color: #dfdbd9;
  color: #000000;
}
#T_778ff_row11_col23, #T_778ff_row23_col25 {
  background-color: #e57058;
  color: #f1f1f1;
}
#T_778ff_row11_col25, #T_778ff_row19_col17 {
  background-color: #d0473d;
  color: #f1f1f1;
}
#T_778ff_row12_col0, #T_778ff_row12_col5, #T_778ff_row12_col21, #T_778ff_row23_col9 {
  background-color: #8fb1fe;
  color: #000000;
}
#T_778ff_row12_col11, #T_778ff_row25_col16 {
  background-color: #bad0f8;
  color: #000000;
}
#T_778ff_row12_col13, #T_778ff_row25_col2 {
  background-color: #f39475;
  color: #000000;
}
#T_778ff_row12_col20, #T_778ff_row18_col9, #T_778ff_row24_col16 {
  background-color: #6c8ff1;
  color: #f1f1f1;
}
#T_778ff_row12_col24, #T_778ff_row13_col9, #T_778ff_row13_col10, #T_778ff_row13_col24, #T_778ff_row22_col14 {
  background-color: #c7d7f0;
  color: #000000;
}
#T_778ff_row12_col25, #T_778ff_row17_col21, #T_778ff_row24_col20 {
  background-color: #aac7fd;
  color: #000000;
}
#T_778ff_row13_col1, #T_778ff_row17_col25 {
  background-color: #d5dbe5;
  color: #000000;
}
#T_778ff_row13_col4, #T_778ff_row23_col22 {
  background-color: #c6d6f1;
  color: #000000;
}
#T_778ff_row13_col7, #T_778ff_row21_col18 {
  background-color: #cdd9ec;
  color: #000000;
}
#T_778ff_row13_col11 {
  background-color: #b2ccfb;
  color: #000000;
}
#T_778ff_row13_col12, #T_778ff_row17_col18 {
  background-color: #ee8468;
  color: #f1f1f1;
}
#T_778ff_row13_col15, #T_778ff_row16_col5 {
  background-color: #a7c5fe;
  color: #000000;
}
#T_778ff_row13_col17, #T_778ff_row14_col6, #T_778ff_row21_col17 {
  background-color: #c5d6f2;
  color: #000000;
}
#T_778ff_row13_col21, #T_778ff_row18_col8, #T_778ff_row21_col16 {
  background-color: #98b9ff;
  color: #000000;
}
#T_778ff_row13_col22, #T_778ff_row20_col9 {
  background-color: #b5cdfa;
  color: #000000;
}
#T_778ff_row14_col0, #T_778ff_row16_col18 {
  background-color: #5572df;
  color: #f1f1f1;
}
#T_778ff_row14_col2, #T_778ff_row21_col10, #T_778ff_row21_col25, #T_778ff_row24_col23 {
  background-color: #f7b99e;
  color: #000000;
}
#T_778ff_row14_col4, #T_778ff_row15_col4 {
  background-color: #e16751;
  color: #f1f1f1;
}
#T_778ff_row14_col13 {
  background-color: #6384eb;
  color: #f1f1f1;
}
#T_778ff_row14_col17, #T_778ff_row20_col14 {
  background-color: #eed0c0;
  color: #000000;
}
#T_778ff_row14_col25 {
  background-color: #d55042;
  color: #f1f1f1;
}
#T_778ff_row15_col6 {
  background-color: #cedaeb;
  color: #000000;
}
#T_778ff_row15_col10, #T_778ff_row15_col11 {
  background-color: #dd5f4b;
  color: #f1f1f1;
}
#T_778ff_row16_col2 {
  background-color: #84a7fc;
  color: #f1f1f1;
}
#T_778ff_row16_col9, #T_778ff_row22_col23, #T_778ff_row23_col8 {
  background-color: #c3d5f4;
  color: #000000;
}
#T_778ff_row16_col17 {
  background-color: #5b7ae5;
  color: #f1f1f1;
}
#T_778ff_row16_col22 {
  background-color: #4c66d6;
  color: #f1f1f1;
}
#T_778ff_row16_col24, #T_778ff_row24_col13 {
  background-color: #80a3fa;
  color: #f1f1f1;
}
#T_778ff_row17_col2, #T_778ff_row17_col14, #T_778ff_row18_col6 {
  background-color: #e1dad6;
  color: #000000;
}
#T_778ff_row17_col4, #T_778ff_row22_col11 {
  background-color: #e3d9d3;
  color: #000000;
}
#T_778ff_row17_col5, #T_778ff_row19_col21 {
  background-color: #ccd9ed;
  color: #000000;
}
#T_778ff_row17_col13 {
  background-color: #6e90f2;
  color: #f1f1f1;
}
#T_778ff_row17_col19, #T_778ff_row25_col14 {
  background-color: #d24b40;
  color: #f1f1f1;
}
#T_778ff_row17_col20 {
  background-color: #516ddb;
  color: #f1f1f1;
}
#T_778ff_row17_col22, #T_778ff_row24_col3 {
  background-color: #f7aa8c;
  color: #000000;
}
#T_778ff_row17_col23 {
  background-color: #dbdcde;
  color: #000000;
}
#T_778ff_row17_col24 {
  background-color: #f7a889;
  color: #000000;
}
#T_778ff_row18_col0, #T_778ff_row20_col6 {
  background-color: #88abfd;
  color: #000000;
}
#T_778ff_row18_col5, #T_778ff_row21_col2 {
  background-color: #f5c0a7;
  color: #000000;
}
#T_778ff_row19_col5, #T_778ff_row20_col4 {
  background-color: #f2cab5;
  color: #000000;
}
#T_778ff_row19_col11 {
  background-color: #f6a586;
  color: #000000;
}
#T_778ff_row19_col18 {
  background-color: #bb1b2c;
  color: #f1f1f1;
}
#T_778ff_row19_col22 {
  background-color: #f59c7d;
  color: #000000;
}
#T_778ff_row20_col13 {
  background-color: #4358cb;
  color: #f1f1f1;
}
#T_778ff_row22_col7, #T_778ff_row22_col8 {
  background-color: #5875e1;
  color: #f1f1f1;
}
#T_778ff_row23_col2 {
  background-color: #f7a98b;
  color: #000000;
}
#T_778ff_row23_col3 {
  background-color: #ea7b60;
  color: #f1f1f1;
}
#T_778ff_row23_col14 {
  background-color: #e97a5f;
  color: #f1f1f1;
}
#T_778ff_row23_col16 {
  background-color: #9ebeff;
  color: #000000;
}
#T_778ff_row24_col0 {
  background-color: #3e51c5;
  color: #f1f1f1;
}
#T_778ff_row24_col2 {
  background-color: #f4c5ad;
  color: #000000;
}
#T_778ff_row24_col5 {
  background-color: #efcebd;
  color: #000000;
}
#T_778ff_row24_col9 {
  background-color: #6485ec;
  color: #f1f1f1;
}
#T_778ff_row24_col14 {
  background-color: #efcfbf;
  color: #000000;
}
#T_778ff_row25_col10 {
  background-color: #cb3e38;
  color: #f1f1f1;
}
#T_778ff_row25_col15 {
  background-color: #d65244;
  color: #f1f1f1;
}
#T_778ff_row25_col17 {
  background-color: #ecd3c5;
  color: #000000;
}
#T_778ff_row25_col24 {
  background-color: #f6bda2;
  color: #000000;
}
</style>
<table id="T_778ff_">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th class="col_heading level0 col0" >year</th>
      <th class="col_heading level0 col1" >G</th>
      <th class="col_heading level0 col2" >GS</th>
      <th class="col_heading level0 col3" >MP</th>
      <th class="col_heading level0 col4" >FG</th>
      <th class="col_heading level0 col5" >FGA</th>
      <th class="col_heading level0 col6" >FG%</th>
      <th class="col_heading level0 col7" >3P</th>
      <th class="col_heading level0 col8" >3PA</th>
      <th class="col_heading level0 col9" >3P%</th>
      <th class="col_heading level0 col10" >2P</th>
      <th class="col_heading level0 col11" >2PA</th>
      <th class="col_heading level0 col12" >2P%</th>
      <th class="col_heading level0 col13" >eFG%</th>
      <th class="col_heading level0 col14" >FT</th>
      <th class="col_heading level0 col15" >FTA</th>
      <th class="col_heading level0 col16" >FT%</th>
      <th class="col_heading level0 col17" >ORB</th>
      <th class="col_heading level0 col18" >DRB</th>
      <th class="col_heading level0 col19" >TRB</th>
      <th class="col_heading level0 col20" >AST</th>
      <th class="col_heading level0 col21" >STL</th>
      <th class="col_heading level0 col22" >BLK</th>
      <th class="col_heading level0 col23" >TOV</th>
      <th class="col_heading level0 col24" >PF</th>
      <th class="col_heading level0 col25" >PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_778ff_level0_row0" class="row_heading level0 row0" >year</th>
      <td id="T_778ff_row0_col0" class="data row0 col0" >1.00</td>
      <td id="T_778ff_row0_col1" class="data row0 col1" >-0.19</td>
      <td id="T_778ff_row0_col2" class="data row0 col2" >-0.08</td>
      <td id="T_778ff_row0_col3" class="data row0 col3" >-0.06</td>
      <td id="T_778ff_row0_col4" class="data row0 col4" >-0.09</td>
      <td id="T_778ff_row0_col5" class="data row0 col5" >-0.06</td>
      <td id="T_778ff_row0_col6" class="data row0 col6" >-0.12</td>
      <td id="T_778ff_row0_col7" class="data row0 col7" >0.40</td>
      <td id="T_778ff_row0_col8" class="data row0 col8" >0.42</td>
      <td id="T_778ff_row0_col9" class="data row0 col9" >0.24</td>
      <td id="T_778ff_row0_col10" class="data row0 col10" >-0.22</td>
      <td id="T_778ff_row0_col11" class="data row0 col11" >-0.24</td>
      <td id="T_778ff_row0_col12" class="data row0 col12" >0.08</td>
      <td id="T_778ff_row0_col13" class="data row0 col13" >0.14</td>
      <td id="T_778ff_row0_col14" class="data row0 col14" >-0.13</td>
      <td id="T_778ff_row0_col15" class="data row0 col15" >-0.15</td>
      <td id="T_778ff_row0_col16" class="data row0 col16" >0.02</td>
      <td id="T_778ff_row0_col17" class="data row0 col17" >-0.19</td>
      <td id="T_778ff_row0_col18" class="data row0 col18" >0.05</td>
      <td id="T_778ff_row0_col19" class="data row0 col19" >-0.03</td>
      <td id="T_778ff_row0_col20" class="data row0 col20" >-0.09</td>
      <td id="T_778ff_row0_col21" class="data row0 col21" >-0.13</td>
      <td id="T_778ff_row0_col22" class="data row0 col22" >-0.04</td>
      <td id="T_778ff_row0_col23" class="data row0 col23" >-0.18</td>
      <td id="T_778ff_row0_col24" class="data row0 col24" >-0.23</td>
      <td id="T_778ff_row0_col25" class="data row0 col25" >-0.06</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row1" class="row_heading level0 row1" >G</th>
      <td id="T_778ff_row1_col0" class="data row1 col0" >-0.19</td>
      <td id="T_778ff_row1_col1" class="data row1 col1" >1.00</td>
      <td id="T_778ff_row1_col2" class="data row1 col2" >0.58</td>
      <td id="T_778ff_row1_col3" class="data row1 col3" >0.59</td>
      <td id="T_778ff_row1_col4" class="data row1 col4" >0.49</td>
      <td id="T_778ff_row1_col5" class="data row1 col5" >0.46</td>
      <td id="T_778ff_row1_col6" class="data row1 col6" >0.41</td>
      <td id="T_778ff_row1_col7" class="data row1 col7" >0.16</td>
      <td id="T_778ff_row1_col8" class="data row1 col8" >0.13</td>
      <td id="T_778ff_row1_col9" class="data row1 col9" >0.05</td>
      <td id="T_778ff_row1_col10" class="data row1 col10" >0.47</td>
      <td id="T_778ff_row1_col11" class="data row1 col11" >0.46</td>
      <td id="T_778ff_row1_col12" class="data row1 col12" >0.29</td>
      <td id="T_778ff_row1_col13" class="data row1 col13" >0.37</td>
      <td id="T_778ff_row1_col14" class="data row1 col14" >0.39</td>
      <td id="T_778ff_row1_col15" class="data row1 col15" >0.39</td>
      <td id="T_778ff_row1_col16" class="data row1 col16" >0.17</td>
      <td id="T_778ff_row1_col17" class="data row1 col17" >0.36</td>
      <td id="T_778ff_row1_col18" class="data row1 col18" >0.42</td>
      <td id="T_778ff_row1_col19" class="data row1 col19" >0.42</td>
      <td id="T_778ff_row1_col20" class="data row1 col20" >0.33</td>
      <td id="T_778ff_row1_col21" class="data row1 col21" >0.42</td>
      <td id="T_778ff_row1_col22" class="data row1 col22" >0.27</td>
      <td id="T_778ff_row1_col23" class="data row1 col23" >0.42</td>
      <td id="T_778ff_row1_col24" class="data row1 col24" >0.51</td>
      <td id="T_778ff_row1_col25" class="data row1 col25" >0.48</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row2" class="row_heading level0 row2" >GS</th>
      <td id="T_778ff_row2_col0" class="data row2 col0" >-0.08</td>
      <td id="T_778ff_row2_col1" class="data row2 col1" >0.58</td>
      <td id="T_778ff_row2_col2" class="data row2 col2" >1.00</td>
      <td id="T_778ff_row2_col3" class="data row2 col3" >0.84</td>
      <td id="T_778ff_row2_col4" class="data row2 col4" >0.75</td>
      <td id="T_778ff_row2_col5" class="data row2 col5" >0.74</td>
      <td id="T_778ff_row2_col6" class="data row2 col6" >0.33</td>
      <td id="T_778ff_row2_col7" class="data row2 col7" >0.26</td>
      <td id="T_778ff_row2_col8" class="data row2 col8" >0.25</td>
      <td id="T_778ff_row2_col9" class="data row2 col9" >0.05</td>
      <td id="T_778ff_row2_col10" class="data row2 col10" >0.72</td>
      <td id="T_778ff_row2_col11" class="data row2 col11" >0.71</td>
      <td id="T_778ff_row2_col12" class="data row2 col12" >0.25</td>
      <td id="T_778ff_row2_col13" class="data row2 col13" >0.29</td>
      <td id="T_778ff_row2_col14" class="data row2 col14" >0.63</td>
      <td id="T_778ff_row2_col15" class="data row2 col15" >0.64</td>
      <td id="T_778ff_row2_col16" class="data row2 col16" >0.16</td>
      <td id="T_778ff_row2_col17" class="data row2 col17" >0.48</td>
      <td id="T_778ff_row2_col18" class="data row2 col18" >0.64</td>
      <td id="T_778ff_row2_col19" class="data row2 col19" >0.62</td>
      <td id="T_778ff_row2_col20" class="data row2 col20" >0.53</td>
      <td id="T_778ff_row2_col21" class="data row2 col21" >0.61</td>
      <td id="T_778ff_row2_col22" class="data row2 col22" >0.38</td>
      <td id="T_778ff_row2_col23" class="data row2 col23" >0.68</td>
      <td id="T_778ff_row2_col24" class="data row2 col24" >0.59</td>
      <td id="T_778ff_row2_col25" class="data row2 col25" >0.74</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row3" class="row_heading level0 row3" >MP</th>
      <td id="T_778ff_row3_col0" class="data row3 col0" >-0.06</td>
      <td id="T_778ff_row3_col1" class="data row3 col1" >0.59</td>
      <td id="T_778ff_row3_col2" class="data row3 col2" >0.84</td>
      <td id="T_778ff_row3_col3" class="data row3 col3" >1.00</td>
      <td id="T_778ff_row3_col4" class="data row3 col4" >0.88</td>
      <td id="T_778ff_row3_col5" class="data row3 col5" >0.89</td>
      <td id="T_778ff_row3_col6" class="data row3 col6" >0.35</td>
      <td id="T_778ff_row3_col7" class="data row3 col7" >0.40</td>
      <td id="T_778ff_row3_col8" class="data row3 col8" >0.40</td>
      <td id="T_778ff_row3_col9" class="data row3 col9" >0.13</td>
      <td id="T_778ff_row3_col10" class="data row3 col10" >0.82</td>
      <td id="T_778ff_row3_col11" class="data row3 col11" >0.82</td>
      <td id="T_778ff_row3_col12" class="data row3 col12" >0.27</td>
      <td id="T_778ff_row3_col13" class="data row3 col13" >0.35</td>
      <td id="T_778ff_row3_col14" class="data row3 col14" >0.75</td>
      <td id="T_778ff_row3_col15" class="data row3 col15" >0.75</td>
      <td id="T_778ff_row3_col16" class="data row3 col16" >0.25</td>
      <td id="T_778ff_row3_col17" class="data row3 col17" >0.49</td>
      <td id="T_778ff_row3_col18" class="data row3 col18" >0.71</td>
      <td id="T_778ff_row3_col19" class="data row3 col19" >0.67</td>
      <td id="T_778ff_row3_col20" class="data row3 col20" >0.64</td>
      <td id="T_778ff_row3_col21" class="data row3 col21" >0.73</td>
      <td id="T_778ff_row3_col22" class="data row3 col22" >0.38</td>
      <td id="T_778ff_row3_col23" class="data row3 col23" >0.81</td>
      <td id="T_778ff_row3_col24" class="data row3 col24" >0.69</td>
      <td id="T_778ff_row3_col25" class="data row3 col25" >0.89</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row4" class="row_heading level0 row4" >FG</th>
      <td id="T_778ff_row4_col0" class="data row4 col0" >-0.09</td>
      <td id="T_778ff_row4_col1" class="data row4 col1" >0.49</td>
      <td id="T_778ff_row4_col2" class="data row4 col2" >0.75</td>
      <td id="T_778ff_row4_col3" class="data row4 col3" >0.88</td>
      <td id="T_778ff_row4_col4" class="data row4 col4" >1.00</td>
      <td id="T_778ff_row4_col5" class="data row4 col5" >0.98</td>
      <td id="T_778ff_row4_col6" class="data row4 col6" >0.41</td>
      <td id="T_778ff_row4_col7" class="data row4 col7" >0.35</td>
      <td id="T_778ff_row4_col8" class="data row4 col8" >0.35</td>
      <td id="T_778ff_row4_col9" class="data row4 col9" >0.11</td>
      <td id="T_778ff_row4_col10" class="data row4 col10" >0.96</td>
      <td id="T_778ff_row4_col11" class="data row4 col11" >0.95</td>
      <td id="T_778ff_row4_col12" class="data row4 col12" >0.32</td>
      <td id="T_778ff_row4_col13" class="data row4 col13" >0.36</td>
      <td id="T_778ff_row4_col14" class="data row4 col14" >0.85</td>
      <td id="T_778ff_row4_col15" class="data row4 col15" >0.85</td>
      <td id="T_778ff_row4_col16" class="data row4 col16" >0.25</td>
      <td id="T_778ff_row4_col17" class="data row4 col17" >0.48</td>
      <td id="T_778ff_row4_col18" class="data row4 col18" >0.66</td>
      <td id="T_778ff_row4_col19" class="data row4 col19" >0.64</td>
      <td id="T_778ff_row4_col20" class="data row4 col20" >0.56</td>
      <td id="T_778ff_row4_col21" class="data row4 col21" >0.64</td>
      <td id="T_778ff_row4_col22" class="data row4 col22" >0.38</td>
      <td id="T_778ff_row4_col23" class="data row4 col23" >0.82</td>
      <td id="T_778ff_row4_col24" class="data row4 col24" >0.59</td>
      <td id="T_778ff_row4_col25" class="data row4 col25" >0.99</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row5" class="row_heading level0 row5" >FGA</th>
      <td id="T_778ff_row5_col0" class="data row5 col0" >-0.06</td>
      <td id="T_778ff_row5_col1" class="data row5 col1" >0.46</td>
      <td id="T_778ff_row5_col2" class="data row5 col2" >0.74</td>
      <td id="T_778ff_row5_col3" class="data row5 col3" >0.89</td>
      <td id="T_778ff_row5_col4" class="data row5 col4" >0.98</td>
      <td id="T_778ff_row5_col5" class="data row5 col5" >1.00</td>
      <td id="T_778ff_row5_col6" class="data row5 col6" >0.28</td>
      <td id="T_778ff_row5_col7" class="data row5 col7" >0.43</td>
      <td id="T_778ff_row5_col8" class="data row5 col8" >0.44</td>
      <td id="T_778ff_row5_col9" class="data row5 col9" >0.15</td>
      <td id="T_778ff_row5_col10" class="data row5 col10" >0.91</td>
      <td id="T_778ff_row5_col11" class="data row5 col11" >0.93</td>
      <td id="T_778ff_row5_col12" class="data row5 col12" >0.21</td>
      <td id="T_778ff_row5_col13" class="data row5 col13" >0.27</td>
      <td id="T_778ff_row5_col14" class="data row5 col14" >0.84</td>
      <td id="T_778ff_row5_col15" class="data row5 col15" >0.82</td>
      <td id="T_778ff_row5_col16" class="data row5 col16" >0.28</td>
      <td id="T_778ff_row5_col17" class="data row5 col17" >0.40</td>
      <td id="T_778ff_row5_col18" class="data row5 col18" >0.62</td>
      <td id="T_778ff_row5_col19" class="data row5 col19" >0.58</td>
      <td id="T_778ff_row5_col20" class="data row5 col20" >0.60</td>
      <td id="T_778ff_row5_col21" class="data row5 col21" >0.66</td>
      <td id="T_778ff_row5_col22" class="data row5 col22" >0.31</td>
      <td id="T_778ff_row5_col23" class="data row5 col23" >0.83</td>
      <td id="T_778ff_row5_col24" class="data row5 col24" >0.56</td>
      <td id="T_778ff_row5_col25" class="data row5 col25" >0.98</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row6" class="row_heading level0 row6" >FG%</th>
      <td id="T_778ff_row6_col0" class="data row6 col0" >-0.12</td>
      <td id="T_778ff_row6_col1" class="data row6 col1" >0.41</td>
      <td id="T_778ff_row6_col2" class="data row6 col2" >0.33</td>
      <td id="T_778ff_row6_col3" class="data row6 col3" >0.35</td>
      <td id="T_778ff_row6_col4" class="data row6 col4" >0.41</td>
      <td id="T_778ff_row6_col5" class="data row6 col5" >0.28</td>
      <td id="T_778ff_row6_col6" class="data row6 col6" >1.00</td>
      <td id="T_778ff_row6_col7" class="data row6 col7" >-0.13</td>
      <td id="T_778ff_row6_col8" class="data row6 col8" >-0.18</td>
      <td id="T_778ff_row6_col9" class="data row6 col9" >-0.06</td>
      <td id="T_778ff_row6_col10" class="data row6 col10" >0.48</td>
      <td id="T_778ff_row6_col11" class="data row6 col11" >0.38</td>
      <td id="T_778ff_row6_col12" class="data row6 col12" >0.84</td>
      <td id="T_778ff_row6_col13" class="data row6 col13" >0.85</td>
      <td id="T_778ff_row6_col14" class="data row6 col14" >0.31</td>
      <td id="T_778ff_row6_col15" class="data row6 col15" >0.35</td>
      <td id="T_778ff_row6_col16" class="data row6 col16" >-0.03</td>
      <td id="T_778ff_row6_col17" class="data row6 col17" >0.49</td>
      <td id="T_778ff_row6_col18" class="data row6 col18" >0.43</td>
      <td id="T_778ff_row6_col19" class="data row6 col19" >0.47</td>
      <td id="T_778ff_row6_col20" class="data row6 col20" >0.10</td>
      <td id="T_778ff_row6_col21" class="data row6 col21" >0.19</td>
      <td id="T_778ff_row6_col22" class="data row6 col22" >0.38</td>
      <td id="T_778ff_row6_col23" class="data row6 col23" >0.28</td>
      <td id="T_778ff_row6_col24" class="data row6 col24" >0.43</td>
      <td id="T_778ff_row6_col25" class="data row6 col25" >0.37</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row7" class="row_heading level0 row7" >3P</th>
      <td id="T_778ff_row7_col0" class="data row7 col0" >0.40</td>
      <td id="T_778ff_row7_col1" class="data row7 col1" >0.16</td>
      <td id="T_778ff_row7_col2" class="data row7 col2" >0.26</td>
      <td id="T_778ff_row7_col3" class="data row7 col3" >0.40</td>
      <td id="T_778ff_row7_col4" class="data row7 col4" >0.35</td>
      <td id="T_778ff_row7_col5" class="data row7 col5" >0.43</td>
      <td id="T_778ff_row7_col6" class="data row7 col6" >-0.13</td>
      <td id="T_778ff_row7_col7" class="data row7 col7" >1.00</td>
      <td id="T_778ff_row7_col8" class="data row7 col8" >0.98</td>
      <td id="T_778ff_row7_col9" class="data row7 col9" >0.51</td>
      <td id="T_778ff_row7_col10" class="data row7 col10" >0.06</td>
      <td id="T_778ff_row7_col11" class="data row7 col11" >0.07</td>
      <td id="T_778ff_row7_col12" class="data row7 col12" >0.02</td>
      <td id="T_778ff_row7_col13" class="data row7 col13" >0.30</td>
      <td id="T_778ff_row7_col14" class="data row7 col14" >0.22</td>
      <td id="T_778ff_row7_col15" class="data row7 col15" >0.16</td>
      <td id="T_778ff_row7_col16" class="data row7 col16" >0.31</td>
      <td id="T_778ff_row7_col17" class="data row7 col17" >-0.26</td>
      <td id="T_778ff_row7_col18" class="data row7 col18" >0.10</td>
      <td id="T_778ff_row7_col19" class="data row7 col19" >-0.02</td>
      <td id="T_778ff_row7_col20" class="data row7 col20" >0.33</td>
      <td id="T_778ff_row7_col21" class="data row7 col21" >0.29</td>
      <td id="T_778ff_row7_col22" class="data row7 col22" >-0.14</td>
      <td id="T_778ff_row7_col23" class="data row7 col23" >0.23</td>
      <td id="T_778ff_row7_col24" class="data row7 col24" >0.02</td>
      <td id="T_778ff_row7_col25" class="data row7 col25" >0.42</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row8" class="row_heading level0 row8" >3PA</th>
      <td id="T_778ff_row8_col0" class="data row8 col0" >0.42</td>
      <td id="T_778ff_row8_col1" class="data row8 col1" >0.13</td>
      <td id="T_778ff_row8_col2" class="data row8 col2" >0.25</td>
      <td id="T_778ff_row8_col3" class="data row8 col3" >0.40</td>
      <td id="T_778ff_row8_col4" class="data row8 col4" >0.35</td>
      <td id="T_778ff_row8_col5" class="data row8 col5" >0.44</td>
      <td id="T_778ff_row8_col6" class="data row8 col6" >-0.18</td>
      <td id="T_778ff_row8_col7" class="data row8 col7" >0.98</td>
      <td id="T_778ff_row8_col8" class="data row8 col8" >1.00</td>
      <td id="T_778ff_row8_col9" class="data row8 col9" >0.48</td>
      <td id="T_778ff_row8_col10" class="data row8 col10" >0.06</td>
      <td id="T_778ff_row8_col11" class="data row8 col11" >0.08</td>
      <td id="T_778ff_row8_col12" class="data row8 col12" >0.01</td>
      <td id="T_778ff_row8_col13" class="data row8 col13" >0.23</td>
      <td id="T_778ff_row8_col14" class="data row8 col14" >0.23</td>
      <td id="T_778ff_row8_col15" class="data row8 col15" >0.17</td>
      <td id="T_778ff_row8_col16" class="data row8 col16" >0.31</td>
      <td id="T_778ff_row8_col17" class="data row8 col17" >-0.26</td>
      <td id="T_778ff_row8_col18" class="data row8 col18" >0.10</td>
      <td id="T_778ff_row8_col19" class="data row8 col19" >-0.02</td>
      <td id="T_778ff_row8_col20" class="data row8 col20" >0.35</td>
      <td id="T_778ff_row8_col21" class="data row8 col21" >0.31</td>
      <td id="T_778ff_row8_col22" class="data row8 col22" >-0.15</td>
      <td id="T_778ff_row8_col23" class="data row8 col23" >0.25</td>
      <td id="T_778ff_row8_col24" class="data row8 col24" >0.02</td>
      <td id="T_778ff_row8_col25" class="data row8 col25" >0.42</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row9" class="row_heading level0 row9" >3P%</th>
      <td id="T_778ff_row9_col0" class="data row9 col0" >0.24</td>
      <td id="T_778ff_row9_col1" class="data row9 col1" >0.05</td>
      <td id="T_778ff_row9_col2" class="data row9 col2" >0.05</td>
      <td id="T_778ff_row9_col3" class="data row9 col3" >0.13</td>
      <td id="T_778ff_row9_col4" class="data row9 col4" >0.11</td>
      <td id="T_778ff_row9_col5" class="data row9 col5" >0.15</td>
      <td id="T_778ff_row9_col6" class="data row9 col6" >-0.06</td>
      <td id="T_778ff_row9_col7" class="data row9 col7" >0.51</td>
      <td id="T_778ff_row9_col8" class="data row9 col8" >0.48</td>
      <td id="T_778ff_row9_col9" class="data row9 col9" >1.00</td>
      <td id="T_778ff_row9_col10" class="data row9 col10" >-0.04</td>
      <td id="T_778ff_row9_col11" class="data row9 col11" >-0.03</td>
      <td id="T_778ff_row9_col12" class="data row9 col12" >-0.04</td>
      <td id="T_778ff_row9_col13" class="data row9 col13" >0.26</td>
      <td id="T_778ff_row9_col14" class="data row9 col14" >0.03</td>
      <td id="T_778ff_row9_col15" class="data row9 col15" >-0.02</td>
      <td id="T_778ff_row9_col16" class="data row9 col16" >0.24</td>
      <td id="T_778ff_row9_col17" class="data row9 col17" >-0.29</td>
      <td id="T_778ff_row9_col18" class="data row9 col18" >-0.08</td>
      <td id="T_778ff_row9_col19" class="data row9 col19" >-0.16</td>
      <td id="T_778ff_row9_col20" class="data row9 col20" >0.18</td>
      <td id="T_778ff_row9_col21" class="data row9 col21" >0.13</td>
      <td id="T_778ff_row9_col22" class="data row9 col22" >-0.20</td>
      <td id="T_778ff_row9_col23" class="data row9 col23" >0.04</td>
      <td id="T_778ff_row9_col24" class="data row9 col24" >-0.12</td>
      <td id="T_778ff_row9_col25" class="data row9 col25" >0.15</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row10" class="row_heading level0 row10" >2P</th>
      <td id="T_778ff_row10_col0" class="data row10 col0" >-0.22</td>
      <td id="T_778ff_row10_col1" class="data row10 col1" >0.47</td>
      <td id="T_778ff_row10_col2" class="data row10 col2" >0.72</td>
      <td id="T_778ff_row10_col3" class="data row10 col3" >0.82</td>
      <td id="T_778ff_row10_col4" class="data row10 col4" >0.96</td>
      <td id="T_778ff_row10_col5" class="data row10 col5" >0.91</td>
      <td id="T_778ff_row10_col6" class="data row10 col6" >0.48</td>
      <td id="T_778ff_row10_col7" class="data row10 col7" >0.06</td>
      <td id="T_778ff_row10_col8" class="data row10 col8" >0.06</td>
      <td id="T_778ff_row10_col9" class="data row10 col9" >-0.04</td>
      <td id="T_778ff_row10_col10" class="data row10 col10" >1.00</td>
      <td id="T_778ff_row10_col11" class="data row10 col11" >0.99</td>
      <td id="T_778ff_row10_col12" class="data row10 col12" >0.33</td>
      <td id="T_778ff_row10_col13" class="data row10 col13" >0.29</td>
      <td id="T_778ff_row10_col14" class="data row10 col14" >0.84</td>
      <td id="T_778ff_row10_col15" class="data row10 col15" >0.85</td>
      <td id="T_778ff_row10_col16" class="data row10 col16" >0.17</td>
      <td id="T_778ff_row10_col17" class="data row10 col17" >0.59</td>
      <td id="T_778ff_row10_col18" class="data row10 col18" >0.68</td>
      <td id="T_778ff_row10_col19" class="data row10 col19" >0.68</td>
      <td id="T_778ff_row10_col20" class="data row10 col20" >0.50</td>
      <td id="T_778ff_row10_col21" class="data row10 col21" >0.59</td>
      <td id="T_778ff_row10_col22" class="data row10 col22" >0.44</td>
      <td id="T_778ff_row10_col23" class="data row10 col23" >0.80</td>
      <td id="T_778ff_row10_col24" class="data row10 col24" >0.62</td>
      <td id="T_778ff_row10_col25" class="data row10 col25" >0.92</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row11" class="row_heading level0 row11" >2PA</th>
      <td id="T_778ff_row11_col0" class="data row11 col0" >-0.24</td>
      <td id="T_778ff_row11_col1" class="data row11 col1" >0.46</td>
      <td id="T_778ff_row11_col2" class="data row11 col2" >0.71</td>
      <td id="T_778ff_row11_col3" class="data row11 col3" >0.82</td>
      <td id="T_778ff_row11_col4" class="data row11 col4" >0.95</td>
      <td id="T_778ff_row11_col5" class="data row11 col5" >0.93</td>
      <td id="T_778ff_row11_col6" class="data row11 col6" >0.38</td>
      <td id="T_778ff_row11_col7" class="data row11 col7" >0.07</td>
      <td id="T_778ff_row11_col8" class="data row11 col8" >0.08</td>
      <td id="T_778ff_row11_col9" class="data row11 col9" >-0.03</td>
      <td id="T_778ff_row11_col10" class="data row11 col10" >0.99</td>
      <td id="T_778ff_row11_col11" class="data row11 col11" >1.00</td>
      <td id="T_778ff_row11_col12" class="data row11 col12" >0.23</td>
      <td id="T_778ff_row11_col13" class="data row11 col13" >0.20</td>
      <td id="T_778ff_row11_col14" class="data row11 col14" >0.84</td>
      <td id="T_778ff_row11_col15" class="data row11 col15" >0.85</td>
      <td id="T_778ff_row11_col16" class="data row11 col16" >0.19</td>
      <td id="T_778ff_row11_col17" class="data row11 col17" >0.56</td>
      <td id="T_778ff_row11_col18" class="data row11 col18" >0.65</td>
      <td id="T_778ff_row11_col19" class="data row11 col19" >0.65</td>
      <td id="T_778ff_row11_col20" class="data row11 col20" >0.52</td>
      <td id="T_778ff_row11_col21" class="data row11 col21" >0.60</td>
      <td id="T_778ff_row11_col22" class="data row11 col22" >0.41</td>
      <td id="T_778ff_row11_col23" class="data row11 col23" >0.81</td>
      <td id="T_778ff_row11_col24" class="data row11 col24" >0.62</td>
      <td id="T_778ff_row11_col25" class="data row11 col25" >0.92</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row12" class="row_heading level0 row12" >2P%</th>
      <td id="T_778ff_row12_col0" class="data row12 col0" >0.08</td>
      <td id="T_778ff_row12_col1" class="data row12 col1" >0.29</td>
      <td id="T_778ff_row12_col2" class="data row12 col2" >0.25</td>
      <td id="T_778ff_row12_col3" class="data row12 col3" >0.27</td>
      <td id="T_778ff_row12_col4" class="data row12 col4" >0.32</td>
      <td id="T_778ff_row12_col5" class="data row12 col5" >0.21</td>
      <td id="T_778ff_row12_col6" class="data row12 col6" >0.84</td>
      <td id="T_778ff_row12_col7" class="data row12 col7" >0.02</td>
      <td id="T_778ff_row12_col8" class="data row12 col8" >0.01</td>
      <td id="T_778ff_row12_col9" class="data row12 col9" >-0.04</td>
      <td id="T_778ff_row12_col10" class="data row12 col10" >0.33</td>
      <td id="T_778ff_row12_col11" class="data row12 col11" >0.23</td>
      <td id="T_778ff_row12_col12" class="data row12 col12" >1.00</td>
      <td id="T_778ff_row12_col13" class="data row12 col13" >0.79</td>
      <td id="T_778ff_row12_col14" class="data row12 col14" >0.22</td>
      <td id="T_778ff_row12_col15" class="data row12 col15" >0.24</td>
      <td id="T_778ff_row12_col16" class="data row12 col16" >-0.02</td>
      <td id="T_778ff_row12_col17" class="data row12 col17" >0.32</td>
      <td id="T_778ff_row12_col18" class="data row12 col18" >0.33</td>
      <td id="T_778ff_row12_col19" class="data row12 col19" >0.35</td>
      <td id="T_778ff_row12_col20" class="data row12 col20" >0.08</td>
      <td id="T_778ff_row12_col21" class="data row12 col21" >0.16</td>
      <td id="T_778ff_row12_col22" class="data row12 col22" >0.28</td>
      <td id="T_778ff_row12_col23" class="data row12 col23" >0.19</td>
      <td id="T_778ff_row12_col24" class="data row12 col24" >0.29</td>
      <td id="T_778ff_row12_col25" class="data row12 col25" >0.29</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row13" class="row_heading level0 row13" >eFG%</th>
      <td id="T_778ff_row13_col0" class="data row13 col0" >0.14</td>
      <td id="T_778ff_row13_col1" class="data row13 col1" >0.37</td>
      <td id="T_778ff_row13_col2" class="data row13 col2" >0.29</td>
      <td id="T_778ff_row13_col3" class="data row13 col3" >0.35</td>
      <td id="T_778ff_row13_col4" class="data row13 col4" >0.36</td>
      <td id="T_778ff_row13_col5" class="data row13 col5" >0.27</td>
      <td id="T_778ff_row13_col6" class="data row13 col6" >0.85</td>
      <td id="T_778ff_row13_col7" class="data row13 col7" >0.30</td>
      <td id="T_778ff_row13_col8" class="data row13 col8" >0.23</td>
      <td id="T_778ff_row13_col9" class="data row13 col9" >0.26</td>
      <td id="T_778ff_row13_col10" class="data row13 col10" >0.29</td>
      <td id="T_778ff_row13_col11" class="data row13 col11" >0.20</td>
      <td id="T_778ff_row13_col12" class="data row13 col12" >0.79</td>
      <td id="T_778ff_row13_col13" class="data row13 col13" >1.00</td>
      <td id="T_778ff_row13_col14" class="data row13 col14" >0.22</td>
      <td id="T_778ff_row13_col15" class="data row13 col15" >0.23</td>
      <td id="T_778ff_row13_col16" class="data row13 col16" >0.10</td>
      <td id="T_778ff_row13_col17" class="data row13 col17" >0.25</td>
      <td id="T_778ff_row13_col18" class="data row13 col18" >0.33</td>
      <td id="T_778ff_row13_col19" class="data row13 col19" >0.32</td>
      <td id="T_778ff_row13_col20" class="data row13 col20" >0.12</td>
      <td id="T_778ff_row13_col21" class="data row13 col21" >0.19</td>
      <td id="T_778ff_row13_col22" class="data row13 col22" >0.23</td>
      <td id="T_778ff_row13_col23" class="data row13 col23" >0.19</td>
      <td id="T_778ff_row13_col24" class="data row13 col24" >0.29</td>
      <td id="T_778ff_row13_col25" class="data row13 col25" >0.36</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row14" class="row_heading level0 row14" >FT</th>
      <td id="T_778ff_row14_col0" class="data row14 col0" >-0.13</td>
      <td id="T_778ff_row14_col1" class="data row14 col1" >0.39</td>
      <td id="T_778ff_row14_col2" class="data row14 col2" >0.63</td>
      <td id="T_778ff_row14_col3" class="data row14 col3" >0.75</td>
      <td id="T_778ff_row14_col4" class="data row14 col4" >0.85</td>
      <td id="T_778ff_row14_col5" class="data row14 col5" >0.84</td>
      <td id="T_778ff_row14_col6" class="data row14 col6" >0.31</td>
      <td id="T_778ff_row14_col7" class="data row14 col7" >0.22</td>
      <td id="T_778ff_row14_col8" class="data row14 col8" >0.23</td>
      <td id="T_778ff_row14_col9" class="data row14 col9" >0.03</td>
      <td id="T_778ff_row14_col10" class="data row14 col10" >0.84</td>
      <td id="T_778ff_row14_col11" class="data row14 col11" >0.84</td>
      <td id="T_778ff_row14_col12" class="data row14 col12" >0.22</td>
      <td id="T_778ff_row14_col13" class="data row14 col13" >0.22</td>
      <td id="T_778ff_row14_col14" class="data row14 col14" >1.00</td>
      <td id="T_778ff_row14_col15" class="data row14 col15" >0.98</td>
      <td id="T_778ff_row14_col16" class="data row14 col16" >0.28</td>
      <td id="T_778ff_row14_col17" class="data row14 col17" >0.45</td>
      <td id="T_778ff_row14_col18" class="data row14 col18" >0.59</td>
      <td id="T_778ff_row14_col19" class="data row14 col19" >0.58</td>
      <td id="T_778ff_row14_col20" class="data row14 col20" >0.52</td>
      <td id="T_778ff_row14_col21" class="data row14 col21" >0.56</td>
      <td id="T_778ff_row14_col22" class="data row14 col22" >0.35</td>
      <td id="T_778ff_row14_col23" class="data row14 col23" >0.80</td>
      <td id="T_778ff_row14_col24" class="data row14 col24" >0.52</td>
      <td id="T_778ff_row14_col25" class="data row14 col25" >0.90</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row15" class="row_heading level0 row15" >FTA</th>
      <td id="T_778ff_row15_col0" class="data row15 col0" >-0.15</td>
      <td id="T_778ff_row15_col1" class="data row15 col1" >0.39</td>
      <td id="T_778ff_row15_col2" class="data row15 col2" >0.64</td>
      <td id="T_778ff_row15_col3" class="data row15 col3" >0.75</td>
      <td id="T_778ff_row15_col4" class="data row15 col4" >0.85</td>
      <td id="T_778ff_row15_col5" class="data row15 col5" >0.82</td>
      <td id="T_778ff_row15_col6" class="data row15 col6" >0.35</td>
      <td id="T_778ff_row15_col7" class="data row15 col7" >0.16</td>
      <td id="T_778ff_row15_col8" class="data row15 col8" >0.17</td>
      <td id="T_778ff_row15_col9" class="data row15 col9" >-0.02</td>
      <td id="T_778ff_row15_col10" class="data row15 col10" >0.85</td>
      <td id="T_778ff_row15_col11" class="data row15 col11" >0.85</td>
      <td id="T_778ff_row15_col12" class="data row15 col12" >0.24</td>
      <td id="T_778ff_row15_col13" class="data row15 col13" >0.23</td>
      <td id="T_778ff_row15_col14" class="data row15 col14" >0.98</td>
      <td id="T_778ff_row15_col15" class="data row15 col15" >1.00</td>
      <td id="T_778ff_row15_col16" class="data row15 col16" >0.17</td>
      <td id="T_778ff_row15_col17" class="data row15 col17" >0.53</td>
      <td id="T_778ff_row15_col18" class="data row15 col18" >0.64</td>
      <td id="T_778ff_row15_col19" class="data row15 col19" >0.64</td>
      <td id="T_778ff_row15_col20" class="data row15 col20" >0.49</td>
      <td id="T_778ff_row15_col21" class="data row15 col21" >0.56</td>
      <td id="T_778ff_row15_col22" class="data row15 col22" >0.41</td>
      <td id="T_778ff_row15_col23" class="data row15 col23" >0.81</td>
      <td id="T_778ff_row15_col24" class="data row15 col24" >0.56</td>
      <td id="T_778ff_row15_col25" class="data row15 col25" >0.89</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row16" class="row_heading level0 row16" >FT%</th>
      <td id="T_778ff_row16_col0" class="data row16 col0" >0.02</td>
      <td id="T_778ff_row16_col1" class="data row16 col1" >0.17</td>
      <td id="T_778ff_row16_col2" class="data row16 col2" >0.16</td>
      <td id="T_778ff_row16_col3" class="data row16 col3" >0.25</td>
      <td id="T_778ff_row16_col4" class="data row16 col4" >0.25</td>
      <td id="T_778ff_row16_col5" class="data row16 col5" >0.28</td>
      <td id="T_778ff_row16_col6" class="data row16 col6" >-0.03</td>
      <td id="T_778ff_row16_col7" class="data row16 col7" >0.31</td>
      <td id="T_778ff_row16_col8" class="data row16 col8" >0.31</td>
      <td id="T_778ff_row16_col9" class="data row16 col9" >0.24</td>
      <td id="T_778ff_row16_col10" class="data row16 col10" >0.17</td>
      <td id="T_778ff_row16_col11" class="data row16 col11" >0.19</td>
      <td id="T_778ff_row16_col12" class="data row16 col12" >-0.02</td>
      <td id="T_778ff_row16_col13" class="data row16 col13" >0.10</td>
      <td id="T_778ff_row16_col14" class="data row16 col14" >0.28</td>
      <td id="T_778ff_row16_col15" class="data row16 col15" >0.17</td>
      <td id="T_778ff_row16_col16" class="data row16 col16" >1.00</td>
      <td id="T_778ff_row16_col17" class="data row16 col17" >-0.15</td>
      <td id="T_778ff_row16_col18" class="data row16 col18" >0.01</td>
      <td id="T_778ff_row16_col19" class="data row16 col19" >-0.04</td>
      <td id="T_778ff_row16_col20" class="data row16 col20" >0.26</td>
      <td id="T_778ff_row16_col21" class="data row16 col21" >0.18</td>
      <td id="T_778ff_row16_col22" class="data row16 col22" >-0.13</td>
      <td id="T_778ff_row16_col23" class="data row16 col23" >0.19</td>
      <td id="T_778ff_row16_col24" class="data row16 col24" >0.03</td>
      <td id="T_778ff_row16_col25" class="data row16 col25" >0.29</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row17" class="row_heading level0 row17" >ORB</th>
      <td id="T_778ff_row17_col0" class="data row17 col0" >-0.19</td>
      <td id="T_778ff_row17_col1" class="data row17 col1" >0.36</td>
      <td id="T_778ff_row17_col2" class="data row17 col2" >0.48</td>
      <td id="T_778ff_row17_col3" class="data row17 col3" >0.49</td>
      <td id="T_778ff_row17_col4" class="data row17 col4" >0.48</td>
      <td id="T_778ff_row17_col5" class="data row17 col5" >0.40</td>
      <td id="T_778ff_row17_col6" class="data row17 col6" >0.49</td>
      <td id="T_778ff_row17_col7" class="data row17 col7" >-0.26</td>
      <td id="T_778ff_row17_col8" class="data row17 col8" >-0.26</td>
      <td id="T_778ff_row17_col9" class="data row17 col9" >-0.29</td>
      <td id="T_778ff_row17_col10" class="data row17 col10" >0.59</td>
      <td id="T_778ff_row17_col11" class="data row17 col11" >0.56</td>
      <td id="T_778ff_row17_col12" class="data row17 col12" >0.32</td>
      <td id="T_778ff_row17_col13" class="data row17 col13" >0.25</td>
      <td id="T_778ff_row17_col14" class="data row17 col14" >0.45</td>
      <td id="T_778ff_row17_col15" class="data row17 col15" >0.53</td>
      <td id="T_778ff_row17_col16" class="data row17 col16" >-0.15</td>
      <td id="T_778ff_row17_col17" class="data row17 col17" >1.00</td>
      <td id="T_778ff_row17_col18" class="data row17 col18" >0.78</td>
      <td id="T_778ff_row17_col19" class="data row17 col19" >0.90</td>
      <td id="T_778ff_row17_col20" class="data row17 col20" >-0.00</td>
      <td id="T_778ff_row17_col21" class="data row17 col21" >0.25</td>
      <td id="T_778ff_row17_col22" class="data row17 col22" >0.65</td>
      <td id="T_778ff_row17_col23" class="data row17 col23" >0.40</td>
      <td id="T_778ff_row17_col24" class="data row17 col24" >0.65</td>
      <td id="T_778ff_row17_col25" class="data row17 col25" >0.44</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row18" class="row_heading level0 row18" >DRB</th>
      <td id="T_778ff_row18_col0" class="data row18 col0" >0.05</td>
      <td id="T_778ff_row18_col1" class="data row18 col1" >0.42</td>
      <td id="T_778ff_row18_col2" class="data row18 col2" >0.64</td>
      <td id="T_778ff_row18_col3" class="data row18 col3" >0.71</td>
      <td id="T_778ff_row18_col4" class="data row18 col4" >0.66</td>
      <td id="T_778ff_row18_col5" class="data row18 col5" >0.62</td>
      <td id="T_778ff_row18_col6" class="data row18 col6" >0.43</td>
      <td id="T_778ff_row18_col7" class="data row18 col7" >0.10</td>
      <td id="T_778ff_row18_col8" class="data row18 col8" >0.10</td>
      <td id="T_778ff_row18_col9" class="data row18 col9" >-0.08</td>
      <td id="T_778ff_row18_col10" class="data row18 col10" >0.68</td>
      <td id="T_778ff_row18_col11" class="data row18 col11" >0.65</td>
      <td id="T_778ff_row18_col12" class="data row18 col12" >0.33</td>
      <td id="T_778ff_row18_col13" class="data row18 col13" >0.33</td>
      <td id="T_778ff_row18_col14" class="data row18 col14" >0.59</td>
      <td id="T_778ff_row18_col15" class="data row18 col15" >0.64</td>
      <td id="T_778ff_row18_col16" class="data row18 col16" >0.01</td>
      <td id="T_778ff_row18_col17" class="data row18 col17" >0.78</td>
      <td id="T_778ff_row18_col18" class="data row18 col18" >1.00</td>
      <td id="T_778ff_row18_col19" class="data row18 col19" >0.98</td>
      <td id="T_778ff_row18_col20" class="data row18 col20" >0.23</td>
      <td id="T_778ff_row18_col21" class="data row18 col21" >0.40</td>
      <td id="T_778ff_row18_col22" class="data row18 col22" >0.67</td>
      <td id="T_778ff_row18_col23" class="data row18 col23" >0.58</td>
      <td id="T_778ff_row18_col24" class="data row18 col24" >0.67</td>
      <td id="T_778ff_row18_col25" class="data row18 col25" >0.65</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row19" class="row_heading level0 row19" >TRB</th>
      <td id="T_778ff_row19_col0" class="data row19 col0" >-0.03</td>
      <td id="T_778ff_row19_col1" class="data row19 col1" >0.42</td>
      <td id="T_778ff_row19_col2" class="data row19 col2" >0.62</td>
      <td id="T_778ff_row19_col3" class="data row19 col3" >0.67</td>
      <td id="T_778ff_row19_col4" class="data row19 col4" >0.64</td>
      <td id="T_778ff_row19_col5" class="data row19 col5" >0.58</td>
      <td id="T_778ff_row19_col6" class="data row19 col6" >0.47</td>
      <td id="T_778ff_row19_col7" class="data row19 col7" >-0.02</td>
      <td id="T_778ff_row19_col8" class="data row19 col8" >-0.02</td>
      <td id="T_778ff_row19_col9" class="data row19 col9" >-0.16</td>
      <td id="T_778ff_row19_col10" class="data row19 col10" >0.68</td>
      <td id="T_778ff_row19_col11" class="data row19 col11" >0.65</td>
      <td id="T_778ff_row19_col12" class="data row19 col12" >0.35</td>
      <td id="T_778ff_row19_col13" class="data row19 col13" >0.32</td>
      <td id="T_778ff_row19_col14" class="data row19 col14" >0.58</td>
      <td id="T_778ff_row19_col15" class="data row19 col15" >0.64</td>
      <td id="T_778ff_row19_col16" class="data row19 col16" >-0.04</td>
      <td id="T_778ff_row19_col17" class="data row19 col17" >0.90</td>
      <td id="T_778ff_row19_col18" class="data row19 col18" >0.98</td>
      <td id="T_778ff_row19_col19" class="data row19 col19" >1.00</td>
      <td id="T_778ff_row19_col20" class="data row19 col20" >0.17</td>
      <td id="T_778ff_row19_col21" class="data row19 col21" >0.36</td>
      <td id="T_778ff_row19_col22" class="data row19 col22" >0.69</td>
      <td id="T_778ff_row19_col23" class="data row19 col23" >0.55</td>
      <td id="T_778ff_row19_col24" class="data row19 col24" >0.69</td>
      <td id="T_778ff_row19_col25" class="data row19 col25" >0.61</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row20" class="row_heading level0 row20" >AST</th>
      <td id="T_778ff_row20_col0" class="data row20 col0" >-0.09</td>
      <td id="T_778ff_row20_col1" class="data row20 col1" >0.33</td>
      <td id="T_778ff_row20_col2" class="data row20 col2" >0.53</td>
      <td id="T_778ff_row20_col3" class="data row20 col3" >0.64</td>
      <td id="T_778ff_row20_col4" class="data row20 col4" >0.56</td>
      <td id="T_778ff_row20_col5" class="data row20 col5" >0.60</td>
      <td id="T_778ff_row20_col6" class="data row20 col6" >0.10</td>
      <td id="T_778ff_row20_col7" class="data row20 col7" >0.33</td>
      <td id="T_778ff_row20_col8" class="data row20 col8" >0.35</td>
      <td id="T_778ff_row20_col9" class="data row20 col9" >0.18</td>
      <td id="T_778ff_row20_col10" class="data row20 col10" >0.50</td>
      <td id="T_778ff_row20_col11" class="data row20 col11" >0.52</td>
      <td id="T_778ff_row20_col12" class="data row20 col12" >0.08</td>
      <td id="T_778ff_row20_col13" class="data row20 col13" >0.12</td>
      <td id="T_778ff_row20_col14" class="data row20 col14" >0.52</td>
      <td id="T_778ff_row20_col15" class="data row20 col15" >0.49</td>
      <td id="T_778ff_row20_col16" class="data row20 col16" >0.26</td>
      <td id="T_778ff_row20_col17" class="data row20 col17" >-0.00</td>
      <td id="T_778ff_row20_col18" class="data row20 col18" >0.23</td>
      <td id="T_778ff_row20_col19" class="data row20 col19" >0.17</td>
      <td id="T_778ff_row20_col20" class="data row20 col20" >1.00</td>
      <td id="T_778ff_row20_col21" class="data row20 col21" >0.71</td>
      <td id="T_778ff_row20_col22" class="data row20 col22" >-0.03</td>
      <td id="T_778ff_row20_col23" class="data row20 col23" >0.75</td>
      <td id="T_778ff_row20_col24" class="data row20 col24" >0.28</td>
      <td id="T_778ff_row20_col25" class="data row20 col25" >0.58</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row21" class="row_heading level0 row21" >STL</th>
      <td id="T_778ff_row21_col0" class="data row21 col0" >-0.13</td>
      <td id="T_778ff_row21_col1" class="data row21 col1" >0.42</td>
      <td id="T_778ff_row21_col2" class="data row21 col2" >0.61</td>
      <td id="T_778ff_row21_col3" class="data row21 col3" >0.73</td>
      <td id="T_778ff_row21_col4" class="data row21 col4" >0.64</td>
      <td id="T_778ff_row21_col5" class="data row21 col5" >0.66</td>
      <td id="T_778ff_row21_col6" class="data row21 col6" >0.19</td>
      <td id="T_778ff_row21_col7" class="data row21 col7" >0.29</td>
      <td id="T_778ff_row21_col8" class="data row21 col8" >0.31</td>
      <td id="T_778ff_row21_col9" class="data row21 col9" >0.13</td>
      <td id="T_778ff_row21_col10" class="data row21 col10" >0.59</td>
      <td id="T_778ff_row21_col11" class="data row21 col11" >0.60</td>
      <td id="T_778ff_row21_col12" class="data row21 col12" >0.16</td>
      <td id="T_778ff_row21_col13" class="data row21 col13" >0.19</td>
      <td id="T_778ff_row21_col14" class="data row21 col14" >0.56</td>
      <td id="T_778ff_row21_col15" class="data row21 col15" >0.56</td>
      <td id="T_778ff_row21_col16" class="data row21 col16" >0.18</td>
      <td id="T_778ff_row21_col17" class="data row21 col17" >0.25</td>
      <td id="T_778ff_row21_col18" class="data row21 col18" >0.40</td>
      <td id="T_778ff_row21_col19" class="data row21 col19" >0.36</td>
      <td id="T_778ff_row21_col20" class="data row21 col20" >0.71</td>
      <td id="T_778ff_row21_col21" class="data row21 col21" >1.00</td>
      <td id="T_778ff_row21_col22" class="data row21 col22" >0.16</td>
      <td id="T_778ff_row21_col23" class="data row21 col23" >0.69</td>
      <td id="T_778ff_row21_col24" class="data row21 col24" >0.47</td>
      <td id="T_778ff_row21_col25" class="data row21 col25" >0.64</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row22" class="row_heading level0 row22" >BLK</th>
      <td id="T_778ff_row22_col0" class="data row22 col0" >-0.04</td>
      <td id="T_778ff_row22_col1" class="data row22 col1" >0.27</td>
      <td id="T_778ff_row22_col2" class="data row22 col2" >0.38</td>
      <td id="T_778ff_row22_col3" class="data row22 col3" >0.38</td>
      <td id="T_778ff_row22_col4" class="data row22 col4" >0.38</td>
      <td id="T_778ff_row22_col5" class="data row22 col5" >0.31</td>
      <td id="T_778ff_row22_col6" class="data row22 col6" >0.38</td>
      <td id="T_778ff_row22_col7" class="data row22 col7" >-0.14</td>
      <td id="T_778ff_row22_col8" class="data row22 col8" >-0.15</td>
      <td id="T_778ff_row22_col9" class="data row22 col9" >-0.20</td>
      <td id="T_778ff_row22_col10" class="data row22 col10" >0.44</td>
      <td id="T_778ff_row22_col11" class="data row22 col11" >0.41</td>
      <td id="T_778ff_row22_col12" class="data row22 col12" >0.28</td>
      <td id="T_778ff_row22_col13" class="data row22 col13" >0.23</td>
      <td id="T_778ff_row22_col14" class="data row22 col14" >0.35</td>
      <td id="T_778ff_row22_col15" class="data row22 col15" >0.41</td>
      <td id="T_778ff_row22_col16" class="data row22 col16" >-0.13</td>
      <td id="T_778ff_row22_col17" class="data row22 col17" >0.65</td>
      <td id="T_778ff_row22_col18" class="data row22 col18" >0.67</td>
      <td id="T_778ff_row22_col19" class="data row22 col19" >0.69</td>
      <td id="T_778ff_row22_col20" class="data row22 col20" >-0.03</td>
      <td id="T_778ff_row22_col21" class="data row22 col21" >0.16</td>
      <td id="T_778ff_row22_col22" class="data row22 col22" >1.00</td>
      <td id="T_778ff_row22_col23" class="data row22 col23" >0.30</td>
      <td id="T_778ff_row22_col24" class="data row22 col24" >0.52</td>
      <td id="T_778ff_row22_col25" class="data row22 col25" >0.35</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row23" class="row_heading level0 row23" >TOV</th>
      <td id="T_778ff_row23_col0" class="data row23 col0" >-0.18</td>
      <td id="T_778ff_row23_col1" class="data row23 col1" >0.42</td>
      <td id="T_778ff_row23_col2" class="data row23 col2" >0.68</td>
      <td id="T_778ff_row23_col3" class="data row23 col3" >0.81</td>
      <td id="T_778ff_row23_col4" class="data row23 col4" >0.82</td>
      <td id="T_778ff_row23_col5" class="data row23 col5" >0.83</td>
      <td id="T_778ff_row23_col6" class="data row23 col6" >0.28</td>
      <td id="T_778ff_row23_col7" class="data row23 col7" >0.23</td>
      <td id="T_778ff_row23_col8" class="data row23 col8" >0.25</td>
      <td id="T_778ff_row23_col9" class="data row23 col9" >0.04</td>
      <td id="T_778ff_row23_col10" class="data row23 col10" >0.80</td>
      <td id="T_778ff_row23_col11" class="data row23 col11" >0.81</td>
      <td id="T_778ff_row23_col12" class="data row23 col12" >0.19</td>
      <td id="T_778ff_row23_col13" class="data row23 col13" >0.19</td>
      <td id="T_778ff_row23_col14" class="data row23 col14" >0.80</td>
      <td id="T_778ff_row23_col15" class="data row23 col15" >0.81</td>
      <td id="T_778ff_row23_col16" class="data row23 col16" >0.19</td>
      <td id="T_778ff_row23_col17" class="data row23 col17" >0.40</td>
      <td id="T_778ff_row23_col18" class="data row23 col18" >0.58</td>
      <td id="T_778ff_row23_col19" class="data row23 col19" >0.55</td>
      <td id="T_778ff_row23_col20" class="data row23 col20" >0.75</td>
      <td id="T_778ff_row23_col21" class="data row23 col21" >0.69</td>
      <td id="T_778ff_row23_col22" class="data row23 col22" >0.30</td>
      <td id="T_778ff_row23_col23" class="data row23 col23" >1.00</td>
      <td id="T_778ff_row23_col24" class="data row23 col24" >0.60</td>
      <td id="T_778ff_row23_col25" class="data row23 col25" >0.83</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row24" class="row_heading level0 row24" >PF</th>
      <td id="T_778ff_row24_col0" class="data row24 col0" >-0.23</td>
      <td id="T_778ff_row24_col1" class="data row24 col1" >0.51</td>
      <td id="T_778ff_row24_col2" class="data row24 col2" >0.59</td>
      <td id="T_778ff_row24_col3" class="data row24 col3" >0.69</td>
      <td id="T_778ff_row24_col4" class="data row24 col4" >0.59</td>
      <td id="T_778ff_row24_col5" class="data row24 col5" >0.56</td>
      <td id="T_778ff_row24_col6" class="data row24 col6" >0.43</td>
      <td id="T_778ff_row24_col7" class="data row24 col7" >0.02</td>
      <td id="T_778ff_row24_col8" class="data row24 col8" >0.02</td>
      <td id="T_778ff_row24_col9" class="data row24 col9" >-0.12</td>
      <td id="T_778ff_row24_col10" class="data row24 col10" >0.62</td>
      <td id="T_778ff_row24_col11" class="data row24 col11" >0.62</td>
      <td id="T_778ff_row24_col12" class="data row24 col12" >0.29</td>
      <td id="T_778ff_row24_col13" class="data row24 col13" >0.29</td>
      <td id="T_778ff_row24_col14" class="data row24 col14" >0.52</td>
      <td id="T_778ff_row24_col15" class="data row24 col15" >0.56</td>
      <td id="T_778ff_row24_col16" class="data row24 col16" >0.03</td>
      <td id="T_778ff_row24_col17" class="data row24 col17" >0.65</td>
      <td id="T_778ff_row24_col18" class="data row24 col18" >0.67</td>
      <td id="T_778ff_row24_col19" class="data row24 col19" >0.69</td>
      <td id="T_778ff_row24_col20" class="data row24 col20" >0.28</td>
      <td id="T_778ff_row24_col21" class="data row24 col21" >0.47</td>
      <td id="T_778ff_row24_col22" class="data row24 col22" >0.52</td>
      <td id="T_778ff_row24_col23" class="data row24 col23" >0.60</td>
      <td id="T_778ff_row24_col24" class="data row24 col24" >1.00</td>
      <td id="T_778ff_row24_col25" class="data row24 col25" >0.57</td>
    </tr>
    <tr>
      <th id="T_778ff_level0_row25" class="row_heading level0 row25" >PTS</th>
      <td id="T_778ff_row25_col0" class="data row25 col0" >-0.06</td>
      <td id="T_778ff_row25_col1" class="data row25 col1" >0.48</td>
      <td id="T_778ff_row25_col2" class="data row25 col2" >0.74</td>
      <td id="T_778ff_row25_col3" class="data row25 col3" >0.89</td>
      <td id="T_778ff_row25_col4" class="data row25 col4" >0.99</td>
      <td id="T_778ff_row25_col5" class="data row25 col5" >0.98</td>
      <td id="T_778ff_row25_col6" class="data row25 col6" >0.37</td>
      <td id="T_778ff_row25_col7" class="data row25 col7" >0.42</td>
      <td id="T_778ff_row25_col8" class="data row25 col8" >0.42</td>
      <td id="T_778ff_row25_col9" class="data row25 col9" >0.15</td>
      <td id="T_778ff_row25_col10" class="data row25 col10" >0.92</td>
      <td id="T_778ff_row25_col11" class="data row25 col11" >0.92</td>
      <td id="T_778ff_row25_col12" class="data row25 col12" >0.29</td>
      <td id="T_778ff_row25_col13" class="data row25 col13" >0.36</td>
      <td id="T_778ff_row25_col14" class="data row25 col14" >0.90</td>
      <td id="T_778ff_row25_col15" class="data row25 col15" >0.89</td>
      <td id="T_778ff_row25_col16" class="data row25 col16" >0.29</td>
      <td id="T_778ff_row25_col17" class="data row25 col17" >0.44</td>
      <td id="T_778ff_row25_col18" class="data row25 col18" >0.65</td>
      <td id="T_778ff_row25_col19" class="data row25 col19" >0.61</td>
      <td id="T_778ff_row25_col20" class="data row25 col20" >0.58</td>
      <td id="T_778ff_row25_col21" class="data row25 col21" >0.64</td>
      <td id="T_778ff_row25_col22" class="data row25 col22" >0.35</td>
      <td id="T_778ff_row25_col23" class="data row25 col23" >0.83</td>
      <td id="T_778ff_row25_col24" class="data row25 col24" >0.57</td>
      <td id="T_778ff_row25_col25" class="data row25 col25" >1.00</td>
    </tr>
  </tbody>
</table>





```python
# Select columns which have high correlation
df_numerical = df_numerical[['GS', 'Age', 'G', 'GS','3PA', '3P%','2P%', 'eFG%','TOV', 'PTS']]
df_numerical.head()
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
      <th>GS</th>
      <th>Age</th>
      <th>G</th>
      <th>GS</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P%</th>
      <th>eFG%</th>
      <th>TOV</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>80.0</td>
      <td>31</td>
      <td>80.0</td>
      <td>80.0</td>
      <td>0.2</td>
      <td>0.222</td>
      <td>0.488</td>
      <td>0.485</td>
      <td>3.0</td>
      <td>14.1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8.0</td>
      <td>30</td>
      <td>82.0</td>
      <td>8.0</td>
      <td>0.6</td>
      <td>0.212</td>
      <td>0.418</td>
      <td>0.410</td>
      <td>1.8</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>82.0</td>
      <td>23</td>
      <td>82.0</td>
      <td>82.0</td>
      <td>1.7</td>
      <td>0.406</td>
      <td>0.481</td>
      <td>0.494</td>
      <td>3.2</td>
      <td>21.3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7.0</td>
      <td>29</td>
      <td>82.0</td>
      <td>7.0</td>
      <td>0.5</td>
      <td>0.293</td>
      <td>0.485</td>
      <td>0.482</td>
      <td>1.7</td>
      <td>11.1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.0</td>
      <td>33</td>
      <td>60.0</td>
      <td>0.0</td>
      <td>0.1</td>
      <td>0.167</td>
      <td>0.361</td>
      <td>0.357</td>
      <td>0.6</td>
      <td>2.8</td>
    </tr>
  </tbody>
</table>
</div>




```python
from sklearn.ensemble import GradientBoostingRegressor
from sklearn.ensemble import RandomForestRegressor
from sklearn.linear_model import LinearRegression

X = df_numerical[['GS', 'Age', 'G', 'GS','3PA', '3P%','2P%', 'eFG%','TOV']]
y = df_numerical['PTS']

# Train Regression models
GB = GradientBoostingRegressor(random_state=1)
RF = RandomForestRegressor(random_state=1)
LR = LinearRegression()

GB.fit(X, y)
RF.fit(X, y)
LR.fit(X, y)
```




    LinearRegression()




```python
# Predict gross using trained regression models
GB_pred = GB.predict(X)
RF_pred = RF.predict(X)
LR_pred = LR.predict(X)
```


```python
fig, axs = plt.subplots(1,3,figsize=(16,9))

axs[0].plot(GB_pred, "gd", label="GradientBoostingRegressor")
axs[0].tick_params(axis="x", which="both", bottom=False, top=False, labelbottom=False)
axs[0].set_ylabel("predicted")
axs[0].set_xlabel("training samples")
axs[0].legend(loc="best")
axs[0].set_title("Regressor predictions and their average")

axs[1].plot(RF_pred, "b^", label="RandomForestRegressor")
axs[1].tick_params(axis="x", which="both", bottom=False, top=False, labelbottom=False)
axs[1].set_ylabel("predicted")
axs[1].set_xlabel("training samples")
axs[1].legend(loc="best")
axs[1].set_title("Regressor predictions and their average")

axs[2].plot(LR_pred, "ys", label="LinearRegression")
axs[2].tick_params(axis="x", which="both", bottom=False, top=False, labelbottom=False)
axs[2].set_ylabel("predicted")
axs[2].set_xlabel("training samples")
axs[2].legend(loc="best")
axs[2].set_title("Regressor predictions and their average")

plt.show()
```


    
![png](output_33_0.png)
    


## Calculate RMSE values for 3-regressors


```python
from sklearn.metrics import mean_squared_error
import numpy as np
rmse = []
pred_PTS = pd.DataFrame({'PTS': y,'GB_PTS': GB_pred, 'RF_PTS':RF_pred, 'LR_PTS': LR_pred})
rmse.append(mean_squared_error(pred_PTS['PTS'], pred_PTS['GB_PTS'], squared=False))
rmse.append(mean_squared_error(pred_PTS['PTS'], pred_PTS['RF_PTS'], squared=False))
rmse.append(mean_squared_error(pred_PTS['PTS'], pred_PTS['LR_PTS'], squared=False))
methods = ['GradientBoostingRegressor', 'RandomForestRegressor', 'LinearRegression']
RMSE = pd.DataFrame({'Method': methods,'RMSE': rmse})
RMSE
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
      <th>Method</th>
      <th>RMSE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>GradientBoostingRegressor</td>
      <td>2.430155</td>
    </tr>
    <tr>
      <th>1</th>
      <td>RandomForestRegressor</td>
      <td>0.938909</td>
    </tr>
    <tr>
      <th>2</th>
      <td>LinearRegression</td>
      <td>2.708650</td>
    </tr>
  </tbody>
</table>
</div>




```python
RMSE.plot.bar(x = 'Method', y = 'RMSE', figsize=(10, 5))
```




    <AxesSubplot:xlabel='Method'>




    
![png](output_36_1.png)
    


*From above table and graph, we can see that Random Forest Regressor is the best. The RMSE of Random Forest Regressor is less than 1.
Now lets predict PTS for all values.*

# Predict the PTS using other features


```python
RF_pred = RF.predict(X)
df_numerical['pred_PTS'] = RF_pred
df_numerical.head(10)
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
      <th>GS</th>
      <th>Age</th>
      <th>G</th>
      <th>GS</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P%</th>
      <th>eFG%</th>
      <th>TOV</th>
      <th>PTS</th>
      <th>pred_PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>80.0</td>
      <td>31</td>
      <td>80.0</td>
      <td>80.0</td>
      <td>0.2</td>
      <td>0.222</td>
      <td>0.488</td>
      <td>0.485</td>
      <td>3.0</td>
      <td>14.1</td>
      <td>16.328</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8.0</td>
      <td>30</td>
      <td>82.0</td>
      <td>8.0</td>
      <td>0.6</td>
      <td>0.212</td>
      <td>0.418</td>
      <td>0.410</td>
      <td>1.8</td>
      <td>9.0</td>
      <td>8.520</td>
    </tr>
    <tr>
      <th>2</th>
      <td>82.0</td>
      <td>23</td>
      <td>82.0</td>
      <td>82.0</td>
      <td>1.7</td>
      <td>0.406</td>
      <td>0.481</td>
      <td>0.494</td>
      <td>3.2</td>
      <td>21.3</td>
      <td>21.144</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7.0</td>
      <td>29</td>
      <td>82.0</td>
      <td>7.0</td>
      <td>0.5</td>
      <td>0.293</td>
      <td>0.485</td>
      <td>0.482</td>
      <td>1.7</td>
      <td>11.1</td>
      <td>11.069</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.0</td>
      <td>33</td>
      <td>60.0</td>
      <td>0.0</td>
      <td>0.1</td>
      <td>0.167</td>
      <td>0.361</td>
      <td>0.357</td>
      <td>0.6</td>
      <td>2.8</td>
      <td>2.650</td>
    </tr>
    <tr>
      <th>5</th>
      <td>79.0</td>
      <td>23</td>
      <td>79.0</td>
      <td>79.0</td>
      <td>0.1</td>
      <td>0.444</td>
      <td>0.541</td>
      <td>0.543</td>
      <td>2.7</td>
      <td>11.4</td>
      <td>13.969</td>
    </tr>
    <tr>
      <th>6</th>
      <td>17.0</td>
      <td>28</td>
      <td>36.0</td>
      <td>17.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>0.467</td>
      <td>0.466</td>
      <td>2.3</td>
      <td>13.8</td>
      <td>13.604</td>
    </tr>
    <tr>
      <th>7</th>
      <td>55.0</td>
      <td>31</td>
      <td>66.0</td>
      <td>55.0</td>
      <td>0.2</td>
      <td>0.083</td>
      <td>0.458</td>
      <td>0.453</td>
      <td>1.6</td>
      <td>14.2</td>
      <td>12.425</td>
    </tr>
    <tr>
      <th>8</th>
      <td>80.0</td>
      <td>23</td>
      <td>80.0</td>
      <td>80.0</td>
      <td>0.1</td>
      <td>0.000</td>
      <td>0.526</td>
      <td>0.522</td>
      <td>2.9</td>
      <td>14.7</td>
      <td>17.268</td>
    </tr>
    <tr>
      <th>9</th>
      <td>78.0</td>
      <td>29</td>
      <td>78.0</td>
      <td>78.0</td>
      <td>0.3</td>
      <td>0.200</td>
      <td>0.523</td>
      <td>0.520</td>
      <td>3.6</td>
      <td>26.9</td>
      <td>26.310</td>
    </tr>
  </tbody>
</table>
</div>




```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```
