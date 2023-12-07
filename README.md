# Semantic HVAC Tool Documentation (Will be ready by Sunday, 10th of December)
The Semantic HVAC Tool is a microservice-oriented web application, designed at the Technical University of Denmark's Department of Mechanical Engineering to perform compliance checking using Semantic Web technologies. It features a four-layer architecture consisting of 5 services. 

## Core Components and Their Functions

### Presentation Layer
This layer employs a Graphical User Interface (GUI), developed using React, to offer an intuitive experience. Users can initiate tasks like conformance checking, hydraulic calculations, and view detailed results of these operations.

### Communication Layer
Acting as the nervous system of the application, this layer facilitates seamless communication between various services. It uses an orchestrator, developed with ExpressJS and NodeJS, to manage interactions between the presentation, business, and database layers.

### Business Layer
This is where the application's intelligence resides. Divided into multiple microservices like the Capacity Service and Rule Service, it handles the core logic for HVAC design and rule compliance. These services are built using FastAPI and are responsible for essential tasks such as hydraulic calculations and conformance checks.

### Database Layer
Underpinning the application, this layer uses a Jena Fuseki server to manage RDF data efficiently. It provides a robust foundation for storing and retrieving HVAC data, ensuring data integrity and facilitating advanced queries.

### Services Overview

#### Capacity Service
Handles hydraulic calculations and pressure drop analyses for various HVAC components.

#### Rule Service
Ensures compliance with HVAC design rules and standards, providing validation reports and conformance checks.

#### Orchestrator Service
Coordinates the workflow between different services, ensuring smooth data flow and task execution.

#### Apache Jena Fuseki Database
Central repository for storing and managing HVAC system data, supporting complex queries for data retrieval and manipulation.


## Setup and Installation

## Settting up the capacity service
Prescrusites
- python 3.8
- 
Python environment variables, python engine 3.11

Fork the repo to your local computer with bash like git clone https://github.com/Semantic-HVAC-Tool/Capacity-Service.git
cd Capacity service
python38 -m venv venv or py -3.8 -m venv venv
source venv/script/activate
pip install -r requirements.txt
uvicorn server:app --reload

Now it is up and running 


## Setting up the rule service
Prescrusites
- python 3.8, Python Version: It appears that node-gyp is now correctly finding Python 3.8, which is good.


Fork the repo to your local computer with bash like git clone https://github.com/Semantic-HVAC-Tool/Rule-Service.git
cd Capacity service
source venv/script/activate
uvicorn server:app --port 8080 --reload

Now it is up an running

## Orchestrator
Prescrusites
Install Visual Studio Build Tools
Use python 3.8 as engine
Node
Fork the repo to your local computer with bash like git clone https://github.com/Semantic-HVAC-Tool/Orchestrator-Service.git
make sure export PATH="/c/users/alkc/appData/local/programs/python/Python38:$PATH"

npm install
npm start


## Apache Jena Fuseki Database
Prescrusites
- download apache jena fuseki from https://jena.apache.org/download/
- install java 8
- add the configuration file ny-db.ttl which is stored in https://github.com/Semantic-HVAC-Tool/Other/blob/main/ny-db.ttl to the configuration folder in you database folder yourPath\apache-jena-fuseki-yourVersion\run\configuration
- Update the tdb2:location path with your own path and version in the ny-db.ttl
- run fuseki-server.bat
Open the browser and go to [localhos:](http://localhost:3030/#/) and you will see it running

## Convert and add data to the database
Option 1. Revit to Fuseki with the parser
prereqsuits 
- Revit 2021
- Visual Studio 2019
- Revit 2021 SDK
- The orchestrator server shall be running on port 3500
- The fuseki server shall be running
* Know how to setup a revit plugin manually. If not follow this tutorial https://www.autodesk.com/support/technical/article/caas/tsarticles/ts/7I2bC1zUr4VjJ3U31uM66K.html, then you might know how to do it.
Clone the repository to you local folder with git clone https://github.com/Semantic-HVAC-Tool/Parser.git
Download the BIM model https://github.com/Semantic-HVAC-Tool/Other/blob/main/BIM-Model.rvt and open it in Revit 2021
Go to RDF convert and click on as shown in picture 1
The data should be stored in the database

Option 2. Use the already RDF converted BIM data
- The fuseki server shall be running
clone the repo XXX BigTurtleReader
cd bigTurtleReader
source venv/script/activate
pip install requests
python postDataToTriplestore.py

When you go to localhost3030 you will that the BIM data and related ontologies are stored in the database

## Frontend
Presqusites
Fork the repo to your local computer with bash like git clone https://github.com/Semantic-HVAC-Tool/Client.git
cd client folder
run npm install in git bash 
run npm in git bash
run npm start
- if you get error:0308010C:digital envelope try export NODE_OPTIONS=--openssl-legacy-provider and then run npm start, then it should work.
go to http://localhost:3000/validationOverviewTable and you will see the following UI:
