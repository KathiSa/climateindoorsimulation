---
layout: default
title: Start Program
parent: Program
nav_order: 1
---
(Author: Sophia Weißenberger) 
# Start Program with Windows

To start the program you have two different options: 
   * start everything at the same time (both frontend and backend)
   * start frontend and backend seperately

You can find a step by step guide including pictures below.

## Simple guide for starting everything at the same time

Open a command window and navigate into your project folder. 

Execute the following: 

```
 python install.py
```

Your backend and frontend has been started. Now you can open [http://localhost:100/](http://localhost:100/) in your browser and use the frontend to run a simulation.

Note: You can find a step by step guide including pictures below.

## Simple guide for starting frontend and backend seperately

### Starting backend: 

Open a command window and navigate into your project folder. 

Execute the following:  

```
cd backend
```

Execute the following:  

```
python -m app
```

Your backend has been started. Now you can use the backend, for example to work with Jupyter Notebooks.

Note: You can find a step by step guide including pictures below.

### Starting frontend (optional): 

Open another command window and navigate into your project folder. 

Execute the following: 

```
cd frontend
```

Execute the following:  

```
python -m app
```


Your frontend has been started. Now you can open [http://localhost:100/](http://localhost:100/) in your browser and use the frontend to run a simulation.

Note: You can find a step by step guide including pictures below.


## Start everything at the same time (with pictures)

Step 1: Open a new command window.

![Figg. 1](images/ProgramStartAll1.PNG)

Step 2: Navigate to the project folder.

(Note: File path in the picture will differ and is only for visualisation purposes.)

![Figg. 2](images/ProgramStartAll2.PNG)

Step 3: Run script install.py 

Execute the following: 

```
 python install.py
```

![Figg. 4](images/ProgramStartAll4.PNG)

Step 4: Backend and frontend start automatically

In this step you have to do nothing. If the frontend started successfully, another command window should open automatically. 

![Figg. 5](images/ProgramStartAll5.PNG)


Step 5: Open [http://localhost:100/](http://localhost:100/) in your browser

You should see the following: 

![Figg. 6](images/ProgramStartAll6.PNG)

## Start frontend and backend seperately (with pictures) 

Step 1: Open a new command window.

![Figg. 1](images/ProgramStartAll1.PNG)

Step 2: Navigate to the project folder.

(Note: File path in the picture will differ and is only for visualisation purposes.)

![Figg. 2](images/ProgramStartAll2.PNG)

Step 3: Start backend 

Execute the following: 

```
cd backend
```

![Figg. 1](images/ProgramStartFB4.PNG)

Execute the following: 

```
python -m app
```

![Figg. 1](images/ProgramStartFB5.PNG)

Your backend has been started. Now you can use the backend, for example to work with Jupyter Notebooks. 

Step 4 (optional): Start frontend 

Repeat Step 1 and 2: Open a new command window and navigate to the project folder. 

Execute the following: 

```
cd frontend
```

![Figg. 1](images/ProgramStartFB6.PNG)

Execute the following: 

```
python -m app
```

![Figg. 1](images/ProgramStartFB7.PNG)

Your frontend has been started. Now you can open [http://localhost:100/](http://localhost:100/) in your browser and use the frontend to run a simulation. 

# Start Program with Linux

TODO: TODOOOOOOOO 
