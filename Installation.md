---
layout: default
title: Installation
nav_order: 2
---

# Introduction

This documentation provides you with information for the programm Indoor Climate Simulation. This programm simulates the concentration of CO2 in rooms. For the simulation the program "EnergyPlus" is used. Additionaly the Python Package "eppy" serves as a connector between EnergyPlus and Python.  

This programm consists of two part, the frontend and the backend. The Frontend includes a web server with a GUI to control the simulation. The backend includes the simualtion via "eppy". Data of the simulations will be saved in a MongoDB and the backend provides additional functionalities with a REST API. 

![Architecutre](images/Architecture.png)


# Installation

Requirements to install the software Indoor Climate Simulation

* Installation of Python 3.10
* Installation of EnergyPlus (Version 22-2-0)
* Current Version of Docker
* Windows or Linux

## Note 
Das Skript führt entweder eine komplette Installation oder lediglich einen Start der 
Ressourcen und des Programms durch. Die komplette Installation beinhaltet das Erstellen 
eines virtuellen Python VENV Environments, das Herunterladen und Starten des MongoDB 
Images und Containers, das Installieren aller benötigten Python Packages und das Starten 
von Frontend und Backend. Das bloße Starten beinhaltet das Starten des bestehenden 
MongoDB Docker Containers und des Programms. Wichtig ist hierbei: Für eine komplette 
Installation wird auf ein Systempfad auf Gleichheit überprüft. Diese Prüfung ist nur dann 
erfolgreich, wenn kein virtuelles Environment aktiviert ist. Um eine komplett frische 
Installation durchzuführen, müssen also alle bestehenden, aktuell aktivierten virtuellen
Umgebungen deaktiviert werden. Für einen bloßen Start der Programme (falls eine 
Installation bereits durchgeführt wurde) muss hingegen vor dem Start des Skripts das 
dedizierte, von der Installation kreierte VENV manuell aktiviert werden.

## Instructions

Um die Software erstmalig zu installieren, muss das Skript „install.py“ im Ordner 
raumklimadaten-simulation-a1 mit dem Befehl „python install.py“ aufgerufen werden. 
Dabei muss sich der Anwender im Projektordner befinden und dieses Skript nicht von 
über/untergeordneten Ordnern aufrufen. Das Skript erstellt mithilfe von .bat Dateien und 
unter Nutzung der Windows Befehlsaufforderung anschließend ein VENV mit dem Namen 
„raumklimadaten-a1“ und aktiviert dieses. Anschließend werden alle benötigten Packages 
mit pip in das virtuelle Environment installiert. Sind diese Schritte abgeschlossen, wird das 
Docker-Image „mongo“ in der Version 6.0.2 von Docker-Hub heruntergeladen. Nach 
Abschluss des Downloads wird ein Docker-Container mit diesem Image mit dem Namen 
„raumklima_db“ erstellt. Sind diese Schritte abgeschlossen, wird das Frontend und Backend 
gestartet. Am Ende der kompletten Installation sollten zwei cmd-Shells geöffnet sein, in welchen die beiden Programmteile ablaufen. Unter dem Port localhost:100 sollte die 
Benutzeroberfläche erreichbar und nutzbar sein. 
Um die Software nach erfolgter Installation lediglich zu starten, muss ebenfalls in den 
Projektordner gewechselt werden. Anschließend muss sichergestellt werden, dass das VENV 
„raumklimadaten-simulation-a1“ aktiviert ist, bevor erneut das Skript „install.py“ 
aufgerufen wird. Das Skript wird dann lediglich den erstellten Container „raumklima_db“ 
und sowohl Frontend als auch Backend starten. Das Programm sollte anschließend ohne 
eine komplette Neuinstallation verwendbar sein. 
Nach der Installation oder des erneuten Starts des Programms sollten zwei Python 
Applikationen und ein MongoDB Docker Container auf der Maschine laufen. Port 100 und 
5000 sollten von der Software selbst blockiert sein. Auf Port 100 läuft das auf dem 
Framework Flask basierende Frontend, während auf Port 5000 die ebenfalls auf Flask 
basierende REST API auf Anfragen wartet. Der MongoDB Container blockiert ebenfalls einen 
Port, welcher jedoch nicht fest von den Programmierern vorgegeben ist.

## Error Management

Das Programm ist in der Lage sein, alle erwarteten und gängigen Fehler ordentlich zu 
bearbeiten. Sollten im Frontend Fehlerzustände auftreten, so wird der Nutzer über eine 
Meldung auf der GUI informiert. Sollte es im Backend zu Fehlern kommen, so wird eine 
Fehlermeldung in der Konsole ausgegeben, in welcher das Backend läuft. Fehler, welche mit 
einer konkreten und leicht verständlichen Fehlermeldung in der Konsole ausgegeben 
werden, führen nicht zum Absturz des Backends. Tritt allerdings ein unerwarteter Fehler 
auf, so würde dies zum kompletten Absturz des Backends führen. In diesem Fall müsste das 
Programm neu gestartet werden. 

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


Einfügen des Kapitels Installation unter Linux aus dem Administrationshandbuch


# Parametrisierung & Konfiguration

Einfügen des Kapitels P & K aus dem Administrationshandbuch


