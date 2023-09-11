**Final-FCA-Project**

**Project 1**

The aim of this project was to build a simple Flask application that could backup files and store logs of its actions in a database. The application was then containerised, stored in a private Nexus repository, and automated using Jenkins.

Project 1 involved the following technical tasks:
1. Install Docker
2. Install MySQL as a Docker image
3. Create a SQL schema

The backup feature involved taking a command from a Flask endpoint and executing the necessary logic in Python. These actions are all recorded in the database.

**Database**
The database can be created using the CLI in MySQL, including the following commands:
CREATE DATABASE fca;

CREATE TABLE log(
  id        INT PRIMARY KEY AUTO_INCREMENT,
  date      TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  action    VARCHAR(100),
  parameter VARCHAR(1000),
  status    VARCHAR(100)
);

**Running the code**
```
python app.py 
```

**Using Curl**

Backup example: 
```
curl -X POST http://localhost:5000 -H "Content-Type: application/json" -d '{"path":"G:\hello"}' 
```
Get Log:
```
curl localhost:5000/log?start=2023-06-01&end=2023-08-31
```
Get Log: 
```
curl localhost:5000/stat
```
**Flask**
The app itself consists of three files. The first - app.py - runs the main Flask application, and handles the routes. The second - backup.py - handles the actual backing up of the folders, and the third - data.py - connects to the MySQL database and handles the logging. 

**Docker** 
Part 2 of the project involved taking the completed application, containerising it and automating it. Containerisation was achieved by creating a Dockerfile that installs all the required dependencies (using pip) and then ran the app.py file on port 5000. 

A standard MySQL container was created using the official image on Docker Hub, which was run on port 3306. Once said container was running, a bash terminal was created on the container, allowing access to the MySQL server. 

Two containers are used in this project: the first, as mentioned, runs the app.py file, and the second stores the logs in the MySQL database.

**Nexus** 

Nexus was used to host the created images in a private repo, and to provide a proxy to the Docker Hub. A containerised version of Nexus was used, and within it, a Docker proxy was created. This was to allow the user to pull images from Docker that were not stored on the local machine via their Nexus repo manager. 

A secured private Docker repo was also created within Nexus to allow the user to store created images on a repo managed by them or a trusted organistion. The purpose here is to ensure that it is not necessary to rely on a third party or public service such as Docker Hub to store images. 

**Jenkins**

Jenkins was used to automate the running of the project - this included:
i. Building the image
ii. Pushing the image to Nexus
iii. Running the image
iv. Connecting the image to a Docker network

The above involved creating a Docker pipeline within Jenkins (with appropriate Docker plugins installed). The pipeline ran based on a Jenkinsfile stored in an external repo (the QA Gitlab account, although the Jenkinsfile is available now in this repo). 

The Jenkins pipeline had three stages: in the build stage, the Docker image for the app is created and tagged 'latest'. In the push stage, the pipeline logs into the private Nexus repo and uploads the tagged Docker image. In the deploy stage, the image is used to run a containerised version of the app, using port 8085 as the host (this port avoids conflict with the other ports used by other containers). The running image is connected to a Docker network to allow communication between the app's container and the database's container. If all pipeline commands have run successfully, a 'Success' message will be printed to the console. 


**Risk Management** 
<img width="576" alt="image" src="https://github.com/LRMB27/Final-FCA-Project/assets/144361653/8057d059-a8f3-4642-aa94-e2c920bb69a7">


