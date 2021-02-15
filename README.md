```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
```


```python
URL = 'https://basketball.realgm.com/nba/awards/by-type/Player-Of-The-Week/30'
```


```python
response = requests.get(URL)
```


```python
soup = BeautifulSoup(response.content,'html.parser')
```


```python
table = soup.find('table', attrs={'class': 'tablesaw','data-tablesaw-mode':'swipe'}).tbody
#tablesaw tablesaw-swipe tablesaw-sortable
#data-tablesaw-mode ='swipe'
```


```python
columns = ['Season', 'Player Name', 'Conference', 'Date', 'Team','Pos','Hight','Weight','Age','PreDraftTeam','DraftYear','YOS']
df = pd.DataFrame(columns=columns)

```


```python
table = soup.find('table', attrs={'class': 'tablesaw','data-tablesaw-mode':'swipe'}).tbody
trs = table.find_all('tr')
for tr in trs:
  tds = tr.find_all('td')
  row = [td.text.replace('/n', '') for td in tds]
  df = df.append(pd.Series(row, index=columns), ignore_index=True)
```


```python
df.to_csv('nba player of the wee.csv',index=False)
```
