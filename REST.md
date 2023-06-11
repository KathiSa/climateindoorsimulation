---
layout: default
title: REST
nav_order: 4
---

# Endpoints

## Overview

|URI|Method|Description|
|-|-|-|
|http://localhost:5000/simulation|POST|Generates a new simulation ID and initiates a new simulation in the backend|
|http://localhost:5000/simulation|GET|Returns all information about a simulation|
|http://localhost:5000/simulation|DEL|Delets a simulation|
|http://localhost:5000/simulation/overview|GET|Overview of all initiated simulations|
|http://localhost:5000/simulation/control|POST|Starts simulation|
|http://localhost:5000/simulation/control|GET|Checks status of the simulation|
|http://localhost:5000/idf|POST|Upload or edit an idf file|
|http://localhost:5000/weather|POST|Upload or edit weather data in epw file|
|http://localhost:5000/occupancy|POST|Upload or edit occupancy data in csv format|
|http://localhost:5000/result|GET|Retrieving the result of a simulation|
|http://localhost:5000/result|DEL|Deletes the result of a simulation|
|http://localhost:5000/result/csv|GET|Returns the results in csv format|
|http://localhost:5000/result/overview|GET|Get an overview of all available results|
|http://localhost:5000/metadata|GET|Retrieve metadata about a simulation|
|http://localhost:5000/simulation/control/onlyidf| POST | Starts simulation with only an idf file|
|http://localhost:5000/series/create|GET/POST|Creates a simulation series|
|http://localhost:5000/series/weather|GET/POST|Upload or edit the weather file for a simulation series|
|http://localhost:5000/series/occupancy|GET/POST|Upload or edit a csv file for the occupancy for a simulation series|
|http://localhost:5000/series/idf|POST and GET?|Uploads or edit an idf file of a simulation series|
|http://localhost:5000/series/run|GET and POST?|Start a simulaiton for a series|
|http://localhost:5000/series/results|GET and POST?|Retrieving the results of a simulationseries|


## POST Simulation

URL: http://localhost:5000/simulation

Method: POST

Parameters: none

Example Request Body: 
```
{}
```

Example Respone: 
```
"63977ba6ed0627cf228854e2"
```

## GET Simulation

URL: http://localhost:5000/simulation

Method: GET

Parameters: 

|Term|Format|Description|
|-|-|-|
|ID|String|The ID of the simulation. The ID is used to query information|

Example Request Body: 
```
{
 "id": "63977ba6ed0627cf228854e2"
}
```

Example Respone: 
```
{
 "csv_data":“...8MDA6MDA6MDB8MHwwDQowfDAwOjAxOjA...“,
 "idf_filename": "SimInput14706816",
 "infiltration_rate": 0.0019,
 "length": 7.77,
 "start_day": 30,
 "start_month": 12,
 "start_year": 2022,
 "width": 6.66
}
```

Example Response for request with error: 
```
{
 "errors": {
 "success": false,
 "message": {'id': ['Field may not be null.']}
 }
}

```

## DELETE Simulation

URL: http://localhost:5000/simulation

Method: DELETE

Parameters: 

|Term|Format|Description|
|-|-|-|
|ID|String|The ID of the simulation which should be deleted|

Example Request Body: 
```
{
 "id": "63977ba6ed0627cf228854e2"
} 
```

Example Respone: 
```
{
 "success": true
}
```

Example Response for request with error: 
```
{
 "errors": {
 "success": false,
 "message": {'id': ['Field may not be null.']}
 }
}

```

## GET Overview

Overview on all simulations

URL: http://localhost:5000/simulation/overview 

Method: GET

Parameters: none

Example Request Body: 
```
{}
```

Example Response: 
```
{
 "_id": "6393475ebb23fdae39e9faf0",
 "date_of_creation": "2022-12-09-15:34",
 "idf_filename": "SimInput06397981"
 },
 {
 "_id": "63977ba6ed0627cf228854e2",
 "date_of_creation": "2022-12-12-20:06",
 "idf_filename": "SimInput14706816"
 }
```

## POST Simulation control

start a simulation

URL: http://localhost:5000/simulation/control

Method: POST

Parameters: 

|Name|Format|Description|
|-|-|-|
|id|String|ID of simulation|
|height|float|height of room|
|lenght|float|lenght of room|
|width|float|width of room|
|start_day|int|start date of simulation (day)|
|start_month|int|start date of simulation (month)|
|start_year|int|start date of simulation (year)|
|end_day|int|end date of simulation (day)|
|end_month|int|end date of simulation (month)|
|end_year|int|end date of simulation (year)|
|infiltration_rate|flaot|Infiltration rate for simulation|

