# Semantic HVAC Tool Documentation (Will be ready by Tuesday, 12th of December)
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
 
## Userflow of the Semantic Web Tool on a real-world building model 
The initial step in the user flow involves parsing Building Information Modeling (BIM) data into a triplestore using Apache Jena Fuseki, facilitated by FSO, FPO, and BOT ontologies. This process starts by downloading the BIM model from the following GitHub link: [BIM Model](https://github.com/Semantic-HVAC-Tool/Other/blob/main/BIM-Model.rvt), and opening it in Revit. In Revit's UI, there's a Ribbon tab labeled "RDF" containing a tool named "BOT." By clicking on this tool, the parser is executed, transferring FSO, FPO, and BOT instances into the database. The accompanying image illustrates the full BIM model, including the building and HVAC components, made partially transparent for clearer visibility of the HVAC elements. This depiction highlights the complexity of an HVAC model within real-world BIM models.
![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture1.png)

To verify that all data has been correctly transferred to the triplestore, navigate to [localhost:3030](http://localhost:3030/dataset.html?tab=query&ds=/ny-db). Here, you can check that 369,044 triples have been successfully transferred. 
![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture2.PNG)

Once all triples are transferred, users can perform the first validation check at http://localhost:3000/validationOverviewTable. The UI is intentionally minimalistic, focusing on practicality over aesthetics. The UI enables users to efficiently carry out critical functions like validation checks, overviewing violations, conducting hydraulic calculations, and designing the capacity of flow-moving devices utlizing Semantic Web technologies.
![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture3.PNG)

Clicking "Validate" in the Semantic HVAC Tool initiates the first validation check, producing an overview table that categorizes violations into HVAC component types. This table not only provides a total count of 372 violations but also details how many violations each specific HVAC component type has committed. This structured categorization is important to pinpoint areas within the BIM model that need attention.

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture4.PNG)

Now, we have the option to click on the "Pipe" row in the first table . This action redirects to a new page, which specifically identifies which instances have violatioted and the reasons behind these violations. This detailed breakdown facilitates a deeper understanding of the violations associated with the fso:Pipe components, allowing for targeted and effective resolutions.

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture5.PNG)

Users can return to the BIM model in Revit and locate instances causing violations using their UniqueID numbers. Upon examination, it's found that the heating pipes leading to and from the building, depicted in the image below, are open-ended. Since open ends in a water-based heating system are not permissible in hydraulic calculations, this results in a violation. To rectify this error, caps can be added to the pipe ends to seal them. Correcting this, and rerunning the validation model, reduces the number of violations from 2 to 0.

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture0.png)

We can also click on the "SpaceHeater" row in the overview table to access a detailed description of which fso:SpaceHeater instances are violating and the reasons for these violations. The detailed table provides an in-depth view of each specific violation, enabling a thorough understanding of violations related to spaceheater components in the BIM model.
![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture12.PNG)

Reading the detailed violation table reveals that certain fso:SpaceHeater instances lack the "transfers heat to" property, meaning they do not service a room. To rectify this, we can manually locate and correct these specific space heaters within the BIM model. Utilizing Revit Lookup, users can find these components in Revit's database by entering their IDs.
![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/16.JPG)

Every Revit object has both a UniqueID, which we use, and an ID, serving different purposes. The ID is essential for locating a component's position in the 3D model. This identification is achieved through Revit Lookup. Once the ID is found, it is copied and pasted into the "Select By ID" tool under the "Manage" Ribbon. This process is crucial for precise localization and modification of specific components within the BIM model, aligning with the tool's strategy for manual correction.

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/17.JPG)

By using Revit's "Show" feature to visualize the selected fso:SpaceHeater instances within a 3D viewer, we can confirm their exact placement in relation to the room boundaries. If found outside, adjusting the boundaries to include the heaters ensures they fulfill their functional requirement in the HVAC system. This method, when applied to all non-compliant fso:SpaceHeater instances and followed by a re-validation in the Semantic HVAC Tool, effectively resolves the identified violations. For example for the fso:SpaceHeater instance, that has the ID observed that the heater is positioned outside the room's boundary. This misplacement is critical as it prevents the fso:SpaceHeater instance from serving any room, which is essential in hydraulic calculations. Adjusting the room's boundary to include the fso:Spaceheater instance, ensures compliance. Applying this method for all three fso:SpaceHeater instances leades to the resolution of violations upon re-validation in the Semantic HVAC Tool. 

