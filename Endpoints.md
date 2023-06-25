---
layout: default
title: Endpoints
parent: REST API
nav_order: 1
---
(Author: Katharina Sammet) 
# Endpoints

## Overview

|URI|Method|Description|
|-|-|-|
|[http://localhost:5000/status](#get-status)|GET|Returns information about the status of the mongo DB in docker|
|[http://localhost:5000/simulation](#post-simulation)|POST|Generates a new simulation ID and initiates a new simulation in the backend|
|[http://localhost:5000/simulation](#get-simulation)|GET|Returns all information about a simulation|
|[http://localhost:5000/simulation](#delete-simulation)|DEL|Delets a simulation|
|[http://localhost:5000/simulation/overview](#get-overview)|GET|Overview of all initiated simulations|
|http://localhost:5000/simulation/control|POST|Starts simulation|
|http://localhost:5000/simulation/control|GET|Checks status of the simulation|
|[http://localhost:5000/idf](#post-idf-file)|POST|Upload or edit an idf file|
|[http://localhost:5000/idf](#get-idf-file)|GET|Return information about the idf file|
|[http://localhost:5000/weather](#post-weather-file)|POST|Upload or edit weather data in epw file|
|[http://localhost:5000/weather](#get-weather-file)|GET|Return all information about the epw file|
|[http://localhost:5000/occupancy](#post-occupancy)|POST|Upload or edit occupancy data in csv format|
|[http://localhost:5000/occupancy](#get-occupancy)|GET|Return information about the occupancy data in csv format|
|http://localhost:5000/result|GET|Retrieving the result of a simulation|
|http://localhost:5000/result|DEL|Deletes the result of a simulation|
|http://localhost:5000/result/csv|GET|Returns the results in csv format|
|http://localhost:5000/result/overview|GET|Get an overview of all available results|
|http://localhost:5000/metadata|GET|Retrieve metadata about a simulation|
|http://localhost:5000/simulation/control/onlyidf| POST | Starts simulation with only an idf file|
|http://localhost:5000/simulation/control/onlyidf| GET | TODO|
|http://localhost:5000/reopensim|GET| TODO|
|http://localhost:5000/reopensim|POST| TODO|
|http://localhost:5000/series/create|POST|Creates a simulation series|
|http://localhost:5000/series/weather|POST|Upload or edit the weather file for a simulation series|
|http://localhost:5000/series/occupancy|POST|Upload or edit a csv file for the occupancy for a simulation series|
|http://localhost:5000/series/idf|POST|Uploads or edit an idf file of a simulation series|
|http://localhost:5000/series/run|GET|Start a simulaiton for a series|
|http://localhost:5000/series/run|POST|Start a simulaiton for a series|
|http://localhost:5000/series/results|GET|Retrieving the results of a simulationseries|


## GET Status
Returns the status of the mongo DB in docker. If the DB in the docker is successfully running it will return true. 

URL: http://localhost:5000/status

Method: GET

Parameters: none

Example Request Body: 
```
{}
```

Example Response: 
```
 "success": True
```

If Mongo DB in docker is not running the response will be: 
```
{'errors': {'success': False,
  'message': 'Failed to connect to MongoDB. Check Docker Container!'}}
```


## POST Simulation
Generates a new simulation ID and initiates a new simulation in the backend. The simulation ID and date of creation is inserted in the DB in the collection input_simulation.

URL: http://localhost:5000/simulation

Method: POST

Parameters: none

Example Request Body: 
```
{}
```

Example Response: 
```
"63977ba6ed0627cf228854e2"
```

## GET Simulation
Returns all information about a simulation from the input_simulation collection in the database. 

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
The content of the response depends on the moment when this request is made. After the creation of the simulation id the response will only contain date of creation and idf_filename. After the user inserts the metadata and files, this variables will contain the information. 
```
{'csv_data': '',
 'date_of_creation': '2023-06-25-13:20',
 'end_day': '',
 'end_month': '',
 'end_year': '',
 'epw_data': '',
 'height': '',
 'idf_data': '',
 'idf_filename': 'SimInput02022628',
 'infiltration_rate': '',
 'length': '',
 'orientation': '',
 'start_day': '',
 'start_month': '',
 'start_year': '',
 'width': '',
 'zone_name': ''}
```

Example Response for request with error: 
```
{'errors': {'success': False,
  'message': {'id': ['Missing data for required field.']}}}
```

## DELETE Simulation
Delets a simulation from the input_simulation collection in the database. 

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
 "success": True
}
```

Example Response for request with error: 
```
{'errors': {'success': False,
  'message': 'The provided simID is not a valid simID in the database. simID does not exist'}}

```

## GET Overview

Overview of all initiated simulations

URL: http://localhost:5000/simulation/overview 

Method: GET

Parameters: none

Example Request Body: 
```
{}
```

Example Response: 
```
[{'_id': '643072a6b167660e37cb196f',
  'date_of_creation': '2023-04-07-21:44',
  'idf_filename': 'SimInput38925173'},
 {'_id': '6430832b8937612757b4ffbd',
  'date_of_creation': '2023-04-07-22:55',
  'idf_filename': 'SimInput07572191'},
 {'_id': '64359a098937612757b4ffcc',
  'date_of_creation': '2023-04-11-19:34',
  'idf_filename': 'SimInput01535043'},
 {'_id': '6436b4388937612757b4ffce',
  'date_of_creation': '2023-04-12-15:38',
  'idf_filename': 'SimInput00868007'},
.......
.......
]
```

## POST Simulation control

Starts a new simulation

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

Checks status of the simulation

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

Upload or edit an idf file. 

IMPORTANT: String need to be a base64-String!

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
    "success": True
```

## GET IDF file

Returns information of an idf file in the input simulation database. 

URL: http://localhost:5000/idf

Method: GET

Parameters: 

|Name|Format|Description|
|-|-|-|
|id|String|ID of simulation|

Example request body: 
{
    
 "id": "63977ba6ed0627cf228854e2",
}

Example for successful response (contens of the idf file)

```
'  Version,22.2;\n\n  Timestep,6;\n\n  LifeCycleCost:Parameters,\n    Life Cycle Cost Parameters,  !- Name\n    EndOfYear,               !- Discounting Convention\n    ConstantDollar,          !- Inflation Approach\n    0.03,                    !- Real Discount Rate\n    ,                        !- Nominal Discount Rate\n    ,                        !- Inflation\n    ,                        !- Base Date Month\n    2011,                    !- Base Date Year\n    ,                        !- Service Date Month\n    2011,                    !- Service Date Year\n    25,                      !- Length of Study Period in Years\n    ,                        !- Tax rate\n    None;                    !- Depreciation Method\n\n  LifeCycleCost:UsePriceEscalation,\n    U.S. Avg  Commercial-Electricity,  !- LCC Price Escal........ ....... .....
```

## POST Weather file

Upload or edit Weather data (epw-file).

IMPORTANT: String need to be a base64-String!

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
    "success": True
```

## GET Weather file

Returns information about weather data (epw-file)

URL: http://localhost:5000/weather

Method: GET

Parameters: 

|Name|Format|Description|
|-|-|-|
|id|String|ID of simulation|

Example request body: 

```
    "id": "63977ba6ed0627cf228854e2",
``` 

Example response: 

```
'LOCATION,Munich-Theresienwiese,BY,DEU,SRC-TMYx,108650,48.16320,11.54290,1.0,520.0\r\nDESIGN CONDITIONS,1,2021 ASHRAE Handbook -- Fundamentals - Chapter 14 Climatic Design Information,,Heating,2,-11.9,-9.2,-15.1,1.1,-10.4,-12.8,1.3,-7.9,10.3,9.2,9.2,7.1,1.8,90,0.476,Cooling,7,8.8,29.5,19.0,27.7,18.1,26.1,17.5,19.7,27.4,18.9,26.1,18.2,24.4,2.6,270,17.2,13.1,21.6,16.4,12.5,21.1,15.8,11.9,20.8,58.6,27.5,55.9,26.3,53.3,24.5,22.7,Extremes,7.8,6.4,5.3,-13.4,33.1,4.3,2.0,-16.5,34.6,-19.0,35.8,-21.4,36.9,-24.5,38.4\r\nTYPICAL/EXTREME PERIODS,6,Summer - Week Nearest Max Temperature For Period,Extreme,8/10,8/16,Summer - Week Nearest Average Temperature For Period,Typical,8/ 3,8/ 9,Winter - Week Nearest Min Temperature For Period,Extreme,12/ 8,12/14,Winter - Week Nearest Average Temperature For Period,Typical,2/17,2/23,Autumn - Week Nearest Average Temperature For Period,Typical,9/29,10/ 5,Spr..... ...... .......
```

## POST Occupancy

Upload or edit of occupancy data in csv format.

IMPORTANT: String need to be a base64-String!

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

## GET Occupancy

Returns information about occupancy data in csv format

URL: http://localhost:5000/occupancy

Method: GET

Parameters: 

|Name|Format|Description|
|id|String|ID of simulation|

Example request body: 
```
{
     "id": "63977ba6ed0627cf228854e2",
}
```

Example response: 
```
{
   'day|time|occupants|win1\n0|00:00:00|0|0\n0|00:01:00|0|0\n0|00:02:00|0|0\n0|00:03:00|0|0\n0|00:04:00|0|0\n0|00:05:00|0|0\n0|00:06:00|0|0\n0|00:07:00|0|0\n0|00:08:00|0|0\n0|00:09:00|0|0\n0|00:10:00|0|0\n0|00:11:00|0|0\n0|00:12:00|0|0\n0|00:13:00|0|0\n0|00:14:00|0|0\n0|00:15:00|0|0\n0|00:16:00|0|0\n0|00:17:00|0|0\n0|00:18:00|0|0\n0|00:19:00|0|0\n0|00:20:00|0|0\n0|00:21:00|0|0\n0|00:22:00|0|0\n0|00:23:00|0|0\n0|00:24:00|0|0\n0|00:25:00|0|0\n0|00:26:00|0|0\n0|00:27:00|0|0\n0|00:28:00|0|0\n0|00:29:00|0|0\n0|00:30:00|0|0\n0|00:31:00|0|0\n0|00:32:00|0|0\n0|00:33:00|0|0\n0|00:34:00|0|0\n0|00:35:00|0|0\n0|00:36:00|0|0\n0|00:37:00|0|0\n0|00:38:00|0|0\n0|00:39:00|0|0\n0|00:40:00|0|0\n0|00:41:00|0|0\n0|00:42:00|0|0\n0|00:43:00|0|0\n0|00:44:00|0|0\n0|00:45:00|0|0\n0|00:46:...... ...... ......
}
```

## GET Result

Retrieve results of a simulation

TODO: database/input/results? 

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

Deletes results of a simulation in the result_simulation database. 

URL: http://localhost:5000/result 

Method: DELETE

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
  
}
```

TODO: check response 

## GET CSV Result

Returns the results in csv format. 

URL: http://localhost:5000/result/csv 

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
  
}
```

TODO: check response 

## GET Result overview

Get an overview of all available results. 

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

TODO: check response 

## GET Metadata

Retrieve metadata of a simulation.

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

TODO: update response

## POST Simulation control onlyidf

Start a simulation with only an idf and epw file. 

URL: http://localhost:5000/simulation/control/onlyidf

Method: POST

Parameters: TODO

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

}
```

TODO: check request and response 

## GET Simulation control onlyidf

TODO

URL: http://localhost:5000/simulation/control/onlyidf

Method: GET

Parameters: TODO

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

}
```

TODO: check request and response 

## GET reopensim

TODO

URL: http://localhost:5000/reopensim

Method: GET

Parameters: TODO

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

}
```

TODO: check request and response

|http://localhost:5000/series/create|POST|Creates a simulation series|
|http://localhost:5000/series/weather|POST|Upload or edit the weather file for a simulation series|
|http://localhost:5000/series/occupancy|POST|Upload or edit a csv file for the occupancy for a simulation series|
|http://localhost:5000/series/idf|POST|Uploads or edit an idf file of a simulation series|
|http://localhost:5000/series/run|GET|Start a simulaiton for a series|
|http://localhost:5000/series/run|POST|Start a simulaiton for a series|
|http://localhost:5000/series/results|GET|Retrieving the results of a simulationseries|

## POST reopensim

TODO

URL: http://localhost:5000/reopensim

Method: POST

Parameters: TODO

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

}
```

TODO: check request and response

## POST Series create

Creates a simulation series

URL: http://localhost:5000/series/create

Method: POST

Parameters: TODO

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

}
```

TODO: check request and response

## POST Series weather

Upload or edit the weather file for a simulation series

URL: http://localhost:5000/series/weather

Method: POST

Parameters: TODO

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

}
```

TODO: check request and response

## POST Series occupancy

Upload or edit a csv file for the occupancy for a simulation series

URL: http://localhost:5000/series/occupancy

Method: POST

Parameters: TODO

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

}
```

TODO: check request and response

## POST Series idf

Uploads or edit an idf file of a simulation series

URL: http://localhost:5000/series/idf

Method: POST

Parameters: TODO

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

}
```

TODO: check request and response

## GET Series run

Start a simulaiton for a series

URL: http://localhost:5000/series/run

Method: GET

Parameters: TODO

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

}
```

TODO: check request and response

## POST Series run

Start a simulaiton for a series

URL: http://localhost:5000/series/run

Method: POST

Parameters: TODO

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

}
```

TODO: check request and response


## GET Series results

Retrieving the results of a simulationseries

URL: http://localhost:5000/series/results

Method: GET

Parameters: TODO

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

}
```

TODO: check request and response