Example of request body: 
```
{
    
 "id": "63977ba6ed0627cf228854e2",
 "height": 8.88,
 "length": 7.77,
 "width": 6.66,
 "start_day": 1,
 "start_month": 12,
 "start_year": 2022,
 "end_day": 31,
 "end_month": 12,
 "end_year": 2022,
 "infiltration_rate": 0.0019
}
```

Example response: 
```
{
 "success": true
}
```

## GET Simulation control

Check status of simulation

URL: http://localhost:5000/simulation/control

Method: GET

Parameter: 

|Name|Format|Description|
|-|-|-|
|id|String|ID of simulation|

Example request body: 
```
{ 
 "id": "63977ba6ed0627cf228854e2"
}
```

Example response: 
```
{
    "status": "done"
}
```

## POST IDF file

Upload of a idf file

URL: http://localhost:5000/idf

Method: POST

Parameters: 

|Name|Format|Description|
|-|-|-|
|id|String|ID of simulation|
|data|String(base64)|idf file as base64-String|

Example request body: 
{
    
 "id": "63977ba6ed0627cf228854e2",
 "data": "...MHwwfDANCjB8MDA6MTg6MDB8MH..."
}

Example for successful response

```
    "success": true
```

If request has an error, the following error message might appear (Depending on the input of the use, the tool will give a response message regarding the input): 

```
{
     "errors": {
 "success": false,
 "message": "provided id was not 24 characters long"
 }
}
```

TODO: check if successful is still the same and check for error message
TODO: add upload of idf file

## POST Weather file

Upload or Edit of Weather data (epw-file)

URL: http://localhost:5000/weather

Method: POST

Parameters: 

|Name|Format|Description|
|-|-|-|
|id|String|ID of simulation|
|data|String (base64)|epw file as base64-String|

Example request body: 

```
    "id": "63977ba6ed0627cf228854e2",
    "data": "...fDANCjB8MDA6NDU6MD..."
``` 

Example response: 

```
    "success": true
```

TODO add upload of epw file

## POST Occupancy

Upload or edit of occupancy data in csv file

URL: http://localhost:5000/occupancy

Method: POST

Parameters: 

|Name|Format|Description|
|id|String|ID of simulation|
|data|String(base64)| csv file as base64-String|

Example request body: 
```
{
     "id": "63977ba6ed0627cf228854e2",
    "data": "...B8MDE6MDM6MDB8MHwwDQowfDAx..."
}
```

Example response: 
```
{
    "success": true
}
```
TODO: add how to upload an csv file

## GET Result

Retrieve results of a simulation

URL: http://localhost:5000/result 

Method: GET

Parameters: 

|Name|Format|Description|
|id|String|ID of simulation|

Example of request body: 
```
{
    
 "id": "63977ba6ed0627cf228854e2"
}
```
Example response: 
```
{
    "_id": "639322941bf5b614046cfc70",
 "date_of_creation": "2022-12-09-12:57",
 "eso_data": "...UHJvZ3JhbSBWZXJzaW9uLE...”
 "filename": "SimOutput08740758",
 "idf_data": "... MHwwfDANCjB8MDA6MTg6MDB8MH...”
 "sim_id": "63977ba6ed0627cf228854e2",
 "status": "done"
}
```
TODO: Check response => Beispiel aus vorhandenen Handbuch stimmt nicht mehr mit aktuellen Eintrag in der Datenbank überein

## DELETE Result

## GET CSV Result

## GET Result overview

Retrieve overview from all available results

URL: http://localhost:5000/result/overview

Method: GET

Parameters: None

Example request body: 
```
{}
```

Example response: 
```
[{
 "_id": "6390d7bf988fcf505d032575",
 "date_of_creation": "2022-12-07-19:13",
 "filename": "SimOutput19305230",
 "input_simulation_id": "6390d798988fcf505d03255d",
 "status": "done"
 },
 {
 "_id": "6390dd1785dd1128d052a3bf",
 "date_of_creation": "2022-12-07-19:36",
 "filename": "SimOutput07974376",
 "input_simulation_id": "6390dced85dd1128d052a3a7",
 "status": "done"
 }
]
```

## GET Metadata

Retrieve metadata of a simulation

URL: http://localhost:5000/metadata

Method: GET

Parameters: 

|Name|Format|Description|
|-|-|-|
|id|String|Id of simulation|

Example request body: 
```
{
    "id": "63977ba6ed0627cf228854e2"
}
```

Example response: 
``` 
{
    "end_day": 31,
    "end_month": 12,
    "end_year": 2022,
    "height": 8.88,
    "infiltration_rate": 0.0019,
    "length": 7.77,
    "start_day": 30,
    "start_month": 12,
    "start_year": 2022,
    "width": 6.66
}
```
