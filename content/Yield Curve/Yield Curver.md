

Title: My First Post
Author: Tony Ryan
Date: 2018-12-10

python
import pandas as pd
from pandas import DataFrame
import fredapi
from fredapi import Fred

fred = Fred(api_key='632e5a734e44aaaea641aa6f2f3f0898')
data = pd.DataFrame(fred.get_series('DGS30'))
a = fred.get_series('DGS30')
data2 = pd.DataFrame(fred.get_series('DGS10'))
b = fred.get_series('DGS10')
data3 = pd.DataFrame(fred.get_series('DGS2'))
c = fred.get_series('DGS2')
data4 = pd.DataFrame(fred.get_series('DGS5'))
d = fred.get_series('DGS5')
data5 = pd.DataFrame(fred.get_series('DGS1'))
e = fred.get_series('DGS1')
data6 = pd.DataFrame(fred.get_series('DGS7'))
e = fred.get_series('DGS7')
data7 = pd.DataFrame(fred.get_series('DGS3'))
e = fred.get_series('DGS3')
Median_income16plus = pd.DataFrame(fred.get_series('LES1252881600Q'))
```


```python
data.columns=['DGS30']
data2.columns = ['DGS10']
data3.columns = ['DGS2']
data4.columns = ['DGS5']
data5.columns = ['DGS1']
data6.columns = ['DGS7']
data7.columns = ['DGS3']
Median_income16plus.columns = ['Median Income']



data.index.name = 'Date'
data2.index.name = 'Date'
data3.index.name = 'Date'
data4.index.name = 'Date'
data5.index.name = 'Date'
data6.index.name = 'Date'
data7.index.name = 'Date'
Median_income16plus.index.name = 'Date'
Yields = data5.join(data3).join(data7).join(data4).join(data6).join(data2).join(data)
Yields = Yields.dropna()
Yields.index = Yields.index.date

```


```python


Yields_Employment = data5.join(data3).join(data7).join(data4).join(data6).join(data2).join(data).join(Median_income16plus)
Yields_Employment.index = Yields_Employment.index.date
#Yields_Employment = Yields_Employment.dropna()

```


```python
from datetime import datetime
from dateutil.relativedelta import relativedelta
last = Yields.index.values[-1]
lastmonth1 = last + relativedelta(months=-1)
lastmonth3 = last + relativedelta(months=-3)
lastmonth6 = last + relativedelta(months=-6)
lastyear1 = last + relativedelta(years=-1)
lastyear2 = last + relativedelta(years=-2)
#lastyear3 = last + relativedelta(years=-3)
lastmonth39 = last + relativedelta(months=-39)
#lastyear5 = last + relativedelta(years=-5)

```


```python
Yields.loc[lastmonth6]
Yields.iloc[-1]
```




    DGS1     2.70
    DGS2     2.75
    DGS3     2.76
    DGS5     2.75
    DGS7     2.80
    DGS10    2.87
    DGS30    3.14
    Name: 2018-12-06, dtype: float64




```python
#Yields.plot()
import matplotlib.pyplot as plt


plt.plot(Yields.iloc[-1].index, Yields.iloc[-1])
plt.plot(Yields.iloc[-1].index, Yields.loc[lastmonth1])
#plt.plot(Yields.iloc[-1].index, Yields.loc[lastmonth3])
plt.plot(Yields.iloc[-1].index, Yields.loc[lastmonth6])
plt.plot(Yields.iloc[-1].index, Yields.loc[lastyear1])
plt.plot(Yields.iloc[-1].index, Yields.loc[lastyear2])
plt.plot(Yields.iloc[-1].index, Yields.loc[lastyear5])
#plt.plot(Yields.iloc[-1].index, Yields.loc[lastmonth39])
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)



plt.show()


```


![png](output_5_0.png)



```python
#Yields.plot()
import matplotlib.pyplot as plt



fig, ax1 = plt.subplots()
ax2 = ax1.twinx() 


ax1.plot(Yields_Employment.iloc[4500:30000].index, Yields_Employment['DGS10'].iloc[4500:30000],  color = 'tab:blue')
#plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
ax2.plot(Median_income16plus.iloc[1:3000].index, Median_income16plus['Median Income'].iloc[1:3000], color = 'tab:red')
#plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
plt.show()


```


![png](output_6_0.png)



```python
plt.plot(Median_income16plus.iloc[100:300].index, Median_income16plus['Median Income'].iloc[100:300])
plt.show()
```


![png](output_7_0.png)



```python
plt.plot(Yields.iloc[-1].index, Yields.iloc[-1])
plt.plot(Yields.iloc[-1].index, Yields.iloc[-30])
plt.plot(Yields.iloc[-1].index, Yields.iloc[-90])
plt.plot(Yields.iloc[-1].index, Yields.iloc[-180])
plt.plot(Yields.iloc[-1].index, Yields.iloc[-365])
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
```




    <matplotlib.legend.Legend at 0xfefeb57d30>




![png](output_8_1.png)

