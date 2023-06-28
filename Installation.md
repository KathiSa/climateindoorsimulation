---
layout: default
title: Installation
nav_order: 2
---
(Author: Diana) 
# Installation

Requirements to install the software Indoor Climate Simulation:

* Installation of Python 3.10
* [Installation of EnergyPlus (Version 22-2-0)](https://energyplus.net/)
* [Current Version of Docker](https://www.docker.com/)
* Windows or Linux as operating system
* [Installation of Node.js](https://learn.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-on-windows)

## Note 
The script performs either a complete installation or just a start of the resources and the program. The full installation includes creating a Python VENV virtual environment, downloading and starting the MongoDB image and container, installing all required Python packages, and starting the frontend and backend. Just the starting involves starting the existing MongoDB Docker container and the program. The important thing to note here is that for a complete installation, a system path will be checked for equality. This check is only successful if no virtual environment is enabled. So, to do a completely fresh installation, all existing, currently activated virtual environments must be deactivated. For just starting the programs (if an installation has already been executed), whereas, you must manually activate the dedicated VENV that is created by the installation before starting the script.

## Instructions

To install the software for the first time, the "install.py" script in the indoor-climate-simultion folder must be called with the "python install.py" command. To do this, the user must be in the project folder and not call this script from parent/subordinate folders. The script then creates a VENV named "indoor-climate-simultion" using .bat files and the Windows command prompt and activates it. After that, all required packages will be installed into the virtual environment using pip. Once these steps are completed, the Docker image "mongo" is downloaded in the current version of Docker Hub. After the download is complete, a Docker container is created with this image named "simulation_db". If these steps are done, the frontend and backend will be started. At the end of the complete installation, two cmd shells should be open in which the two program parts run. Under the port localhost:100 the GUI should be accessible and usable.  To start the software after the installation has been completed, it is also necessary to change to the project folder. Afterwards it must be ensured that the VENV "indoor-climate-simulation" is activated before the script "install.py" is called again. The script will then only start the created container "simulation_db" and both frontend and backend. The program should then be usable without a complete reinstallation. After installing or restarting the program, two Python applications and a MongoDB Docker container should be running on the machine. Port 100 and 5000 should be blocked by the software itself. Port 100 runs the frontend based on the Flask framework, while port 5000 is where the REST API, also based on Flask, waits for requests. The MongoDB container also blocks a port, but this port is not fixed by the programmers.

## Memory

Input

Um Simulationen mit EnergyPlus durchzuführen, werden einige Daten benötigt. Konkret 
handelt es sich um eine .idf Datei, eine .csv Datei der Occupancy und um einige benötigte 
Metadaten wie Startdatum, Enddatum, Infiltrationsrate, Raum und Fenstermaße. All diese 
Daten werden persistent in der MongoDB Collection simulation-input gespeichert. Diese 
Einträge werden dabei noch mit einem Filename und einem Zeitstempel versehen, welcher 
ebenfalls gespeichert wird. 


Output

Das Programm führt EnergyPlus Simulationen durch. Ergebnisse solcher Simulationen sind 
einige Output-Dateien von EnergyPlus. Diese werden im Projektordner im eppy-output 
Verzeichnis abgelegt, wobei diese bei jedem Simulationslauf überschrieben werden. Die für 
die Simulation genutzten .idf Dateien sind hiervon nicht übertroffen. Diese werden mit 
einem tag versehen und dauerhaft im Ordner gespeichert. Die wichtigste Datei, die .eso Datei 
wird mit einem tag versehen und im Ordner „eso-output“ abgelegt. Diese werden nicht 
überschrieben, was bedeutet, dass mit jeder erfolgreichen Simulation eine weitere .eso Datei 
in diesem Ordner erstellt wird. Die zur Simulation genutzte .idf und die .csv Datei werden 
hingegeben persistent in der MongoDB Collection „simulation-output“ gespeichert. Auch 
hier wird ebenfalls ein Zeitstempel, sowie ein Filename zusätzlich zu den oben genannten 
Daten gespeichert.

## Installation with Linux

Für die Installation unter Linux gibt es ein paar Eigenheiten zu beachten. Die .bat Skripte 
sind unter Linux nicht ausführbar. Dafür werden extra Shell-Skripte mit der Endung „.sh“ 
bereitgestellt. Die Installation Erfolg mit diesen Skripten analog zu den Bash-Skripten. Zum 
Starten des Backend muss allerdings die config.ini im Backend-Verzeichnis angepasst 
werden. Im Folgenden ist die Beispiel-Konfiguration in der EnergyPlus im /opt/ Verzeichnis 
installiert wurde.

```
[EnergyPlus]
EplusPath = /opt/EnergyPlus-22.2.0
iddPath = /opt/EnergyPlus-22.2.0/Energy+.idd
```

Erfolgt die Installation auf einem Server und soll von Clients erreichbar sein, muss außerdem 
die frontend_config.ini im Frontend-Verzeichnis angepasst werden.

```
[Frontend]
IP = 0.0.0.0
Port = 80
```

Durch die eingestellt IP-Adresse 0.0.0.0 läuft der Webserver auf allen verfügbaren Netzwerk Adaptern. Oft sind innerhalb eines Netzwerks nur wenige Ports durch die Firewall 
zugelassen. Standardmäßig ist im Frontend der Port 100 konfiguriert, da dieser kein 
Standard ist und daher oft blockiert wird, empfiehlt es sich den Port auf den Standard http Port 80 zu ändern.
Für die genauer Abfolge der Befehle um die komplette Anwendung, sowie EnergyPlus unter 
Linux zu installieren bitte die README.md Datei des Projekts beachten.


# Parametrisierung & Konfiguration

Die Konfiguration der wichtigsten Softwareparameter für das Backend kann in der config.ini, 
welche sich im Ordner „raumklimadaten-simulation-a1/backend“ befindet, vorgenommen 
werden. Folgende Parameter können eingestellt werden:

| Parameter | Beschreibung | Bereich |
|----------|----------|----------|
|EplusPath|Pfad zur EnergyPlus Installation auf der lokalen Maschine. Dieser Pfad muss hier angegeben werden. Beispiel: C:/EnergyPlusV22-2-0| EnergyPlus
|iddPath|Pfad zur .idd Datei von EnergyPlus. Dieser Pfad muss angegeben werden, um Simulationen starten zu können. Der Pfad muss hierbei in den Ordner der EnergyPlus Installation führen. Idd-Files, welche in anderen Verzeichnissen liegen, werden zu Fehlermeldungen führen. Beispiel: C:/EnergyPlusV22-2-0/Energy+.idd|EnergyPlus|
|idfZone|Die zu nutzende Zone innerhalb einer .idf Datei. Dieser Wert liegt standardmäßig bei „RL_Office_27214585“. Dieser Wert basiert auf dem Standard-IDF Gebäude3.idf. Wird ein komplett eigenes .idf genutzt und nicht nur Raum und Fenstermaße angepasst, so muss dieser Name entsprechend vom Admin geändert werden. Beispiel: RL_OFFICE-27214585|EnergyPlus|
|co2OutdoorValue|Ergebnis der WarmUp Simulation. Parameter gibt die Ausgangs Co2 Werte an, welche beim Start einer Simulation bereits gelten sollen. Angabe des Wertes im ppm. Standardmäßig auf 437 eingestellt. Wird benötigt, um Plots der Co2 Werte erstellen zu können.Beispiel: 437|EnergyPlus|
|co2GenerationRate|Zugrundeliegende Standard-Co2 Generationsrate. Standardmäßig auf 0.0000000382 eingestellt. Beispiel: 0.0000000382|EnergyPlus|
|ActivityLevel|Das Aktivitätslevel möglicher vorhandener Personen in einer Simulation. Basiert auf der American Society of Heating, Refrigerating and Air-Conditioning (ANSHRAE) des American National Standard Institutes (ANSI). Aktivitätslevel beschreiben in Tätigkeiten einer Person und die damit verbundenen Generationsraten von Co2 in einem Raum. Tabelle ersichtlich in ANSI/ASHRAE 55-2010. Standardmäßig auf 108 (Mischung aus 55 % Lesen, 40% Tippen, 3% Ablage sitzend und 2% Umhergehen) eingestellt. Beispiel: 108|EnergyPlus|
|OutputDirectoryName|Name des zu erstellenden und zu nutzenden Output Directorys für Output-EnergyPlus Files. Standardmäßig auf „eppy_output“ eingestellt. Beispiel: eppy_output|EnergyPlus|
|Connection_String|Pfad, um sich mit der Raumklima MongoDB zu verbinden. Standardmäßig auf Port der Standardinstallation mit Docker eingestellt. Der lokal verfügbare Container Port muss angegeben werden. Das vorgeschaltete „mongodb://“ wird immer benötigt und sollte nach nicht abgeändert werden. Beispiel: mongodb://localhost:27017|MongoDB|


Die Konfiguration der Parameter für das Frontend werden in der frontend_config.ini 
vorgenommen. Diese Datei befindet sich im Ordner „„raumklimadaten-simulation-a1/frontend


|Parameter|Beschreibung|Bereich|
|-|-|-|
|IP|Die IP Adresse unter der das Frontend erreichbar ist. |Frontend|
|Port|Der Port unter dem das Frontend erreichbar ist. |Frontend|
|Adress|Die Adresse unter der das Frontend mit dem Backend kommunizieren kann. |Backend|
|Port|Der Port unter dem das Frontend mit dem Backend kommunizieren kann|Backend|
|Zone|Dieser Wert liegt standardmäßig bei „RL_Office_27214585“. Dieser Wert basiert auf dem Standard-IDF Gebäude3.idf. Wird ein komplett eigenes .idf genutzt und nicht nur Raum und Fenstermaße angepasst, so muss dieser Name entsprechend vom Admin geändert werden. |Simulation|




