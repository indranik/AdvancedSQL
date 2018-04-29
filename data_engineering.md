

```python
# Dependencies
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

import csv
import os

# Imports the method used for connecting to DBs
from sqlalchemy import create_engine, MetaData

# Imports the methods needed to abstract classes into tables
from sqlalchemy.ext.declarative import declarative_base

# Allow us to declare column types
from sqlalchemy import Column, Integer, String, Float

```


```python
# Read CSVs
stationDf = pd.read_csv("hawaii_stations.csv")
measurementsDf = pd.read_csv("hawaii_measurements.csv")
measurementsDf.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>station</th>
      <th>date</th>
      <th>prcp</th>
      <th>tobs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>USC00519397</td>
      <td>2010-01-01</td>
      <td>0.08</td>
      <td>65</td>
    </tr>
    <tr>
      <th>1</th>
      <td>USC00519397</td>
      <td>2010-01-02</td>
      <td>0.00</td>
      <td>63</td>
    </tr>
    <tr>
      <th>2</th>
      <td>USC00519397</td>
      <td>2010-01-03</td>
      <td>0.00</td>
      <td>74</td>
    </tr>
    <tr>
      <th>3</th>
      <td>USC00519397</td>
      <td>2010-01-04</td>
      <td>0.00</td>
      <td>76</td>
    </tr>
    <tr>
      <th>4</th>
      <td>USC00519397</td>
      <td>2010-01-06</td>
      <td>NaN</td>
      <td>73</td>
    </tr>
  </tbody>
</table>
</div>




```python
measurementsCleanDf = measurementsDf.dropna()
measurementsCleanDf = measurementsCleanDf.reset_index()

stationDf.to_csv("clean_hawaii_stations.csv",index = False)
measurementsCleanDf.to_csv("clean_hawaii_measurements.csv", index = False)

```


```python
# Database creation
import sqlalchemy
# Imports the method used for connecting to DBs
from sqlalchemy import create_engine

# Imports the methods needed to abstract classes into tables
from sqlalchemy.ext.declarative import declarative_base

# Allow us to declare column types
from sqlalchemy import Column, Integer, String, Float, Text
```


```python
# Sets an object to utilize the default declarative base in SQL Alchemy
Base = declarative_base()
```


```python
# Create an engine to a SQLite database file called `hawaii.sqlite`
engine = create_engine("sqlite:///hawaii.sqlite")
```


```python
# Create a connection to the engine called `conn`
conn = engine.connect()
```


```python
# Creating two classes - Station and Meausrement
class Station (Base):
    __tablename__ = 'Station'

    id = Column(Integer, primary_key=True)
    station = Column(String)
    name = Column(String)
    latitude = Column(Float)
    longitude = Column(Float)
    elevation = Column(Float)
    
    def __repr__(self):
        return f"id={self.id}, name={self.name}"
    
class Measurement (Base):
    __tablename__ = 'Measurement'

    id = Column(Integer, primary_key=True)
    station = Column(String)
    date = Column(String)
    prcp = Column(Float)
    tobs = Column(Float)
   
    
    def __repr__(self):
        return f"id={self.id}, name={self.name}"
```


```python
# Use `create_all` to create the above tables in the database
Base.metadata.create_all(engine)
```


```python
# loading cleaned CSVs into dataframes
cleanStationDf = pd.read_csv("clean_hawaii_stations.csv")
stationData = cleanStationDf.to_dict(orient='records')

cleanMeasurementDf =pd.read_csv("clean_hawaii_measurements.csv")
measurementData = cleanMeasurementDf.to_dict(orient='records')

