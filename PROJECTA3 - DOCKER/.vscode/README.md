# PROJECT 13
## DEPLOYMENT OF A DOCKERISED TASK MANAGEMENT API DEVELOPMENT FOR ELITE CORPORATION

### PROJECT OVERVIEW
---

Elite Corporation, a fast-growing tech firm, is struggling to manage employee tasks effectively. The management has requested a lightweight Task Management API that can run in a Docker container without requiring a database.

As a software engineer at Elite Corporation, I have a task to build a Dockerized Task Management API that allows employees to:

- Add new tasks
- View all tasks
- Update task status
- Delete tasks

Since this is an internal tool for quick task tracking, data will be stored in an in-memory array, meaning all data will reset when the container restarts.

### Project Requirements:
1). Build a Simple Node.js Express API
Your API should have the following endpoints:

- POST /tasks → Add a new task
- GET /tasks → Get all tasks
- PUT /tasks/:id → Update a task
- DELETE /tasks/:id → Delete a task

Data structure for tasks:

[
  { id: 1, title: "Fix server issue", status: "Pending" },
  { id: 2, title: "Deploy new feature", status: "Completed" }
]

2). Write a Dockerfile

Use node:18-alpine as the base image
Copy the project files into the container

Install dependencies
- Expose port 3000
- Run the app inside the container

3). Test the API using Postman or Curl
curl http://localhost:3000/tasks

- Run the Application with Docker
- Build the image:
- docker build -t elite-task-api .
- Run the container:
docker run -p 3000:3000 elite-task-api

The primary goal of this project is to design and implement an internal task management solution that is:

Lightweight: Minimal resource usage for easy deployment
Containerized: Runs in Docker for portability across environments
Database-free: Uses in-memory storage for simplicity
CRUD-capable: Full task lifecycle management (Create, Read, Update, Delete)
Ephemeral: Data persistence is not required (resets on container restart)

The solution must provide a RESTful API with four core endpoints for task management, packaged as a Docker container that can be easily deployed on any infrastructure.



------------

#### DOCKER WORKFLOW BASICS

![alt text](<img/DOCKER OVERVIEW SNAP SHOT.jpeg>)


As shown in above picture below is a step-by-step overview of the Docker workflow:


#### Step 1: Create Dockerfile
Overview: A Dockerfile is a text file containing instructions for building a Docker image. It specifies the base image, dependencies, and application code.

Explanation: A Dockerfile is essentially a recipe for creating a Docker image. It includes commands such as:

- FROM: Specifies the base image (e.g., Ubuntu, Node.js)
- RUN: Executes commands during the build process
- COPY: Copies files from the host system into the image
- EXPOSE: Exposes ports for communication
- CMD: Specifies the default command to run when the container starts

Requirements: Docker installation , Text editor or IDE

Developer Tasks: Write Dockerfile instructions, Specify base image and dependencies

#### Step 2: Build Docker Image
Overview: A Docker image is a lightweight, read-only template for creating containers. It contains the application code, dependencies, and configuration.

Explanation: A Docker image is created by running the docker build command on the Dockerfile. The resulting image is a binary file that can be stored in a registry.

Types of Docker Images:
- Base Image: A minimal image with an operating system (e.g., Ubuntu)
- Official Image: A pre-built image for popular applications (e.g., Node.js, Python)
- Custom Image: A user-created image for specific applications

Requirements:Dockerfile , Docker installation

Developer Tasks: Run docker build command , Specify build context and Dockerfile location
    - Sample command: docker build -t my-node-app .

#### Step 3: Push Image to Registry
Overview: Upload the Docker image to a registry.

Requirements: Docker image , Docker Hub or private registry account

Developer Tasks: Run docker push command , Authenticate with registry
    - command: docker tag my-node-app:latest myusername/my-node-app:latest
    - command: docker push myusername/my-node-app:latest

#### Step 4: Pull Image from Registry
Overview: Download the Docker image from a registry.

Requirements: Docker installation , Registry account

Developer Tasks: Run docker pull command, Specify image name and registry
    - command: docker pull myusername/my-node-app:latest

#### Step 5: Run Docker Container
Overview: Start a container from the Docker image.

