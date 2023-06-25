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
|http://localhost:5000/idf|POST|Upload or edit an idf file|
|http://localhost:5000/idf|GET|Return information about the idf file|
|http://localhost:5000/weather|POST|Upload or edit weather data in epw file|
|http://localhost:5000/weather|GET|Return all information about the epw file|
|http://localhost:5000/occupancy|POST|Upload or edit occupancy data in csv format|
|http://localhost:5000/occupancy|GET|Return information about the occupancy data in csv format|
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

Upload or edit of an idf file

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
TODO: explain string

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

Example for successful response

```
    "success": true
```

TODO: check input parameters and response 

## POST Weather file

Upload or edit Weather data (epw-file)

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
TODO: check request and response 

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
    "success": true
```

TODO add upload of epw file
TODO: check request and response 

## POST Occupancy

Upload or edit of occupancy data in csv format

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
    "success": true
}
```
TODO: add how to upload an csv file

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