```

    [{'index': 0, 'station': 'USC00519397', 'date': '2010-01-01', 'prcp': 0.08, 'tobs': 65}, {'index': 1, 'station': 'USC00519397', 'date': '2010-01-02', 'prcp': 0.0, 'tobs': 63}, {'index': 2, 'station': 'USC00519397', 'date': '2010-01-03', 'prcp': 0.0, 'tobs': 74}, {'index': 3, 'station': 'USC00519397', 'date': '2010-01-04', 'prcp': 0.0, 'tobs': 76}, {'index': 5, 'station': 'USC00519397', 'date': '2010-01-07', 'prcp': 0.06, 'tobs': 70}, {'index': 6, 'station': 'USC00519397', 'date': '2010-01-08', 'prcp': 0.0, 'tobs': 64}, {'index': 7, 'station': 'USC00519397', 'date': '2010-01-09', 'prcp': 0.0, 'tobs': 68}, {'index': 8, 'station': 'USC00519397', 'date': '2010-01-10', 'prcp': 0.0, 'tobs': 73}, {'index': 9, 'station': 'USC00519397', 'date': '2010-01-11', 'prcp': 0.01, 'tobs': 64}, {'index': 10, 'station': 'USC00519397', 'date': '2010-01-12', 'prcp': 0.0, 'tobs': 61}, {'index': 11, 'station': 'USC00519397', 'date': '2010-01-14', 'prcp': 0.0, 'tobs': 66}, {'index': 12, 'station': 'USC00519397', 'date': '2010-01-15', 'prcp': 0.0, 'tobs': 65}, {'index': 13, 'station': 'USC00519397', 'date': '2010-01-16', 'prcp': 0.0, 'tobs': 68}, {'index': 14, 'station': 'USC00519397', 'date': '2010-01-17', 'prcp': 0.0, 'tobs': 64}, {'index': 15, 'station': 'USC00519397', 'date': '2010-01-18', 'prcp': 0.0, 'tobs': 72}, {'index': 16, 'station': 'USC00519397', 'date': '2010-01-19', 'prcp': 0.0, 'tobs': 66}, {'index': 17, 'station': 'USC00519397', 'date': '2010-01-20', 'prcp': 0.0, 'tobs': 66}, {'index': 18, 'station': 'USC00519397', 'date': '2010-01-21', 'prcp': 0.0, 'tobs': 69}, {'index': 19, 'station': 'USC00519397', 'date': '2010-01-22', 'prcp': 0.0, 'tobs': 67}, {'index': 20, 'station': 'USC00519397', 'date': '2010-01-23', 'prcp': 0.0, 'tobs': 67}, {'index': 21, 'station': 'USC00519397', 'date': '2010-01-24', 'prcp': 0.01, 'tobs': 71}, {'index': 22, 'station': 'USC00519397', 'date': '2010-01-25', 'prcp': 0.0, 'tobs': 67}, {'index': 23, 'station': 'USC00519397', 'date': '2010-01-26', 'prcp': 0.04, 'tobs': 76}, {'index': 24, 'station': 'USC00519397', 'date': '2010-01-27', 'prcp': 0.12, 'tobs': 68}, {'index': 25, 'station': 'USC00519397', 'date': '2010-01-28', 'prcp': 0.0, 'tobs': 72}, {'index': 27, 'station': 'USC00519397', 'date': '2010-01-31', 'prcp': 0.03, 'tobs': 67}, {'index': 28, 'station': 'USC00519397', 'date': '2010-02-01', 'prcp': 0.01, 'tobs': 66}, {'index': 30, 'station': 'USC00519397', 'date': '2010-02-04', 'prcp': 0.01, 'tobs': 69}, {'index': 31, 'station': 'USC00519397', 'date': '2010-02-05', 'prcp': 0.0, 'tobs': 67}, {'index': 32, 'station': 'USC00519397', 'date': '2010-02-06', 'prcp': 0.0, 'tobs': 67}, {'index': 33, 'station': 'USC00519397', 'date': '2010-02-07', 'prcp': 0.0, 'tobs': 64}, {'index': 34, 'station': 'USC00519397', 'date': '2010-02-08', 'prcp': 0.0, 'tobs': 69}, {'index': 35, 'station': 'USC00519397', 'date': '2010-02-09', 'prcp': 0.0, 'tobs': 73}, {'index': 36, 'station': 'USC00519397', 'date': '2010-02-11', 'prcp': 0.0, 'tobs': 73}, {'index': 37, 'station': 'USC00519397', 'date': '2010-02-12', 'prcp': 0.02, 'tobs': 69}, {'index': 38, 'station': 'USC00519397', 'date': '2010-02-13', 'prcp': 0.01, 'tobs': 69}, {'index': 39, 'station': 'USC00519397', 'date': '2010-02-14', 'prcp': 0.0, 'tobs': 69}, {'index': 40, 'station': 'USC00519397', 'date': '2010-02-15', 'prcp': 0.0, 'tobs': 71}, {'index': 41, 'station': 'USC00519397', 'date': '2010-02-16', 'prcp': 0.0, 'tobs': 61}, {'index': 42, 'station': 'USC00519397', 'date': '2010-02-17', 'prcp': 0.0, 'tobs': 69}, {'index': 44, 'station': 'USC00519397', 'date': '2010-02-20', 'prcp': 0.03, 'tobs': 64}, {'index': 45, 'station': 'USC00519397', 'date': '2010-02-21', 'prcp': 0.0, 'tobs': 65}, {'index': 46, 'station': 'USC00519397', 'date': '2010-02-22', 'prcp': 0.0, 'tobs': 67}, {'index': 47, 'station': 'USC00519397', 'date': '2010-02-23', 'prcp': 0.0, 'tobs': 68}, {'index': 48, 'station': 'USC00519397', 'date': '2010-02-24', 'prcp': 0.0, 'tobs': 65}, {'index': 49, 'station': 'USC00519397', 'date': '2010-02-25', 'prcp': 0.0, 'tobs': 76}, {'index': 50, 'station': 'USC00519397', 'date': '2010-02-26', 'prcp': 0.0, 'tobs': 75}, {'index': 51, 'station': 'USC00519397', 'date': '2010-02-28', 'prcp': 0.0, 'tobs': 66}, {'index': 52, 'station': 'USC00519397', 'date': '2010-03-01', 'prcp': 0.01, 'tobs': 70}, {'index': 53, 'station': 'USC00519397', 'date': '2010-03-02', 'prcp': 0.0, 'tobs': 72}]
    


```python
# Use MetaData from SQLAlchemy to reflect the tables
metadata = MetaData(bind=engine)
metadata.reflect()
```


```python
# Save the reference to the station and measurement tables
stationTable = sqlalchemy.Table('Station', metadata, autoload=True)
measurementTable = sqlalchemy.Table('Measurement', metadata, autoload=True)

