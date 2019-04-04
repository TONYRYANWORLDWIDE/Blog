Title: YouTube Stats
Date: 2019-01-01
Tags: Selenium, Python, Youtube
Slug: YouTube
Authors: Tony Ryan
Summary: YouTube stats of related videos


## Start YouTube stats dictionary from seed page


```python
import urllib
import requests
from requests import get
from bs4 import BeautifulSoup
import pandas as pd

urlstart = 'https://www.youtube.com'
watch = '/watch?v=wYREQkVtvEc' ## seed page

url = urlstart + watch

req = get(url)
a = req.text
dic = dict.fromkeys(['views', 'title', 'likes', 'dislikes' , 'channelname'])
dic['views'] = []
dic['title'] = []
dic['likes'] = []
dic['dislikes'] = []
dic['channelname'] = []
views = []
title = []
likes = []
dislikes = []
channelname = []
a = req.text

Tube = BeautifulSoup(a, 'html.parser')

x = Tube.body.find(class_ = 'watch-view-count')
dic['views'].append(x.text)
dic['title'].append(Tube.body.div.next_sibling.find_all('h1')[1].span['title'])

ytclick = Tube.body.find_all('span', class_ = 'yt-uix-clickcard')
dic['likes'].append(ytclick[2].button.text)

ytclick = Tube.body.find_all('span', class_ = 'yt-uix-clickcard')
dic['dislikes'].append(ytclick[4].button.text)

ytclick = Tube.body.find_all('a', attrs = {'class' : 'yt-uix-sessionlink spf-link '})
dic['channelname'].append(ytclick[0].text)

dic
```




    {'views': ['1,103,557 views'],
     'title': ['How To Deadlift: Starting Strength 5 Step Deadlift'],
     'likes': ['32,002'],
     'dislikes': ['384'],
     'channelname': ['Alan Thrall']}



## Pull in stats of all videos recommended based on seed video


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

watchlist = []
watchstage = []
watchlist2 = []
Used = []
links = Tube.body.find_all('a', attrs = {'class' : ' content-link spf-link yt-uix-sessionlink spf-link '})
for i in range (0, len(links)):
    watchlist.append(links[i]['href'])
    

loops = 1
while loops <= 1:
    for i in watchstage:
        if i not in watchlist:
            watchlist.append(i)
    watchstage = []
    
    for w in watchlist:
        #print(loops)
        #print(len(watchlist), len(watchlist2))
        if w not in watchlist2:

            url = urlstart + w
            req = get(url)
            b = req.text
            Tube = BeautifulSoup(b, 'html.parser')
            x = Tube.body.find(class_ = 'watch-view-count')
            dic['views'].append(x.text)
            dic['title'].append(Tube.body.div.next_sibling.find_all('h1')[1].span['title'])

            ytclick = Tube.body.find_all('span', class_ = 'yt-uix-clickcard')
            dic['likes'].append(ytclick[2].button.text)

            ytclick = Tube.body.find_all('span', class_ = 'yt-uix-clickcard')
            dic['dislikes'].append(ytclick[4].button.text)

            ytclick = Tube.body.find_all('a', attrs = {'class' : 'yt-uix-sessionlink spf-link '})
            dic['channelname'].append(ytclick[0].text)
            watchlist2.append(w)



            links = Tube.body.find_all('a', attrs = {'class' : ' content-link spf-link yt-uix-sessionlink spf-link '})
            
            for i in range (0, len(links)):
                if links[i]['href'] not in watchlist:
                    watchstage.append(links[i]['href'])
                    
            
    loops += 1           
```

## Convert Dictionary to Dataframe and clean data


```python
import pandas
from pandas import DataFrame

YT_Stats = pandas.DataFrame.from_dict(dic) 

trunc_and_replace = lambda x: x[0:-6].replace(',', '')
replace = lambda x: x.replace(',' , '')

YT_Stats['views'] = YT_Stats['views'].apply(trunc_and_replace)
YT_Stats.likes = YT_Stats.likes.apply(replace)
YT_Stats.dislikes = YT_Stats.dislikes.apply(replace)
convert = ['views', 'likes', 'dislikes']

for x in range(0, len(convert)):
    YT_Stats[convert[x]] = pandas.to_numeric(YT_Stats[convert[x]])
    
