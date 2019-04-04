Title: NBA Standings to PostgreSQL
Date: 2018-12-29
Tags: pelican, NBA, Selenium
Slug: NBA
Authors: Tony Ryan
Summary: Net Points vs Total Wins


# Get All Games


```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC 
import time
import os
import urllib
import requests
from requests import get
from bs4 import BeautifulSoup
import pandas as pd
from docx import Document
from docx.shared import Inches

from datetime import datetime
NBA_Season_months = {'October':[1 , 2018] , 'November' : [2,2018] , 'December' : [3,2018], 'January' : [4,2019]
                     ,'February' : [5,2019], 'March' : [6,2019], 'April' : [7,2019], 'May': [8,2019]}



path = 'C:/users/Tonyryanworldwide/untitled folder/chromedriver_win32'
os.chdir(path)
cwd = os.getcwd()

# options = webdriver.ChromeOptions()
# options.add_argument('--ignore-certificate-errors')
# options.add_argument("--test-type")
# driver = webdriver.Chrome(options=options)


home = []
away = []
home_score = []
away_score = []
games = []
date = []
#urls = []

# for month in range(1,8):
#     urls.append('https://stats.nba.com/schedule/#!?Month=' + str(month) + '&PD=N')

for key in NBA_Season_months:
    month = NBA_Season_months[key][0]
    year = NBA_Season_months[key][1]

    url = 'https://stats.nba.com/schedule/#!?Month=' + str(month) + '&PD=N'
    if year == 2018:
        
        
    #for i in range(0,7):
        options = webdriver.ChromeOptions()
        options.add_argument('--ignore-certificate-errors')
        options.add_argument("--test-type")
        driver = webdriver.Chrome(options=options)

        driver.get(url)
        scores_start  = driver.find_elements_by_xpath('/html/body/main/div[2]/div/div[2]/div/div')

        s2 = scores_start[0].get_attribute('outerHTML')
        scores = BeautifulSoup(s2, 'html.parser')
        dates = scores.div.find_all(class_ = "row collapse schedule-content")

        for x in range(0, len(dates)):
            #print(dates[x].a.text)
            tables = dates[x].find_all('table')


            for i in range(0, len(tables)):
                try:
                    away.append(tables[i].find_all('a')[0].text)
                    away_score.append(tables[i].find_all('td')[0].text)
                    home.append(tables[i].find_all('a')[1].text)
                    home_score.append(tables[i].find_all('td')[1].text)

                    date.append(dates[x].a.text)

                    ab = dates[x].a.text
                    date_dt1 = datetime.strptime(ab, '%A, %B %d')
                    date.append(date_dt1.replace(year = year).strftime("%m/%d/%Y"))
                except:        
                    home = []
                    away = []
                    home_score = []
                    aways_score = []
                    date = []
                games.append(home + home_score + away + away_score + date)
                if len(games[-1]) < 5:
                    games.pop()
                home = []
                away = []
                home_score = []
                away_score = []
                date = []
        driver.close()


```

# Get Current Month Games


```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC 
import time
import os
import urllib
import requests
from requests import get
from bs4 import BeautifulSoup
import pandas as pd
from docx import Document
from docx.shared import Inches

from datetime import datetime
NBA_Season_months = {'October':[1 , 2018] , 'November' : [2,2018] , 'December' : [3,2018], 'January' : [4,2019]
                     ,'February' : [5,2019], 'March' : [6,2019], 'April' : [7,2019], 'May': [8,2019]}
start = datetime.today()
a = start.strftime('%B')
month = NBA_Season_months[a][0]
year = NBA_Season_months[a][1]

path = 'C:/users/Tonyryanworldwide/untitled folder/chromedriver_win32'
os.chdir(path)
cwd = os.getcwd()

options = webdriver.ChromeOptions()
options.add_argument('--ignore-certificate-errors')
options.add_argument("--test-type")
driver = webdriver.Chrome(options=options)


home = []
away = []
home_score = []
away_score = []
games = []
date = []
urls = []


url = 'https://stats.nba.com/schedule/#!?Month=' + str(month) + '&PD=N'


options = webdriver.ChromeOptions()
options.add_argument('--ignore-certificate-errors')
options.add_argument("--test-type")
driver = webdriver.Chrome(options=options)

driver.get(url)
scores_start  = driver.find_elements_by_xpath('/html/body/main/div[2]/div/div[2]/div/div')

s2 = scores_start[0].get_attribute('outerHTML')
scores = BeautifulSoup(s2, 'html.parser')
dates = scores.div.find_all(class_ = "row collapse schedule-content")

for x in range(0, len(dates)):
    #print(dates[x].a.text)
    tables = dates[x].find_all('table')


    for i in range(0, len(tables)):
        try:
            away.append(tables[i].find_all('a')[0].text)
            away_score.append(tables[i].find_all('td')[0].text)
            home.append(tables[i].find_all('a')[1].text)
            home_score.append(tables[i].find_all('td')[1].text)
            
            date.append(dates[x].a.text)
            
            ab = dates[x].a.text
            date_dt1 = datetime.strptime(ab, '%A, %B %d')
            date.append(date_dt1.replace(year = year).strftime("%m/%d/%Y"))

        except:        
            home = []
            away = []
            home_score = []
            aways_score = []
            date = []
        games.append(home + home_score + away + away_score + date)
        if len(games[-1]) < 5:
            games.pop()
        home = []
        away = []
        home_score = []
        away_score = []
        date = []
driver.close()


```


```python
import pandas
from pandas import *
cols = ['home', 'hscore', 'away', 'ascore', 'dayOfWeekDate','date']
Games_DF = pandas.DataFrame(games,columns =cols)
Games_DF.hscore = pandas.to_numeric(Games_DF['hscore'])
Games_DF.ascore = pandas.to_numeric(Games_DF['ascore'])
Games_Played = Games_DF.loc[Games_DF.hscore.notnull()]
Games_Played = Games_Played[Games_Played.date >= '10/16/2018'] ## Regular Season Start

Games_Played['home_won'] = np.where(Games_Played['hscore'] > Games_Played['ascore'], 1 , np.where(Games_Played['hscore'] == Games_Played['ascore'], .5,0))
Games_Played['away_won'] = np.where(Games_Played['ascore'] > Games_Played['hscore'], 1 , np.where(Games_Played['hscore'] == Games_Played['ascore'], .5,0))
Games_Played['home_differential'] = Games_Played.hscore - Games_Played.ascore
Games_Played['away_differential'] = Games_Played.ascore - Games_Played.hscore
HDif = Games_Played.groupby('home').sum()['home_differential']
ADif = Games_Played.groupby('away').sum()['away_differential']
HWon = Games_Played.groupby('home').sum()['home_won']
Awon = Games_Played.groupby('away').sum()['away_won']

Games_Played.reset_index(inplace = True)
Games_Played.drop(columns = 'index', inplace = True)
Games_Played.head(5)
```


```python
Differential = pandas.concat([ADif, HDif, HWon, Awon], axis = 1)
Differential['total_net_points'] = Differential.away_differential + Differential.home_differential
Differential['total_wins'] = Differential['home_won'] + Differential['away_won']
Differential.sort_values(by = 'total_net_points' ,ascending = False)


```


```python
import numpy as np
import matplotlib.pyplot as plt  
from sklearn.linear_model import LinearRegression
from ggplot import *

tnp = Differential['total_net_points'].values.reshape(-1,1)
tw = Differential['total_wins'].values.reshape(-1,1)
linear_regressor = LinearRegression() 
linear_regressor.fit(tnp, tw) 
Y_pred = linear_regressor.predict(tnp) 

p = DataFrame(np.hstack((tnp,tw,Y_pred)))
p.columns = ['Net Points', 'Total Wins' , 'Predicted Wins']
ggplot(p, aes ( 'Net Points', 'Predicted Wins')) + geom_line(color = 'red') + geom_point(aes('Net Points' , 'Total Wins'))
```

