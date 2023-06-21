---
layout: default
title: File Input
parent: Program
nav_order: 3
---

Author: Sophia Weißenberger
# File Input

![Figg. 1](images/FileUpload1Marked.PNG)

After starting a new simulation you will be redirected to the "File Input" page. Here you can see the simulation process-steps for the first time (Fig. 1, marker 1), right now you are in the step "File input". On this page you upload all necessary IDF and EPW files for the simulation. Here is a short overview of the two file types:

## IDF files

IDF files are the core of every EnergyPlus simulation. Based on the information within this file, the following simulation is primarily based. An IDF file is mandatory for a simulation! The file contains information about the global location as well as the room geometry, specific information about the building as well as the presence of people in the simulated room (in the following called "Occupancy").

In the GUI you have two options to provide a suitable IDF file for the simulation:

1. Use an example IDF file (base.idf) via the GUI (Fig. 1, marker 2).
Note: The IDF file, is based on a generic IDF file (Gebauede3), which can be further modified via the web interface.
2. Uploading a custom IDF file via the GUI (Fig. 1, marker 3).

### Step-by-step instructions for choosing the required idf file

![Figg. 2](images/FileUpload3Marked.PNG)

   * (Option A) Using a given example IDF file  
      * Click on the "+" button in the line 'Use example IDF file' (Fig. 1, marker 2).
      * Press the 'Use example' button (Fig. 2, marker 1).
      * If successful, the name "base.idf" will now be displayed in the web interface (Fig. 4, marker 1).
   * (Option B) Upload your own IDF file
      * Click on the "+" in the line 'Upload IDF file' (Fig. 1, marker 3).
      * Click on the 'Select file' or 'Datei auswählen' button (Fig. 2, marker 2).
      * In the Explorer window that opens, choose your desired file.
      * If successfull the name of your file should be displayed in the field next to the button (Fig. 2 marker 3).
      * Then click on the 'Upload' button (Fig. 2, marker 4). Important: Your file has been uploaded only after clicking this button!
      * After a successful upload, the name of the uploaded file is now displayed in the the web interface (Fig. 4, marker 1).
   * Deleting an existing IDF file
      * If you want to remove an uploaded file, you don't have to delete it. You can choose a new file and the old upload will be overwritten.
      


## EPW files

## Step-by-step instructions for choosing the required epw file

![Figg. 3](images/FileUpload5Marked.PNG)

## Optional Feature: Run simulation only via idf & epw file

![Figg. 4](images/FileUpload6Marker.PNG)


![Figg. 5](images/FileUpload7.PNG)

