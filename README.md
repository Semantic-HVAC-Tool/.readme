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
 

## Userflow of the Semantic Web Tool on test building model


## Userflow of the Semantic Web Tool on a real-world building model 
Når alle servicene kører, så er det første vi gør at parse BIM data til en triplestore (Apache Jena Fuseki) vha. FSO, FPO og BOT. 
Det gør vi ved at downloade BIM modellen fra https://github.com/Semantic-HVAC-Tool/Other/blob/main/BIM-Model.rvt og åbne den i Revit. 
I toppen af UI'en er der en Ribbon. I denne er der en tab der hedder RDF og i denne tab er der en tool der hedder BOT. Ved at klikke på BOT eksvereres parseren og vi får parset FSO, FPO og BOT instancer over til databasen. 
Billedet herunder, viser hele BIM modellen inklusiv bygningen (gjort lidt transperant for at kunne se HVAC objekterne tydeligere) og alle HVAC komponenterne. Det ses tydeligt hvor kompleks en HVAC model kan se ud for en bygning. 
![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture1.png)


For at tjekke om alt dataen er overført korrekt, til triplestoren kan vi gå til [localhost:3030](http://localhost:3030/dataset.html?tab=query&ds=/ny-db), og se at vi har fået overført 369044 triples. 
![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture2.PNG)

I og med alle triples er overført kan vi udfører første valideringstjek ved at gå til http://localhost:3000/validationOverviewTable. Det skal siges at UI'en på siden er holdt super simpelt. Der er ikke brugt meget tid på at gøre UI'en flot, derfor er den holdt super simpelt blot for at vise de mest nødvendige funktioner. Netop at vi kan udfører en valideringstjek, få et overblik over violations, køre en hydraulisk beregning, køre en anden valideringstjek og dernæst designe kapaciteten på alle flow moving devices. Af den grund, når man lander på siden, ser man følgende:

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture3.PNG)

Ved at klikke på Validate eksveres første valideringstjek og får en tabel der opdeler violations i kategorier. Sammenlagt har vi 372 violations.

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture4.PNG)

Vi har nu mulighed for at klikke på rækken Pipe. Når vi gør det føres vi til en ny side, der identificerer hvilke instances der violater og hvorfor de violater. 

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture5.PNG)

Det er så muligt at gå tilbage til BIM modellen i Revit, finde frem til instancerne der violater ud fra deres ID nummer. 
Slår vi dem op, viser det sig at der er varmerøret frem og retur til bygningen, som er markeret i billede for neden, der står åbent. Idet, vi i en hydraulisk beregning ikke må have åbne ender, i et vandbaseret varmesystem, får vi en vioalation.
For at kunne korrigerer denne fejl, kan vi tilføje en cap på enden af røret for at forsegle det. Gør vi dette, og køre valideringsmodellen igen, vil antallet af violations i oversigten gå fra 2 til 0. 

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture0.png)

Vi kan også klikke på rækken SpaceHeater i oversigtstabellen for at få en detaljeret beskrivelse af hvilke fso:SpaceHeater instancer der violater og hvorfor de violater. Den detaljerede tabel er vist herunder:

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/picture12.PNG)

Ud fra violation beskrivelsen kan vi konstaterer at de specifikke instancer af typen fso:SpaceHeater ikke har propertien transfers heat to -  med andre ord det servicere ikke et rum. 
Vi har så mulighed for at gå ind i BIM modellen og lokaliserer disse 3 spaceheaters og korrigerer dem manuelt. Vi kan Åbne BIM modellen og anvende Revit Lookup til slå komponentet op i Revits database.
Først indtaster vi ID'et af et af komponenterne ind i Revit Lookup, som vist herunder:

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/16.JPG)

Ethvert Revit object har både en UniqueID (den som vi bruger) og ID. De bruges til forskellige formål. Vi skal bruge ID til at lokaliserer komponentets placering i 3D modellen, derfor finder vi denne igennem Revit Lookup, kopierer det og indsætter der i værktøjet Select By ID, som ligger under Ribbon Manage.
Dette er vist herunder. 

![Alt text](https://raw.githubusercontent.com/Semantic-HVAC-Tool/.readme/main/Real_World_Building_Model_pictures/17.JPG)

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

 