Ved at klikke på show, får vi nedenstående billede. Den blå firkant viser rummets størrelse og placering. Den gule markering som jeg har lavet, viser den specifikke SpaceHeater som violater. Det er tydeligt at SpaceHeateren er placeret lige akkurat udenfor rummet. Det betyder at radiatoren ikke servicerer nogen rum overhovedet. I en hydraulisk beregning er det vigtigt at enhver instance af type fso:SpaceHeater er tilknyttet et rum. Hvis det ikke er det, er det ikke muligt at bestemme pumpens kapacitet, som den er tilknyttet. 
For at rette op dette, skal vi manuelt rykke rummets ydre grænse, helt frem til vinduet således at spaceheateren er placeret indenfor rummets geometri, som vist herunder.
Gøre vi dette for alle 3 spaceheater instancer som violater, og køre valideringstjekken påny vil antallet af violations for spaceheater gå fra 3 til 0.

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture14.JPG)

For at i udviklere/læsere kan teste applikationen uden at skulle åbne Revit og korrigerer alle 372 violations manuelt, har bygget en funktion i front enden som hedder Solve All. Ved at klikke på denne eksevereres en række SPARQL queries, der korrigerer alle 372 violations. Disse SPARLQ queries kan findes under https://github.com/Semantic-HVAC-Tool/Orchestrator-Service/tree/main/public/ManuallySolvingQueries. Foreksempel bruger vi SPARQL querien "insertMissingConnectionBetweenSpaceheaterAndSpace.ttl" til at linke alle 3 spaceheaters til deres repektive rum. Denne query er hardcoded og kan derfor ikke bruges til at bygningsmodeller til at korrigerer instancer af typen fso:SpaceHeater som ikke servicerer et rum, men kun for dette projekt så i undgår at skulle korrigerer i BIM modellen manuelt. På den anden side, er der visse SPARQL queries der ikke er hardcoded og kan bruges på alle projekter for at korrigerer specikke violations. For eksempel SPARQL querien "deleteSystemsWithoutComponents.ttl", kan bruges på alle projekter. Denne query fjerner instancer af typen fso:System som ikke indeholder nogle HVAC komponenter. 

Hvis vi går tilbage til oversigtsiden og klikker på Solve all violations" under den første tabel, vil vi ekskverer alle SPARQL queries. klikker vi dernæst på knappen validate igen, vi se en tabel med 0 violations som vist herunder. 

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture7.PNG)

Nu hvor vi har gennemført første valideringstjek med 0 violations, kan vi gå til næste step. I næste step vil vi udfører en hydraulisk beregning og dernæst lave en anden valideringstjek. Denne valideringstjek vil fortælle os om vi har nogle instancer af typen fso:Pipe der har et for højt pressre drop - som overstiger 100 Pa/m. 
Ved at klikke på hydraulisk Calc knappen får vi følgende resultat, som viser at vi har 14 instancer af typen fso:Pipe som violater. 

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture8.PNG)

Ved at klikke på rækken Pipe, redirecter vi til en side der beskriver disse violations i detaljer. Vi kan aflæse hvilke instancer der overskrider en pressure drop på 100 Pa/m.

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture9.PNG)

Hvis vi går tilbage til oversigtssiden har vi mulighed for at klikke på Solve all violations under den anden tabel. Klikker vi på denne og dernæst klikker på Hydraulic Calc igen får vi 0 violations i den anden tabel. Det skyldes at ved at klikke på Solve all violations eksvereres en SPARQL querien "autossize.ttl". Denne query er generisk og kan bruges på alle projekter. Querien finder alle instancer af typen fso:Pipe, som overskrider 100 Pa/m i pressuredrop, opdaterer deres størrelse og sletter deres pressure drop. Hvis vi så klikker på Hydraulic Calc påny, vil vi beregne pressure drop for disse komponenter påny med de nye størrelser. Idet, de nye størrerlser ikke overskriver 100 Pa/m, får vi 0 violations. 

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture10.PNG)

Nu hvor anden tabel også giver 0 violations betyder det at vi er har alle de nødvendige data til at kunne designe kapaciteten for vores flow moving devices. Ved at klikke på design, ekseveres 3 queries "PumpPressureDrop.ttl", "FanPressureDrop.ttl" og "FlowMovingDeviceFlowRate.ttl". Disse kan findes under https://github.com/Semantic-HVAC-Tool/Orchestrator-Service/tree/main/public/Queries. Idet beregningen af den samlede pressure drop bereges forskelligt for vandbaserede systemer og luftbaserede systemer, har vi en query for hver - "PumpPressureDrop.ttl" og "FanPressureDrop.ttl". De første 2 queries bestemmer den samlede tryktab, hvorimod den sidste bestemmer den samlede flowrate for hvert flow moving device. 
Efter 1.5 min, fås følgende tabel der viser kapaciteten for hver flow moving device.

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture11.PNG)

 
## Userflow of the Semantic Web Tool on a conceptual building model
Click to download and watch the following video

[![Watch the video](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Conceptual_Building_Model_video/conceptualmodel.PNG)](https://github.com/Semantic-HVAC-Tool/.readme/raw/main/Conceptual_Building_Model_video/validatorvideo1.mp4)






