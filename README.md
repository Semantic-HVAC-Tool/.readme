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

- **Capacity Service**: 
  - Handles hydraulic calculations and pressure drop analyses for various HVAC components.

- **Rule Service**: 
  - Ensures compliance with HVAC design rules and standards, providing validation reports and conformance checks.

- **Orchestrator Service**: 
  - Coordinates the workflow between different services, ensuring smooth data flow and task execution.

- **Apache Jena Fuseki Database**: 
  - Serves as the central repository for storing and managing HVAC system data, supporting complex queries for data retrieval and manipulation.


## Setup and Installation
### Capacity Service
- **Prerequisites**: Python 3.8
- **Installation**:
  1. Clone the repository: `git clone https://github.com/Semantic-HVAC-Tool/Capacity-Service.git`
  2. Navigate to the directory: `cd Capacity-Service`
  3. Create and activate the virtual environment:
     - Windows: `python38 -m venv venv` and `venv\Scripts\activate`
     - Linux/Mac: `python3.8 -m venv venv` and `source venv/bin/activate`
  4. Install requirements: `pip install -r requirements.txt`
  5. Start the service: `uvicorn server:app --reload`

### Rule Service
- **Prerequisites**: Python 3.8
- **Installation**:
  1. Clone the repository: `git clone https://github.com/Semantic-HVAC-Tool/Rule-Service.git`
  2. Navigate to the directory `cd Rule-Service`
  3. Create and activate the virtual environment:
     - Windows: `python38 -m venv venv` and `venv\Scripts\activate`
     - Linux/Mac: `python3.8 -m venv venv` and `source venv/bin/activate`
  4. Start the service: `uvicorn server:app --port 8080 --reload`

### Orchestrator Service
- **Prerequisites**: Node.js, Visual Studio Build Tools, Nodemon
- **Installation**:
  1. Clone the repository: `git clone https://github.com/Semantic-HVAC-Tool/Orchestrator-Service.git`
  2. Set up environment: `export PATH="/c/users/username/appData/local/programs/python/Python38:$PATH"`
  3. Install dependencies: `npm install`
  4. Start the service: `npm run dev`

### Apache Jena Fuseki Database
- **Prerequisites**: Java 8, Apache Jena Fuseki (version 4.2.0)
- **Installation**:
  1. Download and configure Apache Jena Fuseki.
  2. Obtain the configuration file `ny-db.ttl` from [this GitHub repository](https://github.com/Semantic-HVAC-Tool/Other/blob/main/ny-db.ttl).
  3. Add the `ny-db.ttl` file to the configuration directory: `yourPath\apache-jena-fuseki-4.2.0\run\configuration`.
  4. Update the `tdb2:location` path in the `ny-db.ttl` file with your specific path. 
  5. Run the `fuseki-server.bat` file to start the server.
  6. Open your web browser and navigate to [localhost:3030](http://localhost:3030/#/) to access the database interface.


### Data Conversion and Database Integration

#### Option 1: Revit to Fuseki
- **Prerequisites**: Revit 2021, Visual Studio 2019
- **Requirements**: Orchestrator and Database services must be running.
- **Steps**:
  1. Set up the development environment in Visual Studio 2019.
  2. Create a Revit plugin using the [My First Revit Plug-in tutorial](https://www.autodesk.com/support/technical/article/caas/tsarticles/ts/7I2bC1zUr4VjJ3U31uM66K.html).
  3. Open the BIM model (available [here](https://github.com/Semantic-HVAC-Tool/Other/blob/main/BIM-Model.rvt)).
  4. Run the plugin to convert BIM data using the parser, with Orchestrator and Database services active.

#### Option 2: Use pre-converted RDF BIM data
- **Requirements**: Database running on port 3030 (named ny-db) and the Data-model.ttl in the same folder.
- **Steps**:
  1. Clone the repository containing the `postDataToTriplestore.py` script ([link](https://github.com/Semantic-HVAC-Tool/Other/blob/main/postDataToTriplestore.py)).
  2.  Create and activate the virtual environment:
     - Windows: `python38 -m venv venv` and `venv\Scripts\activate`
     - Linux/Mac: `python3.8 -m venv venv` and `source venv/bin/activate`
  3. Install requirements: `pip install requests`
  4. Execute the script to upload data to the database, ensuring the database is correctly configured.


### Frontend Setup
- **Prerequisites**: Node.js
- **Installation**:
  1. Clone the repository: `git clone https://github.com/Semantic-HVAC-Tool/Client.git`
  2. Navigate and install dependencies: `cd client` and `npm install
  3. Start the application: `npm start`
  4. Troubleshooting: If you encounter the error 0308010C:digital envelope, resolve it by setting an environment variable with git bash: `export NODE_OPTIONS=--openssl-legacy-provider`, then run `npm start` again.
  5. Access the UI at `localhost:3000/validationOverviewTable`.
 

## Userflow of the Semantic Web Tool on test building model


## Userflow of the Semantic Web Tool on a real-world building model 
Når alle servicene kører, så er det første vi gør at parse BIM data til en triplestore (Apache Jena Fuseki) vha. FSO, FPO og BOT. 
Det gør vi ved at downloade BIM modellen fra https://github.com/Semantic-HVAC-Tool/Other/blob/main/BIM-Model.rvt og åbne den i Revit. 
I toppen af UI'en er der en Ribbon. I denne er der en tab der hedder RDF og i denne tab er der en tool der hedder BOT. Ved at klikke på BOT eksvereres parseren og vi får parset FSO, FPO og BOT instancer over til databasen. 
Billedet herunder, viser hele BIM modellen inklusiv bygningen (gjort lidt transperant for at kunne se HVAC objekterne tydeligere) og alle HVAC komponenterne. Det ses tydeligt hvor kompleks en HVAC model kan se ud for en bygning. 
![Alt text]([path/to/your/image.png](https://github.com/Semantic-HVAC-Tool/.readme/blob/main/Real_World_Building_Model_pictures/picture1.png))




For at tjekke om alt dataen er overført korrekt, til triplestoren kan vi gå til localhost:3030

Once all services are running, we can open the real BIM-Model.rvt stored on https://github.com/Semantic-HVAC-Tool/Other/blob/main/BIM-Model.rvt. 
At the top of the UI there is a Ribbon. Click on RDF, then click on BOT. Doing so, 