# Create Table Syntax


```python
import psycopg2
from sqlalchemy import create_engine
#from config import config


command = (

"""
Create Table games_played_reg_season
(
away_differential float, home_differential float, home_won float, aways_won float,
   total_net_points float, total_wins float

)
"""
)

engine = create_engine('postgresql+psycopg2://postgres:COLLECTORGOD@11@localhost:5432/tonyfirst')
conn = engine.raw_connection()
cur = conn.cursor()

# commit the changes
# cur.execute(command)
# cur.close()
# conn.commit()
```

# DataFrame to New Table syntax


```python
# from sqlalchemy import create_engine
# engine = create_engine('postgresql+psycopg2://postgres:<PASSWORD>@localhost:5432/tonyfirst')

# Games_Played.to_sql('games_played_reg_season' , con = engine)

```

# DataFrame to Existing Table syntax


```python
from sqlalchemy import create_engine
engine = create_engine('postgresql+psycopg2://postgres:COLLECTORGOD@11@localhost:5432/tonyfirst')

Games_Played.to_sql('games_played_reg_season' , con = engine, if_exists = 'append')

```

# Select  from POSTGRESQL Table


```python
from sqlalchemy import create_engine
engine = create_engine('postgresql+psycopg2://postgres:COLLECTORGOD@11@localhost:5432/tonyfirst')
df = engine.execute("Select * from wins").fetchall()
pandas.DataFrame(df).sort_values(by = 5, ascending = False)

```


```python
import psycopg2

command = (

"""
DROP TABLE IF EXISTS temp1
;
CREATE TEMP TABLE temp1 AS
Select *, row_number() over(partition by date, home  order by hscore desc, ascore desc) theord from games_played_reg_season
;
Delete from temp1
where theord <> 1
;
Alter table temp1
drop column theord
;
Truncate games_played_reg_season
;
Insert into games_played_reg_season
Select * from temp1
;
"""
)

conn = create_engine('postgresql+psycopg2://postgres:COLLECTORGOD@11@localhost:5432/tonyfirst')
conn = engine.raw_connection()
cur = conn.cursor()
cur.execute(command)
cur.close()
conn.commit()
```

## A Batch file runs the above cells except the Get All Games cell which only  needed to be run once


```python
from sqlalchemy import create_engine
import pandas
from pandas import *
import matplotlib
engine = create_engine('postgresql+psycopg2://postgres:COLLECTORGOD@11@localhost:5432/tonyfirst')
df = engine.execute("Select * from winning").fetchall()
DF = pandas.DataFrame(df)
DF.sort_values(by = 6, ascending = False)

cols = ['team' , 'away_wins' , 'home_wins' , 'away_dif', 'home_dif' , 'total_wins' , 'total_dif']
DF.columns = cols


```


```python
import os
path = 'C:/Users/Tonyryanworldwide/Blog/content/pages/'
os.chdir(path)
cwd = os.getcwd()

import numpy as np
import matplotlib.pyplot as plt  
from sklearn.linear_model import LinearRegression
from ggplot import *

tnp = DF['total_dif'].values.reshape(-1,1)
tw = DF['total_wins'].values.reshape(-1,1)
linear_regressor = LinearRegression() 
linear_regressor.fit(tnp, tw) 
Y_pred = linear_regressor.predict(tnp) 

p = DataFrame(np.hstack((tnp,tw,Y_pred)))
p.columns = ['Net Points', 'Total Wins' , 'Predicted Wins']
minwins = int(min(p['Total Wins']))
maxwins = int(max(p['Total Wins']))
x = ggplot(p, aes ( 'Net Points', 'Predicted Wins')) + geom_line(color = 'red') + geom_point(
    aes('Net Points' , 'Total Wins')) + scale_y_continuous(breaks = range(minwins,maxwins+2,2))
x.save(filename = 'wins_vs_points.png')

```


![png]({attach}output_18_0.png)



