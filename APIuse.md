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





