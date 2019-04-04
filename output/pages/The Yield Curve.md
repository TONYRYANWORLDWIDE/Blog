Title: Yield Curve2
Date: 2018-12-19
Tags: pelican, Yield, FRED
Slug: my-post
Authors: Tony Ryan
Summary: Yield Curve Data from FRED API

## Bring in Government Bond Yields and Median Income Data from FRED using their API


```python
import pandas as pd
from pandas import DataFrame
import fredapi
from fredapi import Fred
import os
path = 'C:/Users/Tonyryanworldwide/Blog/content/pages/graphs/'
os.chdir(path)
cwd = os.getcwd()

fred = Fred(api_key='632e5a734e44aaaea641aa6f2f3f0898')
data = pd.DataFrame(fred.get_series('DGS30'), columns = ['30YR'])
data2 = pd.DataFrame(fred.get_series('DGS10'), columns = ['10YR'])
data3 = pd.DataFrame(fred.get_series('DGS2'), columns = ['2YR'])
data4 = pd.DataFrame(fred.get_series('DGS5'), columns = ['5YR'])
data5 = pd.DataFrame(fred.get_series('DGS1'), columns = ['1YR'])
data6 = pd.DataFrame(fred.get_series('DGS7'), columns = ['7YR'])
data7 = pd.DataFrame(fred.get_series('DGS3'), columns = ['3YR'])
data8 = pd.DataFrame(fred.get_series('DGS1mo'), columns = ['1MO'])
data9 = pd.DataFrame(fred.get_series('DGS3mo'), columns = ['3MO'])
Median_income16plus = pd.DataFrame(fred.get_series('LES1252881600Q'), columns = ['Median Income'])


frames = [data,data2,data3,data4,data5,data6,data7,data8,data9,Median_income16plus]
for x in frames:    
    x.index.name = 'Date'
Median_income16plus.reset_index(inplace = True)
Median_income16plus.columns = ['Date' , 'Median Income']
Median_income16plus['Date'] = Median_income16plus['Date'].dt.date
```


```python
Yields = data8.join(data9, how = 'outer').join(data5, how = 'outer').join(data3).join(data7).join(data4).join(data6).join(data2).join(data)
Yields.index = Yields.index.date
Yields.dropna(how = 'all', inplace = True)

```


```python
import os
path = 'C:/Users/Tonyryanworldwide/Blog/content/pages/graphs/'
os.chdir(path)
cwd = os.getcwd()

import datetime
from pandas import Series
from datetime import date
DateAfter = datetime.date(1970, 1, 1)
Median_income_Recent = Median_income16plus[Median_income16plus['Date'] >= DateAfter]
Median_income_Recent.reset_index(inplace = True)
Median_income_Recent.drop(columns = ['index'], inplace = True)
Median_income_Recent

Yield_Dates = Yields.index
TheDateList = Median_income_Recent['Date']
DateNear = []
def CloseDate(x):   
    return min(item for item in Yield_Dates if item >= x)    
Median_income_Recent['Near'] = Median_income_Recent['Date'].apply(CloseDate)


Graph = Yields.merge(Median_income_Recent, left_index = True, right_on = 'Near' ,how = 'inner')

```

# Most Recent Yields


```python
import matplotlib.pyplot as plt

fig, ax1 = plt.subplots()
ax2 = ax1.twinx() 

ax1.plot(Graph[1:3000].Near, Graph['10YR'].iloc[1:3000],  color = 'tab:blue')

ax2.plot(Graph.iloc[1:3000].Near, Graph['Median Income'].iloc[1:3000], color = 'tab:red')


fig.legend(bbox_to_anchor=(1.0, 1), loc=1, borderaxespad=0.)

fig1 = plt.gcf()
plt.figure(figsize =(10,10))
fig1.savefig('Yields_Employment.png', bbox_inches = 'tight')


```


![png]({attach}output_6_0.png)



    <Figure size 720x720 with 0 Axes>



```python
Yields.iloc[-1]
```




    1MO     2.36
    3MO     2.42
    1YR     2.68
    2YR     2.73
    3YR     2.72
    5YR     2.73
    7YR     2.81
    10YR    2.89
    30YR    3.14
    Name: 2018-12-14, dtype: float64



## New Graph Method


```python
import matplotlib.pyplot as plt
from matplotlib import figure

list_date = Yields.index
graph_points = []

def closest(x, dif):
    x = last + relativedelta(months = dif)    
    y =  min(item for item in list_date if item > x)
    return y
closest(lastmonth1,-1)

for each in [-24,-18,-12,-6,-1]:
    graph_points.append(closest(x, each))

for points in range(0, len(graph_points)): 
    
    plt.plot(Yields.iloc[-1].index, Yields.loc[graph_points[points]])
plt.plot(Yields.iloc[-1].index, Yields.iloc[-1])
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
fig1 = plt.gcf()
plt.figure(figsize =(10,5))
fig1.savefig('YieldCurve.png', bbox_inches = 'tight')


```


![png]({attach}output_9_0.png)



    <Figure size 720x360 with 0 Axes>


## Day over day changes are more clear looking at the 1 to 10YR yields.


```python
Yields1YR_PLUS = Yields[['1YR','2YR','3YR', '5YR', '7YR', '10YR']]


plt.plot(Yields1YR_PLUS.iloc[-1].index, Yields1YR_PLUS.iloc[-1])
plt.plot(Yields1YR_PLUS.iloc[-2].index, Yields1YR_PLUS.iloc[-2])
plt.plot(Yields1YR_PLUS.iloc[-1].index, Yields1YR_PLUS.iloc[-3])
#plt.plot(Yields.iloc[-1].index, Yields.iloc[-6])
#plt.plot(Yields.iloc[-1].index, Yields.iloc[-9])
#plt.plot(Yields.iloc[-1].index, Yields.iloc[-12])
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)


fig1 = plt.gcf()
plt.figure(figsize =(15,15))
fig1.savefig('Yields_Days_Ago.png', bbox_inches = 'tight')

```


![png]({attach}output_11_0.png)



    <Figure size 1080x1080 with 0 Axes>

