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
|http://localhost:5000/result/overview|GET|Get an overview of all available results|
|http://localhost:5000/metadata|GET|Retrieve metadata about a simulation|

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

## POST Simulation control

## GET Simulation control

## POST IDF file

## POST Weather file

## POST Occupancy

## GET Result

## GET Result overview

## GET Metadata