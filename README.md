# .readme

Flowwing this guide, you should be able to run the setup
The organisation consists of modules 1,2,3,4,5... 
The order we will install is ....

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

## Fronend
Presqusites

Fork the repo to your local computer with bash like git clone https://github.com/Semantic-HVAC-Tool/Client.git

