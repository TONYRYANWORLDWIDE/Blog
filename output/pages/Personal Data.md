Title: Personal Info
Date: 2018-12-09
Author: Tony Ryan



```python
import pandas
from pandas import *
import datetime
#from pandas import Series
from datetime import date

TimeDate = lambda x: datetime.datetime.strptime(x, '%A, %B %d').date()

cols = ['Project','start','end' , 'Time']
Time = pandas.read_csv('C:/Users/Tonyryanworldwide/Desktop/Hours/Daily_Time_Detail_All_Projects.csv', skiprows = 2, header = 0, names =  ['Project','start','end' , 'Time'], index_col = False)


Time = Time[4:]
Time = Time[Time['Project'] != 'Total']
Time = Time[Time['Project'] != 'GRAND TOTAL']
Time = Time[Time.Project.notnull()]
Time = Time.reset_index(drop = True)
Time
Date = []
TaskDate = []
Delete_rows = []
for x in range (0 , len(Time)):
    if Time['Project'][x].find(',') > 0:
        if datetime.datetime.strptime(Time['Project'][x], '%A, %B %d').date().strftime('%m') == '12':
            year = 2018
        else:
            year = 2019
        ITSaDate = datetime.datetime.strptime(Time['Project'][x], '%A, %B %d').date().strftime('%m/%d/' + str(year))
        Delete_rows.append(x)
    else: 
        Date.append(ITSaDate)
        
Time = Time.drop(Delete_rows)
Time = Time.reset_index(drop = True)
Time['Date'] = Series(Date)
Time = Time[['Date','start', 'end','Time','Project']]
startnew = []

for x in Time['start']:
    
    if x.find(':') == 1:
        hour = int(x[:1]) 
    else:
        hour = int(x[:2])
    meridian = x[-2:]   
    if (hour == 12):
        hour = 0
    if (meridian == 'PM'):
        hour += 12
    
    startnew.append(str(hour) + ':' +  x[-5:-3])
Time['start'] = Series(startnew)

endnew = []

for x in Time['end']:
    
    if x.find(':') == 1:
        hour = int(x[:1]) 
    else:
        hour = int(x[:2])
    meridian = x[-2:]   
    if (hour == 12):
        hour = 0
    if (meridian == 'PM'):
        hour += 12
    
    endnew.append(str(hour) + ':' +  x[-5:-3])
Time['end'] = Series(endnew)
Time
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
      <th>start</th>
      <th>end</th>
      <th>Time</th>
      <th>Project</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12/03/2018</td>
      <td>23:45</td>
      <td>6:31</td>
      <td>6.52</td>
      <td>Sleep</td>
    </tr>
    <tr>
      <th>1</th>
      <td>12/03/2018</td>
      <td>6:44</td>
      <td>7:28</td>
      <td>0.73</td>
      <td>Productivity - Coding</td>
    </tr>
    <tr>
      <th>2</th>
      <td>12/03/2018</td>
      <td>9:00</td>
      <td>10:00</td>
      <td>1.00</td>
      <td>Counseling</td>
    </tr>
    <tr>
      <th>3</th>
      <td>12/03/2018</td>
      <td>18:03</td>
      <td>18:23</td>
      <td>0.33</td>
      <td>Family - Quality Family Time</td>
    </tr>
    <tr>
      <th>4</th>
      <td>12/04/2018</td>
      <td>1:04</td>
      <td>7:17</td>
      <td>6.22</td>
      <td>Sleep</td>
    </tr>
    <tr>
      <th>5</th>
      <td>12/04/2018</td>
      <td>7:23</td>
      <td>7:39</td>
      <td>0.27</td>
      <td>Productivity - Reading</td>
    </tr>
    <tr>
      <th>6</th>
      <td>12/04/2018</td>
      <td>20:00</td>
      <td>20:45</td>
      <td>0.75</td>
      <td>Family - Quality Family Time</td>
    </tr>
    <tr>
      <th>7</th>
      <td>12/04/2018</td>
      <td>21:17</td>
      <td>23:14</td>
      <td>1.95</td>
      <td>Productivity - Coding</td>
    </tr>
    <tr>
      <th>8</th>
      <td>12/05/2018</td>
      <td>0:19</td>
      <td>6:30</td>
      <td>6.18</td>
      <td>Sleep</td>
    </tr>
    <tr>
      <th>9</th>
      <td>12/05/2018</td>
      <td>6:49</td>
      <td>7:24</td>
      <td>0.58</td>
      <td>Productivity - Coding</td>
    </tr>
    <tr>
      <th>10</th>
      <td>12/05/2018</td>
      <td>18:04</td>
      <td>20:15</td>
      <td>2.18</td>
      <td>Family - Quality Wife Time</td>
    </tr>
    <tr>
      <th>11</th>
      <td>12/05/2018</td>
      <td>20:15</td>
      <td>21:00</td>
      <td>0.75</td>
      <td>Family - Quality Family Time</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12/05/2018</td>
      <td>21:00</td>
      <td>23:00</td>
      <td>2.00</td>
      <td>Family - Quality Wife Time</td>
    </tr>
    <tr>
      <th>13</th>
      <td>12/05/2018</td>
      <td>23:46</td>
      <td>6:29</td>
      <td>0.23</td>
      <td>Sleep</td>
    </tr>
    <tr>
      <th>14</th>
      <td>12/06/2018</td>
      <td>23:46</td>
      <td>6:29</td>
      <td>6.48</td>
      <td>Sleep</td>
    </tr>
    <tr>
      <th>15</th>
      <td>12/06/2018</td>
      <td>6:40</td>
      <td>7:20</td>
      <td>0.67</td>
      <td>Productivity - Coding</td>
    </tr>
    <tr>
      <th>16</th>
      <td>12/06/2018</td>
      <td>16:00</td>
      <td>17:30</td>
      <td>1.50</td>
      <td>Working Out</td>
    </tr>
    <tr>
      <th>17</th>
      <td>12/06/2018</td>
      <td>20:00</td>
      <td>20:45</td>
      <td>0.75</td>
      <td>Family - Quality Family Time</td>
    </tr>
    <tr>
      <th>18</th>
      <td>12/07/2018</td>
      <td>6:45</td>
      <td>7:20</td>
      <td>0.58</td>
      <td>Productivity - Coding</td>
    </tr>
    <tr>
      <th>19</th>
      <td>12/07/2018</td>
      <td>18:00</td>
      <td>19:30</td>
      <td>1.50</td>
      <td>Family - Kids Events</td>
    </tr>
  </tbody>
</table>
</div>




```python
import pandas
from pandas import *
import datetime
#from pandas import Series
from datetime import date

