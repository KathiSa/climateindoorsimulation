---
layout: default
title: Overview
parent: Program
nav_order: 6
---

Author: Sophia Weißenberger
# Overview

This step of the web application takes you to the overview of simulation inputs and configurations, as well as the execution of the simulation itself.

## Simulation Parameters

The simulation overview allows you to view all inputs again before the simulation and to check their correctness.

TODO: Picture


For instance, in the File Input section (Fig. Overview, marker 1), you can again view which .idf and .epw file you selected at the beginning. In the simulation parameters section of the overview (Fig. Overview, marker 3), you will again see the room dimensions and infiltration rate you specified for the simulation. If you notice that you have made an incorrect entry here, you can return to the parameter input again and adjust the values accordingly (Fig. Overview, marker 3).

TODO: Picture custom occupancy

In the section below (Fig. Overview, marker 2), you can view the room occupancy (Occupancy) created or uploaded in process step 2. This occupancy section is only shown if you have choosen to create a custom occupancy in step 2, If you have uploaded an occupancy file, the file will be shown in the File input section.

## Simulation status 

TODO: Picture

If you are satisfied with the input data, you can now start the simulation. To do this, click on the "Start Simulation" field (Fig. Overview, marker 4). Once you have done this, the simulation ID will be displayed at the top, allowing you to clearly distinguish between the different simulations (Fig. Overview, marker 1). Within the next few seconds, you will now be continuously informed about the progress of the simulation.

In order for EnergyPlus to run the simulation with the data you have provided, the first step is to transfer the data from the frontend (GUI) to the backend. This happens automatically and you will be informed in real time about the data packages transferred in sequence, such as the idf or epw file (Fig. Overview, marker 2). Once this is done, Energyplus will start the execution of the simulation and you will be shown a loading circle while the simulation is running.

After the simulation has been run successfully, you will receive a "simulation finished" success message. The "View Results" Button will take you to the next step of the web application where you can plot the results of the simulation (Fig.Overview, marker 5) and download the complete results as a csv-file or eso-file.