YT_Stats['Views To Get One Like'] = YT_Stats['views'] / YT_Stats['likes']
YT_Stats.sort_values(by = 'Views To Get One Like')
YT_Stats['LikesRatio'] = YT_Stats.likes / YT_Stats.dislikes
YT_Stats.head()

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
      <th>views</th>
      <th>title</th>
      <th>likes</th>
      <th>dislikes</th>
      <th>channelname</th>
      <th>Views To Get One Like</th>
      <th>LikesRatio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1103557</td>
      <td>How To Deadlift: Starting Strength 5 Step Dead...</td>
      <td>32002</td>
      <td>384</td>
      <td>Alan Thrall</td>
      <td>34.484001</td>
      <td>83.338542</td>
    </tr>
    <tr>
      <th>1</th>
      <td>661433</td>
      <td>Common Deadlift Errors ft. Austin Baraki - Sta...</td>
      <td>18303</td>
      <td>176</td>
      <td>Alan Thrall</td>
      <td>36.137956</td>
      <td>103.994318</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5109701</td>
      <td>10 Exercises All Men Should AVOID!</td>
      <td>83716</td>
      <td>10173</td>
      <td>Gravity Transformation - Fat Loss Experts</td>
      <td>61.036134</td>
      <td>8.229234</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4664977</td>
      <td>Why It's Almost Impossible to Jump Higher Than...</td>
      <td>54374</td>
      <td>3423</td>
      <td>WIRED</td>
      <td>85.794258</td>
      <td>15.884896</td>
    </tr>
    <tr>
      <th>4</th>
      <td>392561</td>
      <td>Alan Thrall Calls Out JUJIMUFU / 1,000 lbs Tir...</td>
      <td>9118</td>
      <td>389</td>
      <td>Alan Thrall</td>
      <td>43.053411</td>
      <td>23.439589</td>
    </tr>
  </tbody>
</table>
</div>



## Pull transcript of Video


```python
urlstart = 'https://www.youtube.com'
watch = watchlist[0]

document = Document()
            
document.add_heading('Transcript ' + watch , 0)

path = 'C:/users/Tonyryanworldwide/untitled folder/chromedriver_win32'
os.chdir(path)
cwd = os.getcwd()

transcript = []
f = open(watch[9:] + '.txt','w')

options = webdriver.ChromeOptions()
options.add_argument('--ignore-certificate-errors')
options.add_argument("--test-type")
driver = webdriver.Chrome(options=options)
driver.get(urlstart + watch)
#driver.get(url)
A = Tube.body.find('button', attrs = {'title' : 'More actions'})
more_action_btn = WebDriverWait(driver, 30).until(EC.element_to_be_clickable((By.XPATH, "//button[@aria-label = 'More actions']")))
more_action_btn.click()
open_trancript_btn = WebDriverWait(driver, 15).until(EC.element_to_be_clickable((By.XPATH, "//paper-listbox[@id = 'items']/ytd-menu-service-item-renderer")))
open_trancript_btn.click() 
wait = WebDriverWait(driver, 20)
body_html = driver.find_element_by_xpath("/html/body")


try:
    myElem = WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.XPATH, '//*[@id="body"]/ytd-transcript-body-renderer/div')))
    print ("Start")
    a = driver.find_elements_by_xpath('//*[@id="body"]/ytd-transcript-body-renderer/div')
except TimeoutException:
    print ('TimedOut')


for i in range(0,len(a), 1):
    trans = a[i].get_attribute('outerHTML')
    scrip = BeautifulSoup(trans, 'html.parser')
    #print(scrip.div.contents[3].text.strip())
    transcript.append(scrip.div.contents[3].text.strip())
    f.write('\n' + scrip.div.contents[3].text.strip())
    document.add_paragraph(scrip.div.contents[3].text.strip())

document.save( watch[9:] + '.docx')  
```  


```python
import ggplot
from ggplot import *
# a = YT_Stats['Views To Get One Like'] 
# b = YT_Stats['LikesRatio']

ggplot(YT_Stats, aes('Views To Get One Like','LikesRatio')
      ) + geom_point() + xlim(0,400) + scale_x_continuous(name = 'Views To Get One Like') 
#+ facet_wrap('channelname')


```


![png]({attach}output_8_0.png)





