---
layout: default
title: Use of API
parent: REST API
nav_order: 2
---
(Author: Katharina Sammet) 
# Use API

Here are some examples on how to use the REST API. It is recommended to use [Jupyter Notebook](https://jupyter.org/). 

This notebook can be downloaded [here](https://github.com/KathiSa/indoorclimatesimulation/blob/main/resources/RestAPIsimulations.ipynb).


```
import requests
import base64
import json
import pandas as pd
import esoreader
import random
import time
import io
from datetime import datetime
```
if an error occurs here please check if all packages are installed

## Check if docker is running

```
response = requests.get("http://localhost:5000/status")
response.json()
```
Response: 
```
{'success': True}
```

## Create a new simulation
Create a new sim ID.  

```
response = requests.post("http://localhost:5000/simulation")
response.json()
```
Response: 
```
'64998548b04d0bcba8f22b4d'
```

Set the sim ID 

```
simID = response.json()
print("The simulation ID is", simID)
```
Response: 
```
The simulation ID is 64998548b04d0bcba8f22b4d
```
Return the information about a simulation with the sim ID from the previous request. 
```
response = requests.get("http://localhost:5000/simulation", json={"id": simID})
response.json()
```
Response: 
```
{'csv_data': '',
 'date_of_creation': '2023-06-26-14:32',
 'end_day': '',
 'end_month': '',
 'end_year': '',
 'epw_data': '',
 'height': '',
 'idf_data': '',
 'idf_filename': 'SimInput08228370',
 'infiltration_rate': '',
 'length': '',
 'orientation': '',
 'start_day': '',
 'start_month': '',
 'start_year': '',
 'width': '',
 'zone_name': ''}
```

## Uploads files for simulation

Define the path for all simulation files. If the files are in the same directory as the notebook, its only necessary to define the names of the files. Otherwise use the full path to the files. 

```
idf_file = 'example_room.idf'                    # room model
epw_file = 'DE_Munich_Theresienwiese.epw'        # weather data
csv_file = 'occupancy_1day.csv'                  # occupancy data
```
Upload files. Files need to be encoded!
```
#idf file
with open(idf_file, 'rb') as f:
    encoded_object = base64.encodebytes(f.read()).decode('utf-8')

    params = {
     "id":   simID,
     "data": encoded_object
    }
    response = requests.post("http://localhost:5000/idf", json=params)

    response.json() if response.status_code == 200 else print("Failed with status code", response.status_code)
```
```
#epw file
with open(epw_file, 'rb') as f:
    encoded_object = base64.encodebytes(f.read()).decode('utf-8')
    
    params = {
     "id":   simID,
     "data": encoded_object
    }
    response = requests.post("http://localhost:5000/weather", json=params)

    response.json() if response.status_code == 200 else print("Failed with status code", response.status_code)
```
```
#csv file
with open(csv_file, 'rb') as f:
    encoded_object = base64.encodebytes(f.read()).decode('utf-8')
    
    params = {
     "id":   simID,
     "data": encoded_object
    }
    response = requests.post("http://localhost:5000/occupancy", json=params)

    response.json() if response.status_code == 200 else print("Failed with status code", response.status_code)
```
To check if all files are uploaded check the contents in the input simulation collection. 
```
#idf file
response = requests.get("http://localhost:5000/idf", json={"id": simID})
response.json() if response.status_code == 200 else print("Failed with status code", response.status_code)
```
Response (contens of idf file): 
```
'  Version,22.2;\n\n  Timestep,6;\n\n  LifeCycleCost:Parameters,\n    Life Cycle Cost Parameters,  !- Name\n    EndOfYear,               !- Discounting Convention\n    ConstantDollar,          !- Inflation Approach\n    0.03,                    !- Real Discount Rate\n    ,                        !- Nominal Discount Rate\n    ,                        !- Inflation\n    ,                        !- Base Date Month\n    2011,                    !- Base Date Year\n    ,                        !- Service Date Month\n    2011,                    !- Service Date Year\n    25,                      !- Length of Study Period in Years\n    ,                        !- Tax rate\n    None;                    !- Depreciation Method\n\n  LifeCycleCost:UsePriceEscalation,\n    U.S. Avg  Commercial.... ..... .....
```
```
#epw file
response = requests.get("http://localhost:5000/weather", json={"id": simID})
response.json() if response.status_code == 200 else print("Failed with status code", response.status_code)
```
Response (contens of epw file): 
```
'LOCATION,Munich-Theresienwiese,BY,DEU,SRC-TMYx,108650,48.16320,11.54290,1.0,520.0\r\nDESIGN CONDITIONS,1,2021 ASHRAE Handbook -- Fundamentals - Chapter 14 Climatic Design Information,,Heating,2,-11.9,-9.2,-15.1,1.1,-10.4,-12.8,1.3,-7.9,10.3,9.2,9.2,7.1,1.8,90,0.476,Cooling,7,8.8,29.5,19.0,27.7,18.1,26.1,17.5,19.7,27.4,18.9,26.1,18.2,24.4,2.6,270,17.2,13.1,21.6,16.4,12.5,21.1,15.8,11.9,20.8,58.6,27.5,55.9,26.3,53.3,24.5,22.7,Extremes,7.8,6.4,5.3,-13.4,33.1,4.3,2.0,-16.5,34.6,-19.0,35.8,-21.4,36.9,-24.5,38.4\r\nTYPICAL/EXTREME PERIODS,6,Summer - Week Nearest Max Temperature For Period,Extreme,8/10,8/16,Summer... ..... ..... ....
```
```
#csv file/occupancy
response = requests.get("http://localhost:5000/occupancy", json={"id": simID})
response.json() if response.status_code == 200 else print("Failed with status code", response.status_code)
```
Response (contens of csv file): 
```
'day|time|occupants|win1\n0|00:00:00|0|0\n0|00:01:00|0|0\n0|00:02:00|0|0\n0|00:03:00|0|0\n0|00:04:00|0|0\n0|00:05:00|0|0\n0|00:06:00|0|0\n0|00:07:00|0|0\n0|00:08:00|0|0\n0|00:09:00|0|0\n0|00:10:00|0|0\n0|00:11:00|0|0\n0|00:12:00|0|0\n0|00:13:00|0|0\n0|00:14:00|0|0\n0|00:15:00|0|0\n0|00:16:00|0|0\n0|00:17:00|0|0\n0|00:18:00|0|0\n0|00:19:00|0|0\n0|00:20:00|0|0\n0|00:21:00|0|0\n0|00:22:00|0|0\n0|00:23:00|0|0\n0|00:24:00|0|0\n0|00:25:00|0|0\n0|00:26:00|0|0\n0|00:27:00|0|0\n0|00:28:00|0|0\n0|00:29:00|0|0\n0|00:30:00|0|0\n0|00:31:00|0|0\n0|00:32:00|0|0\n0|00:33:00|0|0\n0|00:34:00|0|0\n0|00:35:00|0|0\n0|00:36:00|0|0\n0|00:37:00|0|0\n0|00:38:00|0|0\n0|00:39:00|0|0\n0|00:40:00|0|0\n0|00:41:00|0|0\n0|00:42:00|0|0.... ..... ..... 
```

## Set parameters/metadata for simulation

Define the parameters. Set the timeframe and room parameters for the simulation. The zone name can be found in the idf file. If the zone name does not match the zone name in the idf file an energy plus error occurs. 


```
params = {
 "id": simID,
 "height": 3,
 "length": 5,
 "width": 4,
 "orientation":0,
 "start_day": 1,
 "start_month": 6,
 "start_year": 2022,
 "end_day": 1,
 "end_month": 6,
 "end_year": 2022,
 "infiltration_rate": 0.0019, 
 "zone_name": "RL_Office_27214585"
}
```

## Start a simulation

```
response = requests.post("http://localhost:5000/simulation/control", json=params)
response.json()
```
Response: 
```
{'success': True}
```