TimeDate = lambda x: datetime.datetime.strptime(x, '%A, %B %d').date()

cols = ['Project','start','end' , 'Time']
Time = pandas.read_csv('C:/Users/Tonyryanworldwide/Desktop/Hours/Daily_Time_Detail_All_Projects.csv', skiprows = 2, header = 0, names =  ['Project','start','end' , 'Time'], index_col = False)


Time = Time[4:]
Time = Time[Time['Project'] != 'Total']
Time = Time[Time['Project'] != 'GRAND TOTAL']
Time = Time[Time.Project.notnull()]
Time = Time.reset_index(drop = True)
Time
Date = []
TaskDate = []
Delete_rows = []
for x in range (0 , len(Time)):
    if Time['Project'][x].find(',') > 0:
        if datetime.datetime.strptime(Time['Project'][x], '%A, %B %d').date().strftime('%m') == '12':
            year = 2018
        else:
            year = 2019
        ITSaDate = datetime.datetime.strptime(Time['Project'][x], '%A, %B %d').date().strftime('%m/%d/' + str(year))
        Delete_rows.append(x)
    else: 
        Date.append(ITSaDate)
        
Time = Time.drop(Delete_rows)
Time = Time.reset_index(drop = True)
Time['Date'] = Series(Date)
Time = Time[['Date','start', 'end','Time','Project']]
startnew = []

for x in Time['start']:
    
    if x.find(':') == 1:
        hour = int(x[:1]) 
    else:
        hour = int(x[:2])
    meridian = x[-2:]   
    if (hour == 12):
        hour = 0
    if (meridian == 'PM'):
        hour += 12
    
    startnew.append(str(hour) + ':' +  x[-5:-3])
Time['start'] = Series(startnew)

endnew = []

for x in Time['end']:
    
    if x.find(':') == 1:
        hour = int(x[:1]) 
    else:
        hour = int(x[:2])
    meridian = x[-2:]   
    if (hour == 12):
        hour = 0
    if (meridian == 'PM'):
        hour += 12
    
    endnew.append(str(hour) + ':' +  x[-5:-3])
Time['end'] = Series(endnew)


