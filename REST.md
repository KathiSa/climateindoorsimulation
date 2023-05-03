---
layout: default
title: REST
nav_order: 4
---

# Endpoints

## Overview

|URI|Methode|Beschreibung|
|-|-|-|
|http://localhost:5000/simulation|POST|Erzeugt eine neue Simulations-ID und initiiert damit eine neue Simulation im Backend|
|http://localhost:5000/simulation|GET|Gibt alle Informationen zu einer Simulation zurück|
|http://localhost:5000/simulation|DEL|Löscht eine Simulation|
|http://localhost:5000/simulation/overview|GET|Übersicht über alle initiierten Simulationen|
|http://localhost:5000/simulation/control|POST|Startet Simulation|
|http://localhost:5000/simulation/control|GET|Check den Status der Simulation|
|http://localhost:5000/idf|POST|Hochladen oder Bearbeiten einer idf-Datei|
|http://localhost:5000/weather|POST|Hochladen oder Bearbeiten von Wetterdaten im epw-Format|
|http://localhost:5000/occupancy|POST|Hochladen oder Bearbeiten von Belegungsdaten im csv-Format|
|http://localhost:5000/result|GET|Ergebnis einer Simulation abrufen|
|http://localhost:5000/result/overview|GET|Übersicht aller verfügbaren Ergebnisse abrufen|
|http://localhost:5000/metadata|GET|Metadaten zu einer Simulation abrufen|

TODO: REST erklären
