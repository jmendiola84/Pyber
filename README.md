
 OBSERVABLE TRENDS
    -
    -
    -


```python
# Dependencies
%matplotlib notebook
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn as sb
```


```python
# Read CSV
city_csv = pd.read_csv('city_data.csv')
ride_csv = pd.read_csv('ride_data.csv')
city_csv.head()
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
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nguyenbury</td>
      <td>8</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East Douglas</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Dawnfurt</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rodriguezburgh</td>
      <td>52</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
ride_csv.head()
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
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Roy</td>
      <td>2016-01-02 18:42:34</td>
      <td>17.49</td>
      <td>4036272335942</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wiseborough</td>
      <td>2016-01-21 17:35:29</td>
      <td>44.18</td>
      <td>3645042422587</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spencertown</td>
      <td>2016-07-31 14:53:22</td>
      <td>6.87</td>
      <td>2242596575892</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nguyenbury</td>
      <td>2016-07-09 04:42:44</td>
      <td>6.28</td>
      <td>1543057793673</td>
    </tr>
  </tbody>
</table>
</div>




```python
pyber_df = pd.merge(city_csv, ride_csv, on="city")
pyber_cities_df = pyber_df.groupby(['type', 'city'])
pyber_cities_df = pyber_cities_df.mean()
pyber_cities_df = pyber_cities_df.drop(['ride_id'],1)
pyber_cities_df.head()
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
      <th></th>
      <th>driver_count</th>
      <th>fare</th>
    </tr>
    <tr>
      <th>type</th>
      <th>city</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">Rural</th>
      <th>East Leslie</th>
      <td>9.0</td>
      <td>33.660909</td>
    </tr>
    <tr>
      <th>East Stephen</th>
      <td>6.0</td>
      <td>39.053000</td>
    </tr>
    <tr>
      <th>East Troybury</th>
      <td>3.0</td>
      <td>33.244286</td>
    </tr>
    <tr>
      <th>Erikport</th>
      <td>3.0</td>
      <td>30.043750</td>
    </tr>
    <tr>
      <th>Hernandezshire</th>
      <td>10.0</td>
      <td>32.002222</td>
    </tr>
  </tbody>
</table>
</div>




```python
total_rides_city_type = pyber_df.groupby(['type'])
total_rides_city_type = total_rides_city_type['ride_id'].count()
total_rides_city_type

# Labels for the sections of our pie chart
labels = ["Rural", "Suburban", "Urban"]

# The values of each section of the pie chart
sizes = [total_rides_city_type['Rural'], total_rides_city_type['Suburban'], total_rides_city_type['Urban'] ]

# The colors of each section of the pie chart
colors = ["gold", "lightskyblue", "lightcoral"]

# Tells matplotlib to seperate the "Python" section from the others
explode = (0, 0, 0.1)

# Creates the pie chart based upon the values above
# Automatically finds the percentages of each part of the pie chart
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=140)

# Tells matplotlib that we want a pie chart with equal axes
plt.axis("equal")

#Add Title to the Pie Chart
plt.title("% of Total Rides by City Type")

# Prints our pie chart to the screen
plt.show()
```


![png](output_5_0.png)



```python
total_drivers_city_type = city_csv.groupby(['type'])
total_drivers_city_type = total_drivers_city_type['driver_count'].sum()
total_drivers_city_type

# Labels for the sections of our pie chart
labels = ["Rural", "Suburban", "Urban"]

# The values of each section of the pie chart
sizes = [total_drivers_city_type['Rural'], total_drivers_city_type['Suburban'], total_drivers_city_type['Urban'] ]

# The colors of each section of the pie chart
colors = ["gold", "lightskyblue", "lightcoral"]

# Tells matplotlib to seperate the "Python" section from the others
explode = (0, 0, 0.1)

# Creates the pie chart based upon the values above
# Automatically finds the percentages of each part of the pie chart
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=140)

# Tells matplotlib that we want a pie chart with equal axes
plt.axis("equal")

#Add Title to the Pie Chart
plt.title("% of Total Drivers by City Type")

# Prints our pie chart to the screen
plt.show()
```


![png](output_6_0.png)



```python
total_fare_city_type = pyber_df.groupby(['type'])
total_fare_city_type = total_fare_city_type['fare'].sum()
total_fare_city_type

# Labels for the sections of our pie chart
labels = ["Rural", "Suburban", "Urban"]

# The values of each section of the pie chart
sizes = [total_fare_city_type['Rural'], total_fare_city_type['Suburban'], total_fare_city_type['Urban'] ]

# The colors of each section of the pie chart
colors = ["gold", "lightskyblue", "lightcoral"]

# Tells matplotlib to seperate the "Python" section from the others
explode = (0, 0, 0.1)

# Creates the pie chart based upon the values above
# Automatically finds the percentages of each part of the pie chart
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=140)

# Tells matplotlib that we want a pie chart with equal axes
plt.axis("equal")

#Add Title to the Pie Chart
plt.title("% of Total Fares by City Type")

# Prints our pie chart to the screen
plt.show()
```


![png](output_7_0.png)



```python
avg_fare_city = pyber_df.groupby(['city'])
avg_fare_city = avg_fare_city['fare'].mean()

avg_fare_city.head()
```




    city
    Alvarezhaven    23.928710
    Alyssaberg      20.609615
    Anitamouth      37.315556
    Antoniomouth    23.625000
    Aprilchester    21.981579
    Name: fare, dtype: float64




```python
rides_per_city = pyber_df.groupby(['city'])
rides_per_city = rides_per_city['ride_id'].count()
rides_per_city.head()
```




    city
    Alvarezhaven    31
    Alyssaberg      26
    Anitamouth       9
    Antoniomouth    22
    Aprilchester    19
    Name: ride_id, dtype: int64




```python

#drivers_df.head()
```


```python
#citylist = pyber_df
#citylist['type'] = citylist['type'].replace({'Rural':'1','Suburban':'2', 'Urban':'3'})

#urbanlist =

#urban_list = pyber_df.groupby(['type'])
#urban_list 
#loc[city_list['type'] == 0]

# create data
x = rides_per_city #.loc[rides_per_city['type'] == 1]
y = avg_fare_city
a = rides_per_city
b = avg_fare_city
c = rides_per_city
d = avg_fare_city


 
# use the scatter function
plt.scatter(x, y, s= drivers_per_city, c=["gold", "lightskyblue", "lightcoral"], alpha=0.5, edgecolors = "black", linewidth=1)



plt.title("Pyber Ride Sharing Data (2016)")
plt.xlabel("Total Number of Rides (Per City)")
plt.ylabel("Average Fare ($)")
#plt.legend([0,1,2], (city_list.index.values))
#,('Rural','Suburban','Urban'))
plt.show()

#["Rural","Suburban","Urban"]
```


    <IPython.core.display.Javascript object>



<img src="data:image/png;base64,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" width="640">



```python
#city_list.head()
#urban_list = city_list.loc[city_list['type'].isin(['Urban'])]
#urban_list
```


```python
#city_list['type'].replace({"Rural":'0',"Suburban":'1', "Urban":'2'})
#test = rides_per_city.loc[rides_per_city['type'] == 0]

#rural_list = pyber_df.groupby(['city'])
#rural_list['type'] = rural_list['type'].rank(method='dense') - 1
#rural_df = pd.merge(rural_list, ride_csv, on="type")

#rural_list
```


```python
#drivers_per_city = citylist.groupby(['city'])
#drivers_per_city = drivers_per_city['driver_count'].mean()
#drivers_per_city


```