Requirements:Docker image , Docker installation

Developer Tasks:Run docker run command , Specify container name, image, and ports
    - command: docker run -p 3000:3000 my-node-app
    - command: docker run -d --name my-container my-node-app

#### Step 6: Manage Containers
Overview: Monitor and manage running containers.

Requirements: Docker installation 

Developer Tasks:
- Run docker ps command (list containers)
- Run docker stop command (stop container)
- Run docker rm command (remove container)




----------
### Project Work Flow
-----

The project begins with setting up the development environment by provisioning an AWS EC2 instance that will serve as our deployment platform. After establishing SSH connectivity, we install Docker to create our containerized environment. The application development starts with creating a Node.js Express server that implements the four required REST endpoints, using an in-memory array for task storage to meet the database-free requirement.

We then containerize the application by creating a Dockerfile that specifies node:18-alpine as the base image for its small footprint and security benefits. The Docker build process copies our application files into the container, installs dependencies, exposes port 3000, and configures the startup command. This packaging ensures the API runs identically across all environments.

After building the Docker image, we run the container with port mapping to make the API accessible. We then thoroughly test each endpoint using curl commands to verify all CRUD operations work as expected. The entire solution provides Elite Corporation with a production-ready task management system that's lightweight, portable, and requires minimal infrastructure overhead.

------------------------

### PROJECT IMPLEMENTATION TASK

#### TASK1 : AWS EC2 Instance Setup and Establish SSH connection

- Launching an EC2 Instance
- Configuring Security Groups
- SSH Connection to EC2 Instance

#### TASK2 : Docker Installation and Verification

- Updating Package Lists
- Installing Docker Engine
- Verifying Installation

#### TASK3 : Application Setup on EC2

- Initializing Node.js Project
- Installing Express

#### TASK4 : API Implementation

- Building the Express Server
- CRUD Endpoints Explanation
- POST /tasks (Add Task)
- GET /tasks (List Tasks)
- PUT /tasks/:id (Update Task)
- DELETE /tasks/:id (Delete Task)

#### TASK5 : Docker Configuration , Building and Running the Container

- Writing the Dockerfile
- Building the Docker Image
- Running the Container with Port Mapping
- Verifying Container Status

#### TASK6 : Testing the API

- Using curl for Endpoint Testing
- Testing Public Accessibility

#### TASK7 : Managing the Container

- Stopping and Starting Containers



  

---
### DCUMENTATION

-------------------------------------------------------

### TASK 1: CREATE AWS INSTANCE AND ESTABLISH SSH CONNECTION 

-----------------------------------------------------

Step 1: AWS in stance creation

Inorder to Provision a virtual server for deployment and Provision of a dedicated environment for the Flask app.

I log in to AWS Management Console, navigate to EC2 Dashboard, launch instance, choose Amazon ubuntu 2, configure instance type, security group, and key pair.

![alt text](<img/1a Docker instance setup.PNG>)

![alt text](<img/1b Docker instance.PNG>)


- Step 2: SSH into EC2 Instance

- I ssh to my instanc inorder to establish secure remote access; Manage the instance and deploy the app.
e

![alt text](<img/1c SSH to instance.PNG>)



----------------------------------------------

### TASK 2: DOCKER INSTALLATION AND VERIFICATION 

---------------------------------------------------

THere are two major step invloved to install Docker.

Step 1: Update Package List by Run sudo apt update 

Step 2: Install Docker by running Run sudo apt install docker.io -y

![alt text](<img/2a Install Docker_apt update.PNG>)


![alt text](<img/2b Docker installed and confirmed.PNG>)

---------------------------------------------------

### TASK 3: NODE.JS APPLICATION SETUP

-------------------------------------------------

Step 1: Create project directory:using the command below

The goal of the project directory is to create a folder in docker instance, move to the directory because every other docker command and files will be run and created there.

***sudo mkdir elite-task-api && cd elite-task-api**

- mkdir creates directory
- && chains commands
- cd changes into directory


Step 2 : Initialize Node.js project:inorder to create package.json configuration file which is used for building docker image
package.json is a JSON file that contains metadata for a project, including:
    - Project name and version
    - Dependencies (libraries and packages required by the project)
    - Scripts (commands to run tasks, such as build, test, or start)

