# Containers with Docker 🐳

## Description

This demo project is part of Module 7: Containers with Docker from the Nana DevOps Bootcamp. It introduces the fundamentals of using Docker containers with MongoDB and Mongo-Express. The demo project covers:
- Setting up a Docker Network.
- Running MongoDB and Mongo-Express containers.
- Comparing the manual setup process with the Docker Compose YAML file approach. 

<br />


## 🚀 Technologies Used

- <b>Docker for containerization.</b>
- <b>Mongo DB: Serves as a database to persist NodeJS data.</b>
- <b>Mongo-Express: WebUI access for managing Mongo DB.</b>
- <b>Linux: Ubuntu for Server configuration and management.</b>
- <b>NodeJs/npm: Nana's application from DevOps Bootcamp and package manager.</b>


## 🎯 Features

- <b>Create Mongo-Network in docker.</b>
- <b>Download and run MongoDB container.</b>
- <b>Download and run Mongo-Express container.</b>
- <b>Deploy NodeJs application.</b>
- <b>Runnning multiple docker containers with docker-compose.</b>

## 🏗 Project Architecture

<img src=""/>

## ⚙️ Project Configuration:

### Cloning Nana’s NodeJS Application Locally

To clone the NodeJS application from Nana DevOps Bootcamp, follow these steps:

1. Find the repository
2. Clone the repository using git clone