```


```python
# Insert the tables. Before you do that delete any pre-exsiting tables
conn.execute(stationTable.delete())
conn.execute(measurementTable.delete())

conn.execute(stationTable.insert(), stationData)
conn.execute(measurementTable.insert(), measurementData)
```




    <sqlalchemy.engine.result.ResultProxy at 0xf73d12e978>




```python
#Testing to make sure that the data transfer is done - Station
conn.execute("select * from Station limit 5").fetchall()
```




    [(1, 'USC00519397', 'WAIKIKI 717.2, HI US', 21.2716, -157.8168, 3.0),
     (2, 'USC00513117', 'KANEOHE 838.1, HI US', 21.4234, -157.8015, 14.6),
     (3, 'USC00514830', 'KUALOA RANCH HEADQUARTERS 886.9, HI US', 21.5213, -157.8374, 7.0),
     (4, 'USC00517948', 'PEARL CITY, HI US', 21.3934, -157.9751, 11.9),
     (5, 'USC00518838', 'UPPER WAHIAWA 874.3, HI US', 21.4992, -158.0111, 306.6)]




```python
#Testing to make sure that the data transfer is done - Station
conn.execute("select * from Measurement limit 50").fetchall()
```




    [(1, 'USC00519397', '2010-01-01', 0.08, 65.0),
     (2, 'USC00519397', '2010-01-02', 0.0, 63.0),
     (3, 'USC00519397', '2010-01-03', 0.0, 74.0),
     (4, 'USC00519397', '2010-01-04', 0.0, 76.0),
     (5, 'USC00519397', '2010-01-07', 0.06, 70.0),
     (6, 'USC00519397', '2010-01-08', 0.0, 64.0),
     (7, 'USC00519397', '2010-01-09', 0.0, 68.0),
     (8, 'USC00519397', '2010-01-10', 0.0, 73.0),
     (9, 'USC00519397', '2010-01-11', 0.01, 64.0),
     (10, 'USC00519397', '2010-01-12', 0.0, 61.0),
     (11, 'USC00519397', '2010-01-14', 0.0, 66.0),
     (12, 'USC00519397', '2010-01-15', 0.0, 65.0),
     (13, 'USC00519397', '2010-01-16', 0.0, 68.0),
     (14, 'USC00519397', '2010-01-17', 0.0, 64.0),
     (15, 'USC00519397', '2010-01-18', 0.0, 72.0),
     (16, 'USC00519397', '2010-01-19', 0.0, 66.0),
     (17, 'USC00519397', '2010-01-20', 0.0, 66.0),
     (18, 'USC00519397', '2010-01-21', 0.0, 69.0),
     (19, 'USC00519397', '2010-01-22', 0.0, 67.0),
     (20, 'USC00519397', '2010-01-23', 0.0, 67.0),
     (21, 'USC00519397', '2010-01-24', 0.01, 71.0),
     (22, 'USC00519397', '2010-01-25', 0.0, 67.0),
     (23, 'USC00519397', '2010-01-26', 0.04, 76.0),
     (24, 'USC00519397', '2010-01-27', 0.12, 68.0),
     (25, 'USC00519397', '2010-01-28', 0.0, 72.0),
     (26, 'USC00519397', '2010-01-31', 0.03, 67.0),
     (27, 'USC00519397', '2010-02-01', 0.01, 66.0),
     (28, 'USC00519397', '2010-02-04', 0.01, 69.0),
     (29, 'USC00519397', '2010-02-05', 0.0, 67.0),
     (30, 'USC00519397', '2010-02-06', 0.0, 67.0),
     (31, 'USC00519397', '2010-02-07', 0.0, 64.0),
     (32, 'USC00519397', '2010-02-08', 0.0, 69.0),
     (33, 'USC00519397', '2010-02-09', 0.0, 73.0),
     (34, 'USC00519397', '2010-02-11', 0.0, 73.0),
     (35, 'USC00519397', '2010-02-12', 0.02, 69.0),
     (36, 'USC00519397', '2010-02-13', 0.01, 69.0),
     (37, 'USC00519397', '2010-02-14', 0.0, 69.0),
     (38, 'USC00519397', '2010-02-15', 0.0, 71.0),
     (39, 'USC00519397', '2010-02-16', 0.0, 61.0),
     (40, 'USC00519397', '2010-02-17', 0.0, 69.0),
     (41, 'USC00519397', '2010-02-20', 0.03, 64.0),
     (42, 'USC00519397', '2010-02-21', 0.0, 65.0),
     (43, 'USC00519397', '2010-02-22', 0.0, 67.0),
     (44, 'USC00519397', '2010-02-23', 0.0, 68.0),
     (45, 'USC00519397', '2010-02-24', 0.0, 65.0),
     (46, 'USC00519397', '2010-02-25', 0.0, 76.0),
     (47, 'USC00519397', '2010-02-26', 0.0, 75.0),
     (48, 'USC00519397', '2010-02-28', 0.0, 66.0),
     (49, 'USC00519397', '2010-03-01', 0.01, 70.0),
     (50, 'USC00519397', '2010-03-02', 0.0, 72.0)]