sudo npm init -y
-y accepts all defaults

![alt text](<img/3a Nodejs Appilication Setup on EC2.PNG>)

Step 3 : Install Express using the command "sudo npm install express cors body-perser"

The command adds Express web framework dependency.
- express, cors, and body-parser: These are the packages being installed.
    - express: A popular Node.js web framework for building web applications.
    - cors: A package that enables Cross-Origin Resource Sharing (CORS) in Express.js.
    - body-parser: A package that parses JSON request bodies in Express.js


![alt text](<img/3b sudo npm install express_to add express web framework dependency.PNG>)


---------

### TASK 4: API IMPLEMENTATION

------------

Step 1: Crteate an API implementation server file.
In order to implement the API logic required by Elite corporation, a server.js file or script is created to perform such function. It's one of the files that should be in the docker folder. The A JavaScript file basically contains the code for the API server, using a framework like Express.js. It also defines the API endpoints, handles requests and responses, and interacts with databases or other services. It runs on Node.js environment.

Note  : A Dockerfile that specifies the Node.js environment, copies the server.js file, and defines the command to run the server.

The server file was created using vim commmand as shown  below

![alt text](<img/4a API implementation_create Docker file and Docker Image.PNG>)

![alt text](<img/4a2 API implementation_Java script_Docker image details.PNG>)

This script is a simple task management API built using Express.js. It allows users to:

1. Create new tasks: Send a POST request to /tasks with a title in the request body to create a new task.
2. Get all tasks: Send a GET request to /tasks to retrieve a list of all tasks.
3. Update task status: Send a PUT request to /tasks/:id with a status in the request body to update the status of a task.
4. Delete tasks: Send a DELETE request to /tasks/:id to delete a task.

The API uses a simple in-memory data store to store tasks, and each task has an id, title, and status. The status can be one of "Pending", "In Progress", or "Completed".

---------

#### TASK 5 : DOCKER CONFIGURATION, BUILDING AND RUNNING (Container)

---------


Step 1: A Dockerfile is a text file containing instructions for building a Docker image. It specifies the base image, dependencies, and application code.

The docker file was created using vim commmand as shown  below

![alt text](<img/4a API implementation_create Docker file and Docker Image.PNG>)

![alt text](<img/4b Docker file details.PNG>)

- FROM: Base image (small Alpine Linux with Node.js 18)
- WORKDIR: Container working directory
- COPY: Copies package files
- RUN: Installs dependencies
- EXPOSE: Documents container port
- CMD: Default command to run

Step 2: Docker Build command creates image from Dockerfile  -  
The command "sudo docker build -t elite-task-api ."

-t: Tags image with name

![alt text](<img/5a Docker Build.PNG>)
![alt text](<img/5a2 Docker Build.PNG>)


Step 3 : Docker Run command is for starting a container from the Docker image

![alt text](<img/5b Docker run to run and verify container.PNG>)

sudo docker run -d -p 3000:3000 --name task-api elite-task-api

-d: Detached mode (runs in background)

-p: Port mapping (host:container)

--name: Names container for easier management

The command "sudo docker ps" for lists running containers. It shows status, ports, names

-------

#### TASK 6 : TESTING THE API

-------

Verify Container is running by testing if the app is running properly in Browser. 

I went to my browser and access my EC2 public IP to check;
http://52.201.226.202:3000/

I first ensured I edited my inbound rule to add port 3000 on the security group as shown below

![alt text](<img/6a click security group.PNG>)

![alt text](<img/6b edit inbound rule.PNG>)

![alt text](<img/6c update inbound rule with port 3000.PNG>)

![alt text](<img/6d Test in Browser to check if the app is running properly.PNG>)

![alt text](<img/6d -2  Test in Browser to check if the app is running properly.PNG>)



-------------------------------------------------


### TASK 7 : MANAGING THE CONTAINER

----------------------------------------------------

- Stopping , Starting and view Containers logs

- sudo docker stop task-api
- sudo docker start task-api

- sudo docker logs task-api
for viewing logs - shows container's output as shown below

Useful for debugging

![alt text](<img/7a  managing container.PNG>)