### Pulling Docker Images
The Docker images for this demo are available on [DockerHub](https://hub.docker.com/_/mongo)
1. Pull Image for MongoDB in this example I used the MongoDB:4.4 version as the CPU requires AVX support.

    ```bash
    docker pull mongo:4.4
    ```
2. Pull image for mongo-express.

    ```bash
    docker pull mongo-express
    ```
   
### Creating Mongo Network

1. Create a mongo-network using docker network create.
   
    ```bash
    docker network create mongo-network
    ```
    
    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_7_docker/blob/main/Img/4.%20Create%20mongoNetwork.PNG"/>
    
### Running MongoDB Container
1. Run the MongoDB container using the docker run command.
   
   ```bash
   docker run \ #Docker Run command
   -d \ #Running Container in detach mode
   -p 27017:27017 \ #Mapping host:container ports
   -e MONGO_INITDB_ROOT_USERNAME= admin \ #Environment variable for MongoDB
   -e MONGO_INITDB_ROOT_PASSWORD= admin \ #Environment variable for MongoDB
   --name mongodb4 \ #Renaming the container
   --net mongo-network \ # Adding the container to mongo-network
   mongo:4.4 #Container Image
   
   ```
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_7_docker/blob/main/Img/5%20Running%20Docker%20container%20mongodb4.png"/>
   
### Running Mongo-Express Container
1. Run the Mongo-Express container using the docker run command.

   ```bash
   docker run \ #Docker Run command
    -d \ #Running Container in detach mode
    -p 8081:8081 \ #Mapping host:container ports
    -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \ #Environment variable for Mongo-Express
    -e ME_CONFIG_MONGODB_ADMINPASSWORD=admin \ #Environment variable for Mongo-Express
    -e ME_CONFIG_MONGODB_SERVER=mongodb4 \ #Environment variable for Mongo-Express
    --net mongo-network \ #Adding the container to mongo-network
    --name mongo-express \ #Renaming the container
    mongo-express #Container Image
   ```

   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_7_docker/blob/main/Img/6%20Running%20mongo-express.PNG"/>
   
### Access MongoDB with Browser
1. Open MongoDB using the browser and the mongo-express port:

   http://localhost:8081

2. Create a New Database named user-account.
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_7_docker/blob/main/Img/7%20Creating%20User%20account%20db.png"/>
   
3. Add a new collection named users to the user-account database.

   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_7_docker/blob/main/Img/Collection.png"/>

### Deploy NodeJS application
1. Update the package manager to ensure it has the latest version.

    ```bash
   apt update
   ```

2. Navigate to the app directory and install dependencies with the npm install command.
   
   ```bash
   npm install
   ```
   
3. Run the application locally.

   ```bash
   node server.js &
   ```
   
4. Access the application from the browser

   http://localhost:3000/
   
5. Click on Edit profile and update the profile.

   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_7_docker/blob/main/Img/Adding%20user%20to%20app1.PNG" width="400"/>
   
6. Verify that the user has been updated in the User collection.

    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_7_docker/blob/main/Img/New%20item%20added%20to%20the%20DB.PNG" width="800"/>
    

# 🐳 Running Multiple Containers with DOCKER-COMPOSE
Docker-compose helps manage multiple container applications by defining all services in a YAML file. Instead of running multiple docker run commands manually.

## ✅ Benefits
- <b>Simplifies multi-container management.</b>
- <b>All services are defined in one file.</b>
- <b>Configuration is easier to modify.</b>
- <b>It creates a dedicated docker network and adds all services to the network by default.</b>

1. Create the docker-compose.yaml file

   ```bash
   version: '3'
   services: 
     mongodb: 
       image: mongo:4.4
       ports:
         - 27017:27017
       environment:
         - MONGO_INITDB_ROOT_USERNAME=admin
         - MONGO_INITDB_ROOT_PASSWORD=admin
   
     mongo-express:
       image: mongo-express
       ports:
         - 8081:8081
       restart: always
       environment:
         - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
         - ME_CONFIG_MONGODB_ADMINPASSWORD=admin
         - ME_CONFIG_MONGODB_SERVER=mongodb
       depends_on:
         - "mongodb"

   ```

2. Run docker compose command

   ```bash
   docker compose -f docker-compose.yaml up
   ```

   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_7_docker/blob/main/Img/10%20docker%20compose%20up%20with%20file.png?raw=true"/>

   ⚠️ **Note:**
   - <b>docker-compose is the legacy version of the standalone tool, which was installed separately from Docker. </b>
   - <b>docker compose is the newer, built-in command that is included natively in Docker.</b>
   
4. Run the NodeJS application locally.

   ```bash
   node server.js &
   ```
   
5. Access the application from the browser.

   http://localhost:3000/

6. Access MongoDB from the browser.

   http://localhost:8081/

   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_7_docker/blob/main/Img/11%20docker%20compose%20up%20mongo%20express.png"/>

7. Add the user-account database.
   
8. Add the user collection.

9. Click on Edit profile and update the profile.

   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_7_docker/blob/main/Img/Adding%20user%20to%20app.PNG" width="400"/>

10. Verify that the user has been updated in the User collection.

    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_7_docker/blob/main/Img/Unicorn%20in%20mongoexpress.png" width="800"/>

11. Shut down containers and remove the network.
   
   ```bash
   docker-compose -f docker-compose.yaml down
   ```

   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_7_docker/blob/main/Img/13%20docker%20compose%20down.PNG"/>
    


## demo app - developing with Docker

This demo app shows a simple user profile app set up using 
- index.html with pure js and css styles
- nodejs backend with express module
- mongodb for data storage

All components are docker-based

### With Docker

#### To start the application

Step 1: Create docker network

    docker network create mongo-network 

Step 2: start mongodb 

    docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo    

Step 3: start mongo-express
    
    docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express   

_NOTE: creating docker-network in optional. You can start both containers in a default network. In this case, just emit `--net` flag in `docker run` command_

Step 4: open mongo-express from browser

    http://localhost:8081

Step 5: create `user-account` _db_ and `users` _collection_ in mongo-express

Step 6: Start your nodejs application locally - go to `app` directory of project 

    cd app
    npm install 
    node server.js
    
Step 7: Access you nodejs application UI from browser

    http://localhost:3000

### With Docker Compose

#### To start the application

Step 1: start mongodb and mongo-express

    docker-compose -f docker-compose.yaml up
    
_You can access the mongo-express under localhost:8080 from your browser_
    
Step 2: in mongo-express UI - create a new database "user-account"

Step 3: in mongo-express UI - create a new collection "users" in the database "user-account"       
    
Step 4: start node server 

    cd app
    npm install
    node server.js
    
Step 5: access the nodejs application from browser 

    http://localhost:3000

#### To build a docker image from the application

    docker build -t my-app:1.0 .       
    
The dot "." at the end of the command denotes location of the Dockerfile.
