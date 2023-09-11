# Final-FCA-Project
**Project 1**
Project 1 consisted of building a Remote backup automation that could be invoked remotely using http.  The purpose was to demonstrate how automation can be invoked and controlled via a WEB (REST) API.

Project 1 involved the following technical tasks:
1. Install Docker
2. Install MySQL as a Docker image
3. Create a SQL schema

The backup feature involved taking a command from a Flask endpoint and executing the necessary logic in Python. These actions are all recorded in the database.

**Database**
The database can be created using the CLI in MyQL, including the following commands:
CREATE DATABASE fca;

CREATE TABLE log(
  id        INT PRIMARY KEY AUTO_INCREMENT,
  date      TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  action    VARCHAR(100),
  parameter VARCHAR(1000),
  status    VARCHAR(100)
);

**Running the code**
python app.py

# Using Curl

# Backup example
curl -X POST http://localhost:5000 -H "Content-Type: application/json" -d '{"path":"G:\hello"}' 

# Get Log
curl localhost:5000/log?start=2023-06-01&end=2023-08-31

# Get Log
curl localhost:5000/stat