LC = pandas.read_csv('C:/Users/Tonyryanworldwide/Desktop/Hours/LC_export.csv')
LC = LC.drop(' NOTE', axis = 1)
cols = ['Start_Date', 'End_Date', 'Start_Time',
       'End_Time', 'Duration_Hours', 'Name', 'Location']
LC.columns = cols
func = lambda x: x.str.strip() if x.dtype == "object" else x
LC = LC.apply(func)
LC = LC[LC.Name != 'Home']
LC = LC[LC.Start_Date >= '12/01/2018']

LC.reset_index(drop = True,inplace = True)
func = lambda x: x.str.strip() if x.dtype == "object" else x
LC = LC.apply(func)
LC = LC[LC.Name != 'Home']
x = LC['Start_Date'][0]
#StartDate = lambda x: datetime.datetime.strptime(x,'%m/%d/%Y %M:%S').date()
StartDate = lambda x: datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S').date()
LC['Start_Date'] = LC['Start_Date'].apply(StartDate)

DateAfter = datetime.date(2018, 12, 1)
LC = LC[LC['Start_Date'] >= DateAfter]
StartDate2 = lambda x: datetime.datetime.strftime(x,'%m/%d/%Y')
LC['Start_Date'] = LC['Start_Date'].apply(StartDate2)
LC.reset_index(drop = True,inplace = True)
LC = LC.drop(['Location','End_Date'], axis = 1)
LC['Duration_Hours'] = LC['Duration_Hours'] / 3600
TrimEst = lambda x:  x[0:-4]
LC['Start_Time'] = LC['Start_Time'].apply(TrimEst)
LC['End_Time'] = LC['End_Time'].apply(TrimEst)
TimeConv = lambda x: datetime.datetime.strptime(x, '%Y-%m-%d %H:%M:%S').strftime('%H:%M')
LC['Start_Time'] = LC['Start_Time'].apply(TimeConv)
LC['End_Time'] = LC['End_Time'].apply(TimeConv)
LC
LC.columns = Time.columns
AllTime = pandas.concat([LC, Time])
AllTime.reset_index(inplace = True,drop = True)

