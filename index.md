---
layout: default
title: Home
nav_order: 1
description: "Get to know the tool EnergyPlus Room Simulation."
permalink: /
---

(Author: Sophia Weißenberger) 
# EnergyPlus Room Simulation

This program is intended to simplify the operation of EnergyPlus for the user. For this purpose, an easy-to-use and straightforward GUI is provided. With this GUI it is possible to simulate the indoor climate of a room with individual IDF and EPW files. Furthermore, adjustments can be made to occupancy (presence of people and window openings), room dimensions and infiltration rate. This allows the simulation to be further individualized. Plots of the simulation results can then be displayed, and the simulation results can be downloaded as a CSV file and an ESO file. All inputs and outputs are persistently stored in a NoSQL database (MongoDB).

This documentation provides you with information about the tool Indoor Climate Simulation. This tool simulates the indoor climate in rooms. The software “EnergyPlus” is used for the simulation. Additionaly the Python package “eppy” serves as a connector between EnergyPlus and Python.

This program consists of two parts: the frontend and the backend. The frontend includes a web server with a GUI to control the simulation. The backend includes the simulation via the “eppy” Python package. Data of the simulations is saved in an MongoDB instance. The backend provides additional functionalities with a REST API.

TODO: BILD EINFÜGEN ENGLISCH ARCHITEKTUR!!!
