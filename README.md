# pyber


```python
# Based on the data more than 62% of the fares were originated from urban desginsted cities. 
```


```python
# Based on the data more than 68% of the rides were generated from urban desginsted cities. 
```


```python
# Based on the data more than 86% of the drivers work within urban desginsted cities. 
```

# Pyber Ride Sharing 


```python
import pandas as pd
import csv
import numpy as np
import seaborn as sns
import glob
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
glob.glob('*.csv')
```




    ['city_data.csv', 'ride_data.csv']




```python
_city=pd.read_csv('city_data.csv')
ride=pd.read_csv('ride_data.csv') 
joined=pd.merge(_city, ride, on='city', how='outer')
joined.shape
```




    (2375, 6)




```python
joined['type_color'] = joined['type'].map(lambda x: 'gold' if x == 'Urban' else('lightcoral' if x == 'Suburban' else 'lightskyblue'))  

# adding a column to differentiate  each city by color.
```


```python
avgfarebycity=joined.groupby('city')['fare'].mean() # Average fare/city
totalridebycity=joined.groupby('city')['ride_id'].count() # Total ride per city
drivertotalbycity=joined.groupby('city')['driver_count'].sum() # driver count by city
```


```python
citybytype=joined.groupby(['city','type','type_color'])[['ride_id']].count().reset_index().set_index("city")
# Creates a df from the existing df based on the above filters. 
```


```python
scatter = plt.figure()
ax = scatter.add_subplot(1,1,1)
ax.scatter(x=totalridebycity, y=avgfarebycity, s=drivertotalbycity, c=citybytype.type_color, alpha=.5)
ax.grid()
#plt.legend(loc='best')
ax.set_title('Total Ride By City',fontweight='bold')
ax.set_xlabel('Total Driver By city',fontweight='bold')
ax.set_ylabel('Average Fare By city',fontweight='bold')
plt.suptitle('Note:')
plt.suptitle('Circle size correlates with driver count per city.')
plt.title('Pyber Ride Sharing Data(2016)',fontweight='bold');
#add legend 
```


![png](output_10_0.png)


# Total Fares by City Type


```python
totalfarebycitytype=joined.groupby('type')['fare'].sum() # total fare by city type
labels = 'Rural','Suburban', 'Urban'
sizes = [15, 45, 30]
explode = (0.1, 0, 0.1) 
colors = ['gold', 'lightskyblue', 'lightcoral']
plt.pie(totalfarebycitytype, labels=labels, colors=colors, explode=explode, autopct='%1.1f%%',shadow=True, startangle=270);
plt.title('% of Total Fares by City Type',fontweight='bold');
```


![png](output_12_0.png)


# Total Rides by City Type


```python
totalridebycitytype=joined.groupby('type')['ride_id'].count() # total fare by city type
labels = 'Rural','Suburban', 'Urban'
sizes = [15, 45, 30]
explode = (0.1, 0, 0.1) 
colors = ['gold', 'lightskyblue', 'lightcoral']
plt.pie(totalridebycitytype, labels=labels, colors=colors, explode=explode, autopct='%1.1f%%',shadow=True, startangle=90);
plt.title('% of Total Rides by City Type',fontweight='bold');
```


![png](output_14_0.png)


# Total Drivers by City Type


```python
totaldriverbycitytype=joined.groupby('type')['driver_count'].sum() # total fare by city type
labels = 'Rural','Suburban', 'Urban'
sizes = [15, 45, 30]
explode = (0.1, 0, 0.1) 
colors = ['gold', 'lightskyblue', 'lightcoral']
plt.pie(totaldriverbycitytype, labels=labels, colors=colors, explode=explode, autopct='%1.1f%%',shadow=True, startangle=180);
plt.title('% of Total Drivers by City Type',fontweight='bold');
```


![png](output_16_0.png)

