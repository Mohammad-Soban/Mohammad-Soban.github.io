---
layout: post
title: You're up and running!
---

## Setting Up a Multi-Container Application with Docker

### Introduction

Docker has become an essential tool in modern software development, allowing developers to package applications and dependencies into lightweight, portable containers. In this tutorial, we’ll explore how to set up a multi-container application using Docker, which is particularly useful for projects with multiple components like frontend, backend, and databases.

#### Prerequisites

Before we begin, ensure you have the following prerequisites:

<ul>
  <li>Docker installed on your machine</li>
  <li>Basic understanding of Docker concepts</li>
  <li>Knowledge of your application’s frontend and backend technologies</li>
</ul>

### Part 1 : Docker Setup

#### Step 1 :- Installing Docker

To install Docker, follow these steps:

1. Visit the Docker website and download the appropriate installer for your operating system.
2. Run the installer and follow the on-screen instructions.
3. Once installation is complete, verify that Docker is installed by running <i>docker --version</i> in your terminal.

#### Step 2 :- Docker Basics

Before diving into multi-container applications, it’s important to understand some basic Docker concepts:

<ul>
  <li><b>Containers:</b> Lightweight, portable environments that encapsulate an application and its dependencies.</li>
  <li><b>Images:</b> Templates used to create containers. Images are created from Dockerfiles, which contain instructions for building the environment.</li>
  <li><b>Dockerfile:</b> A text file that contains all the commands needed to assemble a Docker image.</li>
</ul>

### Part 2 : Creating Docker Images

#### Step 3 :- Frontend Docker File

Create a Dockerfile for the frontend application (assuming it’s built with React). Here’s an example:

```Dockerfile
# Use the official Node.js image as the base image
FROM node:14-alpine

# Set the working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the remaining frontend source code to the working directory
COPY . .

# Expose port 3000
EXPOSE 3000

# Command to run the application
CMD ["npm", "start"]
```

Explanation of each line:

- **FROM**: Specifies the base image to use.
- **WORKDIR**: Sets the working directory inside the container.
- **COPY**: Copies files from the host machine to the container.
- **RUN**: Executes commands inside the container during the build process.
- **EXPOSE**: Exposes a port for communication with the outside world.
- **CMD**: Specifies the command to run when the container starts.

### Step 4: Backend Dockerfile

Create a similar Dockerfile for the backend application.

```Dockerfile
# Use official Node.js image as the base image
FROM node:14-alpine

# Copy package.json and package-lock.json files to the working directory
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy all files to the working directory
COPY . .

# Expose port 5000 to the outside world
EXPOSE 5000

# Command to run the application
CMD ["node", "server.js"]
```

Explanation of each line:

- The first four lines are identical to the frontend Dockerfile, so I won't explain them again here.
- `CMD ["node", "server.js"]`: This specifies that we want to start our server using the `server.js` file in our project root.

### Step 5: Database Dockerfile

Now create a dockerfile for the database application

```Dockerfile
# Use the official MySQL image as the base image
FROM mysql:8.0

# Set environment variables (optional)
ENV MYSQL_ROOT_PASSWORD=root_password
ENV MYSQL_DATABASE=my_database
ENV MYSQL_USER=my_user
ENV MYSQL_PASSWORD=my_password

# Copy the SQL script to initialize the database (if needed)
COPY init.sql /docker-entrypoint-initdb.d/

# Expose port 3306 to the outside world
EXPOSE 3306

# Command to run the MySQL server
CMD ["mysqld"]
```

- **FROM**: Specifies the base image to use.
- **MONGO_INITDB_ROOT_USERNAME** sets the root user's username.
- **MONGO_INITDB_ROOT_PASSWORD** sets the root user's password.
- **COPY**: Copies the MongoDB initialization script to the container.
- **EXPOSE**: Exposes a port for communication with the outside world.
- **CMD**: Specifies the command to run when the container starts.

### Step 6 : Building the images

You can now build both images using the following commands

Building the Frontend Image:

```
docker build -t frontend_image_21BCP296 -f .\frontend\DOCKERFILE .\frontend\
```

Building the Backend Image:

```
docker build -t backend_image_21BCP296  -f .\database\DOCKERFILE .\database\
```

Building the Database Image:

```
docker build -t database_image_21BCP296  -f .\database\DOCKERFILE .\database\
```

### step 7: MySQL docker container

Creating a mysql docker container using prebuild ed image `mysql/mysql-server` with the command :

```
docker run -d --name mysqldb --network Docker_Assignment -p 3306:3306 -e MYSQL_ROOT_PASSWORD=<your_password> mysql:8.0
```

Creating a database and tables inside the mysql container

```
 docker exec -it mysqldb mysql -u root -p
```

Then you will be asked for password which is `your_password`.
After that enter the following SQL queries in order to create a new Database.

```
create database student_db;

show databases;

use test;

CREATE TABLE books (
 id INT AUTO_INCREMENT PRIMARY KEY NOT NULL,
 title VARCHAR(45) NOT NULL,
 `desc` VARCHAR(255) NOT NULL,
 cover VARCHAR(50),
 price INT NOT NULL,
);

desc books;
```

## Part 3: Running the application

### step 8: Building the containers

To build all three containers use this command:

1. Frontend React app:

```
docker run -d --name net_frontend --network Docker_Assignment -p 3000:3000 frontend_image_21bcp296
```

2. Backend NodeJS Express server :

```
docker run -d --name net_backend --network Docker_Assignment -p 5000:5000 backend_image_21bcp321
```

3. MySQL Server:

```
docker run -d --name net_mysql_db --network Docker_Assignment -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:8.0
```

### Step 9 : Checking the docker Image and Container

Checking the containers.
![frontend]({{ site.baseurl }}/images/Container_Docker.png)

Checking the images.
![frontend]({{ site.baseurl }}/images/Images_Docker.png)

### Step 10 : Opening the app in Browser.

Go to the browser and open http://localhost:8080 to see the react app

![frontend]({{ site.baseurl }}/images/IMG-20240425-WA0003.jpg)

![frontend]({{ site.baseurl }}/images/IMG-20240425-WA0004.jpg)

### Step 11 : Checking the Database.

To Check if the Application is running or not we can check that the data is comming to mysql or not by running the query in mysql container

```
Select * from books;
```

![frontend]({{ site.baseurl }}/images/IMG-20240425-WA0005.jpg)

### <center> Thankyou From Mohammad Soban Shaikh : 21BCP296 </center>