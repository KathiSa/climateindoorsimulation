---
layout: default
title: File Input
parent: Program
nav_order: 3
---

Author: Sophia Weißenberger
# File Input

TODO: pic 

After starting a new simulation you will be redirected to the "File Input" page. On this page you load or create all necessary IDF and EPW files for the simulation. Here is a short overview of the two file types:

## IDF files

IDF files are the core of every EnergyPlus simulation. Based on the information within this file, the following simulation is primarily based. An IDF file is mandatory for a simulation! The file contains information about the global location as well as the room geometry, specific information about the building as well as the presence of people in the simulated room (in the following called "Occupancy").

In the GUI you have two options to provide a suitable IDF file for the simulation:

1. Generate an IDF file via the GUI (Fig. 1, marker 2).
Note: The IDF file, which is generated via the web page, is based on a generic IDF file (Gebauede3), which can be further modified via the web interface.
2. Uploading a custom IDF file via the GUI (Fig. 1, marker 3).

### Step-by-step instructions for choosing the required idf file

TODO: Pic

   * (Option A) Creation of a new IDF file via web interface 
      * Click on the "+" in the line 'Generate a new IDF file'.
      * Then enter a file name (Fig. 4 marker 1). IMPORTANT: The name must not be longer than 200 characters and must not contain periods (".")!
      * Press the 'Generate base file' button (Fig. 4 marker 2).
      * After successful creation of the file, the name you have selected for the file will now be displayed in the web interface
   * (Option B) Upload an already existing IDF file
      * Click on the "+" in the line 'Upload new IDF file'.
      * Click on the 'Select file' button (Fig. 4 marker 3).
      * In the Explorer window that opens, double-click on your your desired file. IMPORTANT: Your file name must not contain dots (".")!
      * The name of your file should be displayed in the field next to the button (Fig. 4 marker 3).
      * Then click on the 'Upload custom file' button (fig. 4 marker 4) Important: Your file has been uploaded only after clicking this button!
      * After successful upload, the name of the uploaded file is now displayed in the the web interface
   * Deleting an existing IDF file
      * Click on the button 'Delete (name of file).idf'. This way you can remove an uploaded file in case you want to you want to customize. Before proceeding to the third step, make sure that you have performed either
step 1 or step 2 successfully.


## EPW files

## Step-by-step instructions for choosing the required epw file


## Optional Feature: run simulation via idf & epw 
