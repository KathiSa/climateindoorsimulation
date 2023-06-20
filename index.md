---
layout: default
title: Home
nav_order: 1
description: "Get to know the tool EnergyPlus Room Simulation."
permalink: /
---

(Author: Sophia Weißenberger) 
# EnergyPlus Room Simulation

This program is intended to simplify the operation of EnergyPlus for the user. For this purpose, an easy-to-use and straightforward GUI is provided. With this GUI it is possible to simulate the indoor climate of a room with individual IDF and EPW files. Furthermore, adjustments can be made to occupancy (presence of people and window openings), room dimensions and infiltration rate. This allows the simulation to be further individualized. Plots of the simulation results can then be displayed, and the simulation results can be downloaded as a csv file and an eso file. All inputs and outputs are persistently stored in a nosql database (MongoDB).


This documentation provides you with information for the programm Indoor Climate Simulation. This programm simulates the indoor climate in rooms. For the simulation the program “EnergyPlus” is used. Additionaly the Python Package “eppy” serves as a connector between EnergyPlus and Python.

This programm consists of two parts, the frontend and the backend. The Frontend includes a web server with a GUI to control the simulation. The backend includes the simualtion via “eppy”. Data of the simulations will be saved in a MongoDB and the backend provides additional functionalities with a REST API.

TODO: BILD EINFÜGEN ENGLISCH ARCHITEKTUR!!!