AllTime.to_csv('C:/Users/Tonyryanworldwide/Desktop/Hours/Combined_Hours.csv')
AllTime
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
      <th>start</th>
      <th>end</th>
      <th>Time</th>
      <th>Project</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12/03/2018</td>
      <td>17:07</td>
      <td>17:17</td>
      <td>0.154444</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>1</th>
      <td>12/03/2018</td>
      <td>17:17</td>
      <td>17:35</td>
      <td>0.301944</td>
      <td>Pick  up kids</td>
    </tr>
    <tr>
      <th>2</th>
      <td>12/03/2018</td>
      <td>17:35</td>
      <td>17:40</td>
      <td>0.086389</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>3</th>
      <td>12/03/2018</td>
      <td>18:57</td>
      <td>19:12</td>
      <td>0.245000</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>4</th>
      <td>12/04/2018</td>
      <td>19:12</td>
      <td>20:14</td>
      <td>1.032222</td>
      <td>Dinner</td>
    </tr>
    <tr>
      <th>5</th>
      <td>12/04/2018</td>
      <td>20:14</td>
      <td>20:33</td>
      <td>0.305833</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>6</th>
      <td>12/04/2018</td>
      <td>21:13</td>
      <td>21:21</td>
      <td>0.133333</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>7</th>
      <td>12/04/2018</td>
      <td>21:21</td>
      <td>00:08</td>
      <td>2.784444</td>
      <td>Gym</td>
    </tr>
    <tr>
      <th>8</th>
      <td>12/04/2018</td>
      <td>00:08</td>
      <td>00:15</td>
      <td>0.121944</td>
      <td>Work</td>
    </tr>
    <tr>
      <th>9</th>
      <td>12/04/2018</td>
      <td>01:05</td>
      <td>06:30</td>
      <td>5.403056</td>
      <td>Sleep</td>
    </tr>
    <tr>
      <th>10</th>
      <td>12/04/2018</td>
      <td>08:24</td>
      <td>08:40</td>
      <td>0.275278</td>
      <td>Commute to work</td>
    </tr>
    <tr>
      <th>11</th>
      <td>12/04/2018</td>
      <td>08:24</td>
      <td>08:40</td>
      <td>0.275000</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12/04/2018</td>
      <td>08:40</td>
      <td>18:08</td>
      <td>9.451667</td>
      <td>Work</td>
    </tr>
    <tr>
      <th>13</th>
      <td>12/04/2018</td>
      <td>18:08</td>
      <td>18:44</td>
      <td>0.611944</td>
      <td>Commute to home</td>
    </tr>
    <tr>
      <th>14</th>
      <td>12/04/2018</td>
      <td>18:08</td>
      <td>18:44</td>
      <td>0.586111</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>15</th>
      <td>12/05/2018</td>
      <td>00:19</td>
      <td>06:30</td>
      <td>6.185556</td>
      <td>Sleep</td>
    </tr>
    <tr>
      <th>16</th>
      <td>12/05/2018</td>
      <td>08:04</td>
      <td>08:23</td>
      <td>0.306944</td>
      <td>Commute to work</td>
    </tr>
    <tr>
      <th>17</th>
      <td>12/05/2018</td>
      <td>08:04</td>
      <td>08:23</td>
      <td>0.306667</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>18</th>
      <td>12/05/2018</td>
      <td>08:23</td>
      <td>17:54</td>
      <td>9.512222</td>
      <td>Work</td>
    </tr>
    <tr>
      <th>19</th>
      <td>12/05/2018</td>
      <td>17:54</td>
      <td>18:18</td>
      <td>0.413056</td>
      <td>Commute to home</td>
    </tr>
    <tr>
      <th>20</th>
      <td>12/05/2018</td>
      <td>17:54</td>
      <td>18:18</td>
      <td>0.412778</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>21</th>
      <td>12/06/2018</td>
      <td>23:45</td>
      <td>06:22</td>
      <td>6.606944</td>
      <td>Sleep</td>
    </tr>
    <tr>
      <th>22</th>
      <td>12/06/2018</td>
      <td>08:01</td>
      <td>08:25</td>
      <td>0.413056</td>
      <td>Commute to work</td>
    </tr>
    <tr>
      <th>23</th>
      <td>12/06/2018</td>
      <td>08:01</td>
      <td>08:05</td>
      <td>0.066944</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>24</th>
      <td>12/06/2018</td>
      <td>08:05</td>
      <td>08:11</td>
      <td>0.098889</td>
      <td>Pick  up kids</td>
    </tr>
    <tr>
      <th>25</th>
      <td>12/06/2018</td>
      <td>08:12</td>
      <td>08:23</td>
      <td>0.182222</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>26</th>
      <td>12/06/2018</td>
      <td>08:23</td>
      <td>08:25</td>
      <td>0.046667</td>
      <td>Walk</td>
    </tr>
    <tr>
      <th>27</th>
      <td>12/06/2018</td>
      <td>08:25</td>
      <td>15:17</td>
      <td>6.858333</td>
      <td>Work</td>
    </tr>
    <tr>
      <th>28</th>
      <td>12/06/2018</td>
      <td>15:17</td>
      <td>15:43</td>
      <td>0.426111</td>
      <td>Commute to home</td>
    </tr>
    <tr>
      <th>29</th>
      <td>12/06/2018</td>
      <td>15:18</td>
      <td>15:43</td>
      <td>0.402500</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>33</th>
      <td>12/07/2018</td>
      <td>23:39</td>
      <td>06:30</td>
      <td>6.845278</td>
      <td>Sleep</td>
    </tr>
    <tr>
      <th>34</th>
      <td>12/07/2018</td>
      <td>08:15</td>
      <td>08:35</td>
      <td>0.333333</td>
      <td>Commute to work</td>
    </tr>
    <tr>
      <th>35</th>
      <td>12/07/2018</td>
      <td>08:15</td>
      <td>08:19</td>
      <td>0.060000</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>36</th>
      <td>12/07/2018</td>
      <td>08:19</td>
      <td>08:24</td>
      <td>0.087222</td>
      <td>Pick  up kids</td>
    </tr>
    <tr>
      <th>37</th>
      <td>12/07/2018</td>
      <td>08:24</td>
      <td>08:35</td>
      <td>0.185833</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>38</th>
      <td>12/07/2018</td>
      <td>08:35</td>
      <td>17:41</td>
      <td>9.099167</td>
      <td>Work</td>
    </tr>
    <tr>
      <th>39</th>
      <td>12/07/2018</td>
      <td>17:43</td>
      <td>18:00</td>
      <td>0.297222</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>40</th>
      <td>12/08/2018</td>
      <td>19:41</td>
      <td>19:51</td>
      <td>0.167500</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>41</th>
      <td>12/08/2018</td>
      <td>20:02</td>
      <td>20:16</td>
      <td>0.229167</td>
      <td>Transport</td>
    </tr>
    <tr>
      <th>42</th>
      <td>12/08/2018</td>
      <td>00:40</td>
      <td>07:54</td>
      <td>7.230278</td>
      <td>Sleep</td>
    </tr>
    <tr>
      <th>43</th>
      <td>12/03/2018</td>
      <td>23:45</td>
      <td>6:31</td>
      <td>6.520000</td>
      <td>Sleep</td>
    </tr>
    <tr>
      <th>44</th>
      <td>12/03/2018</td>
      <td>6:44</td>
      <td>7:28</td>
      <td>0.730000</td>
      <td>Productivity - Coding</td>
    </tr>
    <tr>
      <th>45</th>
      <td>12/03/2018</td>
      <td>9:00</td>
      <td>10:00</td>
      <td>1.000000</td>
      <td>Counseling</td>
    </tr>
    <tr>
      <th>46</th>
      <td>12/03/2018</td>
      <td>18:03</td>
      <td>18:23</td>
      <td>0.330000</td>
      <td>Family - Quality Family Time</td>
    </tr>
    <tr>
      <th>47</th>
      <td>12/04/2018</td>
      <td>1:04</td>
      <td>7:17</td>
      <td>6.220000</td>
      <td>Sleep</td>
    </tr>
    <tr>
      <th>48</th>
      <td>12/04/2018</td>
      <td>7:23</td>
      <td>7:39</td>
      <td>0.270000</td>
      <td>Productivity - Reading</td>
    </tr>
    <tr>
      <th>49</th>
      <td>12/04/2018</td>
      <td>20:00</td>
      <td>20:45</td>
      <td>0.750000</td>
      <td>Family - Quality Family Time</td>
    </tr>
    <tr>
      <th>50</th>
      <td>12/04/2018</td>
      <td>21:17</td>
      <td>23:14</td>
      <td>1.950000</td>
      <td>Productivity - Coding</td>
    </tr>
    <tr>
      <th>51</th>
      <td>12/05/2018</td>
      <td>0:19</td>
      <td>6:30</td>
      <td>6.180000</td>
      <td>Sleep</td>
    </tr>
    <tr>
      <th>52</th>
      <td>12/05/2018</td>
      <td>6:49</td>
      <td>7:24</td>
      <td>0.580000</td>
      <td>Productivity - Coding</td>
    </tr>
    <tr>
      <th>53</th>
      <td>12/05/2018</td>
      <td>18:04</td>
      <td>20:15</td>
      <td>2.180000</td>
      <td>Family - Quality Wife Time</td>
    </tr>
    <tr>
      <th>54</th>
      <td>12/05/2018</td>
      <td>20:15</td>
      <td>21:00</td>
      <td>0.750000</td>
      <td>Family - Quality Family Time</td>
    </tr>
    <tr>
      <th>55</th>
      <td>12/05/2018</td>
      <td>21:00</td>
      <td>23:00</td>
      <td>2.000000</td>
      <td>Family - Quality Wife Time</td>
    </tr>
    <tr>
      <th>56</th>
      <td>12/05/2018</td>
      <td>23:46</td>
      <td>6:29</td>
      <td>0.230000</td>
      <td>Sleep</td>
    </tr>
    <tr>
      <th>57</th>
      <td>12/06/2018</td>
      <td>23:46</td>
      <td>6:29</td>
      <td>6.480000</td>
      <td>Sleep</td>
    </tr>
    <tr>
      <th>58</th>
      <td>12/06/2018</td>
      <td>6:40</td>
      <td>7:20</td>
      <td>0.670000</td>
      <td>Productivity - Coding</td>
    </tr>
    <tr>
      <th>59</th>
      <td>12/06/2018</td>
      <td>16:00</td>
      <td>17:30</td>
      <td>1.500000</td>
      <td>Working Out</td>
    </tr>
    <tr>
      <th>60</th>
      <td>12/06/2018</td>
      <td>20:00</td>
      <td>20:45</td>
      <td>0.750000</td>
      <td>Family - Quality Family Time</td>
    </tr>
    <tr>
      <th>61</th>
      <td>12/07/2018</td>
      <td>6:45</td>
      <td>7:20</td>
      <td>0.580000</td>
      <td>Productivity - Coding</td>
    </tr>
    <tr>
      <th>62</th>
      <td>12/07/2018</td>
      <td>18:00</td>
      <td>19:30</td>
      <td>1.500000</td>
      <td>Family - Kids Events</td>
    </tr>
  </tbody>
</table>
<p>63 rows × 5 columns</p>
</div>


