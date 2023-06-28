---
layout: default
title: Installation
nav_order: 2
---
(Author: Diana Marjanovic) 
# Installation

Requirements to install the software Indoor Climate Simulation:

* [Installation of Python 3.10](https://www.python.org/)
* [Installation of EnergyPlus (Version 22-2-0 or newer)](https://energyplus.net/)
* [Current Version of Docker](https://www.docker.com/)
* Windows or Linux OS (Recommended: Ubuntu or Mint)
* [Installation of Node.js](https://learn.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-on-windows)

## Instructions

To install the software for the first time, the "install.py" script in the indoor-climate-simultion folder must be called with the "python install.py" command. To do this, the user must be in the project folder and not call this script from parent/subordinate folders. The script then creates a VENV named "indoor-climate-simultion" using .bat files and the Windows command prompt and activates it. After that, all required packages will be installed into the virtual environment using pip. Once these steps are completed, the Docker image "mongo" is downloaded in the current version of Docker Hub. After the download is complete, a Docker container is created with this image named "simulation_db". If these steps are done, the frontend and backend will be started. At the end of the complete installation, two cmd shells should be open in which the two program parts run. Under the port localhost:100 the GUI should be accessible and usable.  To start the software after the installation has been completed, it is also necessary to change to the project folder. Afterwards it must be ensured that the VENV "indoor-climate-simulation" is activated before the script "install.py" is called again. The script will only start the created container "simulation_db" and both frontend and backend. The program should then be usable without a complete reinstallation. After installing or restarting the program, two Python applications and a MongoDB Docker container should be running on the machine. Different network ports are blocked by the software at this point. Port 100 runs the frontend based on the Flask framework, while port 5000 is where the REST API, also based on Flask, waits for requests. The MongoDB container also blocks port 27017 and the Node.js server, running the visualization is on port 50715.

## Note 
The script performs either a complete installation or just a start of the resources and the program. The full installation includes creating a Python VENV virtual environment, downloading and starting the MongoDB image and container, installing all required Python packages, and starting the frontend and backend."Just starting" involves starting the existing MongoDB Docker container and the program. The important thing to note here is that for a complete installation, a system path will be checked for equality. This check is only successful if no virtual environment is enabled. So, to do a completely fresh installation, all existing, currently activated virtual environments must be deactivated. If you just want to start the programs (if an installation has already been executed), you must manually activate the dedicated VENV that is created by the installation before starting the script.

## Data Storage

Input

To run simulations with EnergyPlus, some data is needed. Specifically, it is an .idf file, a .csv file consisting of the Occupancy and some required metadata such as start date, end date, infiltration rate, and room dimensions. All this data is persistently stored in the MongoDB Collection simulation-input. These entries get a filename and a timestamp, which is also stored.


Output

The program executes EnergyPlus simulations. The results of such simulations are different output files of EnergyPlus. These are stored in the project folder in the eppy-output directory, and most of them will be overwritten with each simulation run. The .idf files that are used for the simulation are not surpassed by this. These are tagged and stored permanently in the folder. The most important file, the .eso file will be tagged and stored in the "eso-output" folder. These will not be overwritten, which means that with each successful simulation, an additional .eso file will be created in this folder. For the simulation used .idf and .csv files are persistently stored in the MongoDB collection "simulation-output". The time frame, as well as a filename, are also stored in addition to the above mentioned data.


## Installation with Linux

For the installation under Linux, there are a few peculiarities to consider. The .bat scripts are not executable under Linux. For this, extra shell scripts with the extension ".sh" are available. The installation happens with these scripts analogous to the bash scripts. To start the backend, the config.ini in the backend directory must be adjusted. Here is the example configuration in which EnergyPlus was installed in the /opt/ directory:

```
[EnergyPlus]
EplusPath = /opt/EnergyPlus-22.2.0
iddPath = /opt/EnergyPlus-22.2.0/Energy+.idd
```

If the installation is made on a server and should be accessible by clients, the frontend_config.ini in the frontend directory must also be adjusted:

```
[Frontend]
IP = 0.0.0.0
Port = 80
```

By setting the IP address to 0.0.0.0, the web server runs on all available network adapters. Often only a few ports are allowed by the firewall within a network. The port 100 is configured in the frontend as default. Because this is not standard and therefore is often blocked, it is recommended to change the port to the standard HTTP port 80.


# Parameterization & Configuration

The configuration of the most important software parameters for the backend can be done in the config.ini, which is located in the folder "indoor-climate-simulation/backend". The following parameters can be set:

| Parameter | Description | Section |
|----------|----------|----------|
|epluspath|Path to the EnergyPlus installation on the local machine. This path must be specified here. Example: C:/EnergyPlusV22-2-0| EnergyPlus
|iddpath|Path to the .idd file of EnergyPlus. This path must be specified to start simulations. The path must lead to the folder of the EnergyPlus installation. Idd-files which are located in other directories will lead to error messages. Example: C:/EnergyPlusV22-2-0/Energy+.idd|EnergyPlus|
|idfzone|The zone to use within an .idf file. This value defaults to "RL_Office_27214585". This value is based on the default .idf Gebäude3.idf. If a completely custom .idf is used and not only room and window dimensions are adjusted, this name must be changed accordingly by the admin. Example: RL_OFFICE-27214585|EnergyPlus|
|co2outdoorvalue|Result of the WarmUp simulation. Parameter specifies the output Co2 values that should already apply at the start of a simulation. Specification of the value in ppm. Set to 437 by default. Is needed to create plots of the Co2 values. Example: 437|EnergyPlus|
|co2generationrate|Underlying default Co2 generation rate. Set to 0.0000000382 by default. Example: 0.0000000382|EnergyPlus|
|activitylevel|The activity level of possible existing people in a simulation. Based on the American Society of Heating, Refrigerating and Air-Conditioning (ANSHRAE) of the American National Standard Institute (ANSI). Activity levels describe in activities of a person and the associated generation rates of Co2 in a space. Table shown in ANSI/ASHRAE 55-2010. Default set to 108 (mix of 55% reading, 40% typing, 3% filing sitting and 2% walking around). Example: 108|EnergyPlus|
|outputdirectoryname|Name of the output directory to be created and used for output EnergyPlus files. Set to "eppy_output" by default. Example: eppy_output|EnergyPlus|
|connection_string|Path to connect to the room climate MongoDB. Set by default to port of default installation with Docker. The locally available container port must be specified. The upstream "mongodb://" is always required and should be changed after not. Example: mongodb://localhost:27017|MongoDB|


The configuration of the parameters for the frontend will be made in the frontend_config.ini. This file is located in the folder "indoor-climate-simulation/frontend".


|Parameter|Description|Section|
|-|-|-|
|ip|The IP address where the frontend can be reached. |frontend|
|port|The port where the frontend can be reached. |frontend|
|adress|The address where the frontend can communicate with the backend. |backend|
|port|The port where the frontend can communicate with the backend. |backend|
|zone|This value is by default "RL_Office_27214585". This value is based on the standard idf Gebäude3.idf. If a completely own .idf is used and not only room and window dimensions are adjusted, this name must be changed accordingly by the admin. |simulation|


For more information regarding configurations (e.g. config.ini), please refer to .

