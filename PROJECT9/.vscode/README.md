# PROJECT 9
## DEPLOYMENT OF A DOCKERISED FLASK ON AWS INSTANCE




### PROJECT OVERVIEW
---

The primary goal of this project is to design and implement a scalable, efficient, and reliable deployment of a Dockerized Flask web application on an Amazon Web Services (AWS) Elastic Compute Cloud (EC2) instance.


This project involves:

1. Creating an AWS EC2 instance
2. Installing and configuring Docker
3. Cloning the Flask app repository
4. Building and deploying the Dockerized Flask app
5. Pushing the Docker image to Docker Hub
6. Configuring load balancing, auto-scaling, and monitoring (optional)

Benefits

1. Scalability: Dockerization enables easy scaling of the Flask app.
2. Efficiency: Containerization reduces resource utilization.
3. Reliability: AWS EC2 ensures high availability and fault tolerance.
4. Flexibility: Docker Hub facilitates easy image sharing and deployment.
5. Cost-effectiveness: AWS EC2 and Docker reduce infrastructure costs.

------------

#### DOCKER OVERVIEW

Docker is a containerization platform that simplifies how applications are built, shared, and run across different environments. Containers solve the very problems Shade and her team faced:

- Lightweight: Unlike VMs, containers share the host operating system’s kernel, making them significantly smaller and faster.
- Consistency: The environment inside a Docker container is identical across development, testing, and production. You can run the same container on your laptop, in the cloud, or anywhere else without worrying about compatibility issues.
- Portability: Containers can run on any platform that supports Docker, making applications truly portable.

In essence, Docker allows you to package your app and its dependencies into a small, isolated unit known as a container.


#### - Docker uses a client-server architecture:

- Docker Client: The CLI tool you use to interact with Docker.
Docker Daemon: The background service (or server) that manages images, containers, networks, and volumes.
- Docker Images: A read-only blueprint for creating containers. Think of an image as the recipe and the container as the dish.
- Docker Containers: These are the running instances of images. A container is like a lightweight, isolated environment where your app runs.

#### - Docker Images vs Containers
Docker Image: A static file containing the code, dependencies, and runtime for your app. Images are built from Dockerfiles, which we'll explore shortly.
Docker Container: A container is a live, running instance of an image. You can have multiple containers running from the same image, each operating independently.





----------
### Project Work Flow
-----

This workflow deploys a Dockerized Flask application on an AWS EC2 instance, leveraging containerization for efficient and scalable deployment.  
It starts by Creating an AWS EC2 instance, provisioning a virtual server for deployment, to provide a dedicated environment for the Flask app. Then, SSH into the EC2 instance, establishing secure remote access to manage the instance and deploy the app.

Next is to Update the package list to ensure it's up-to-date, preparing for Docker installation. Install Docker, enabling containerization, and start the Docker service, initializing the Docker daemon to prepare for container deployment.

Enable Docker to start automatically on boot, ensuring persistent container deployment. Clone the project repository from GitHub, retrieving the Flask app code, using "git clone".

Change into the project directory, navigating to the project root, to prepare for the Docker build. Build the Docker image from the project code, containerizing the Flask app, using "docker build -t my-flask-app .".

Run the Docker container from the image, deploying the Flask app, using "docker run -p 8000:8000 my-flask-app". Log in to Docker Hub, authenticating to prepare for image push.

Tag the Docker image with the Docker Hub repository, identifying the image for push, using "docker tag my-flask-app:latest your-docker-hub-username/my-flask-app". Push the Docker image to Docker Hub, sharing the containerized Flask app, using "docker push your-docker-hub-username/my-flask-app".

 Verify the deployment by accessing the Flask app through the EC2 instance's Public DNS or Public IP address.

-----
### PROJECT TASK
----

This project task deployment of a Dockerized Flask application on an AWS EC2 instance, leveraging containerization for efficient and scalable deployment.

Below is a summary of the respective task involved


#### TASK 1: Create AWS Instance and Establish SSH Connection

Step 1: AWS in stance creation

- Function: Provision a virtual server for deployment and Provision of a dedicated environment for the Flask app.
- Details: Log in to AWS Management Console, navigate to EC2 Dashboard, launch instance, choose Amazon Linux 2, configure instance type, security group, and key pair.

Step 2: SSH into EC2 Instance

- Function: Establish secure remote access; Manage the instance and deploy the app.
- Details: Find Public DNS (or Public IP), open terminal, SSH into instance using ssh -i "path/to/your/key.pem" ec2-user@public-dns.


#### TASK 2: Docker Installation and starting 

Step 1: Update Package List

- Function: Ensure package index is up-to-date and Prepare for Docker installation.
- Details: Run sudo apt update and sudo yum update -y, verify update completion.

Step 2: Install Docker

- Function: Install Docker engine and dependencies and Enable containerization.
- Details: Run sudo apt install docker.io -y

Step 3: Start and Docker Service

- Function: Initialize Docker daemon and Prepare for container deployment.
- Details: sudo systemctl start docker    
           

Step 4: Enable Docker on Boot

- Function: Configure Docker to start automatically and Ensure persistent container deployment.
- Details:  sudo systemctl status docker   
Run sudo service docker start to verify Docker service status.


#### TASK 3: Docker Project cloning and Docker building 

Step 1: Clone Project Repository

- Function: Retrieve project code from GitHub;Obtain the Flask app code.
- Details: git clone https://github.com/TobiOlajumoke/docker-flask and cd docker-flask  -- Navigate to project root to Prepare for Docker build


Step 2: Build Docker Image

- Function: Create Docker image from project code so as to containerize the Flask app.
- Details: docker build -t flask-application:1.0.0 .
           sudo docker images to verify if images are built

Step 3: Run Docker Container

- Function: Launch Docker container from image and Deploy the Flask app.
- Details: sudo docker run -d -p 8000:8000 flask-application:1.0.0
           sudo docker ps  : to check if container is running
           sudo docker ps -a  : to check the list of all containers

Step 4: Verify Container is running
- Function :  to test if the app is running properly in Browser. 
Deatails : Go to your browser and access your EC2 public IP to check;
http://<your-ec2-public-ip>:8000


#### TASK 4: Push the image to docker hub

Step 1: Log in to Docker Hub and create a repo

- Function: Authenticate with Docker Hub and Prepare for image push.
- Details: sudo docker login : login to docker from terminal

Step 2: Tag Docker Image

- Function: Associate image with Docker Hub repository (Docker Hub username and a repository name) and Identify the image for push.
- Details: Run sudo docker tag flask-application:1.0.0 yourusername/flask-application:1.0.0

Step 3: Push Docker Image to Docker Hub

- Function: Upload image to Docker Hub repository; Share the containerized Flask app.
- Details: Run sudo docker push yourusername/flask-application:1.0.0

  

---
### DCUMENTATION

-------------------------------------------------------

### TASK 1: CREATE AWS INSTANCE AND ESTABLISH SSH CONNECTION 

-----------------------------------------------------

Step 1: AWS in stance creation

Inorder to Provision a virtual server for deployment and Provision of a dedicated environment for the Flask app.

I log in to AWS Management Console, navigate to EC2 Dashboard, launch instance, choose Amazon ubuntu 2, configure instance type, security group, and key pair.

!![alt text](<img/1a Docker instance.PNG>)

- Step 2: SSH into EC2 Instance

- I ssh to my instanc inorder to establish secure remote access; Manage the instance and deploy the app.
e

![alt text](<img/1b SSH instance.PNG>)



----------------------------------------------

### TASK 2: DOCKER INSTALLATION AND STARTING 

---------------------------------------------------

THere are four major step invloved to install , start and enable.

Step 1: Update Package List by Run sudo apt update and sudo yum update -y

Step 2: Install Docker by running Run sudo apt install docker.io -y

![alt text](<img/1c Docker Installation.PNG>)

Step 3: Start and Docker Service using the command : sudo systemctl start docker    
          

Step 4: Enable Docker on Boot by runnning the command sudo systemctl status docker   and 
Run sudo service docker start to verify Docker service status.

![alt text](<img/1d Docker start and status.PNG>)

---------------------------------------------------

### TASK 3: DOCKER PROJECT CLONNING AND DOCKER BUILDING

-------------------------------------------------

Step 1: Clone Project Repository using the command: git clone https://github.com/TobiOlajumoke/docker-flask   
cd docker-flask  -- Navigate to project root to Prepare for Docker build

![alt text](<img/1e clone git.PNG>)

I used the command cat dockerfile to read the content of the file

![alt text](<img/1f cat docker file.PNG>)

Below are the meaning and function of the file content.

1. ARG PYTHON_VERSION=3.11.6   
Explanation: This defines a build-time variable (ARG) that specifies the version of Python to use. Here, it’s set to Python 3.11.6.
Why it's useful: By using an argument, you can easily change the Python version later without altering multiple lines in your Dockerfile. It also makes the Dockerfile more flexible.

2. FROM python:${PYTHON_VERSION}-slim as base
Explanation: This tells Docker to use the official Python image (specifically, a slim version of it) as the base image. The slim variant of the Python image is a lightweight version that contains only the essentials for Python.
Why it's useful: Slim images are smaller and faster to download. This improves efficiency in building and running containers.

3. ENV PYTHONDONTWRITEBYTECODE=1
Explanation: This environment variable prevents Python from writing .pyc files (compiled bytecode files).
Why it's useful: By avoiding the creation of bytecode files, you reduce unnecessary writes to the filesystem, which can be especially helpful in container environments where efficiency is key.

4. ENV PYTHONUNBUFFERED=1
Explanation: This environment variable ensures that Python output is displayed directly to the terminal without buffering.
Why it's useful: In a container environment, you often want logs and outputs to be available immediately for monitoring purposes, so unbuffered output is preferred.

5. WORKDIR /app
Explanation: This sets the working directory inside the container to /app. All subsequent commands in the Dockerfile will run from this directory.
Why it's useful: This isolates the app's code and dependencies inside a specific folder, making it easier to organize.

6. ARG UID=10001
Explanation: This defines a build-time variable to specify the user ID for the application user (appuser), set to 10001 by default.
Why it's useful: By specifying a user ID, you improve security by running the application under a non-root user, which minimizes the risk in case of a container breach.

7. RUN adduser ... appuser
Explanation: This creates a new user (appuser) with the specified user ID, but without a password, home directory, or shell access.
Why it's useful: Running processes inside the container as a non-root user (appuser) is a best practice for security reasons, as it limits the damage that could be done by a malicious user.

8. COPY requirements.txt .
Explanation: This copies the requirements.txt file (which lists Python dependencies) from your local machine into the container’s /app directory.
Why it's useful: This file is required to install the necessary dependencies for the Python application.

9. RUN python -m pip install -r requirements.txt
Explanation: This command installs all the dependencies listed in requirements.txt using Python’s package manager (pip).
Why it's useful: This ensures that all required Python libraries are installed inside the container, making the app self-contained.

10. USER appuser
Explanation: This switches the container's user to appuser (the non-root user we created earlier).
Why it's useful: Running the app as a non-root user enhances security by limiting the scope of what can be accessed within the container.

11. COPY . .**
Explanation: This copies all the files from the current directory on your local machine into the /app directory inside the container.
Why it's useful: This is how the rest of your application code gets into the container.

12. EXPOSE 8000**
Explanation: This informs Docker (and anyone using the image) that the container will listen for network requests on port 8000.
Why it's useful: This is necessary to expose the correct port to the outside world when running the container.

13. CMD ["gunicorn", "--bind", "0.0.0.0:8000", "hello:app"]**

Explanation**: This is the command that runs when the container starts. It launches your Python web application using Gunicorn, which is a WSGI server for Python web apps.   

--bind 0.0.0.0:8000**: Tells Gunicorn to bind the app to all network interfaces on port 8000, making it accessible. 

hello:app**: This specifies the Python module (hello) and the WSGI application instance (app) to run. It assumes your Python application file is named hello.py and it contains a variable app that represents your Flask application.

---------


Step 2: Build Docker Image to create Docker image from project code so as to containerize the Flask app.

I ran the the command : docker build -t flask-application:1.0.0 .  and sudo docker images to verify if images are built

![alt text](<img/1g build docker image.PNG>)


![alt text](<img/1h check image built.PNG>)

Step 3: Run Docker Container inorder to Launch Docker container from image and Deploy the Flask app.

I ran the command the following commands: 

sudo docker run -d -p 8000:8000 flask-application:1.0.0   to run docker and 

sudo docker ps  : to check if container is running and 

sudo docker ps -a  : to check the list of all containers

![alt text](<img/1i run container and check list of all containers.PNG>)

Step 4: Verify Container is running by testing if the app is running properly in Browser. 

I went to my browser and access my EC2 public IP to check;
http://35.91.132.218:8000

I first ensured I edited my inbound rule to add port 8000 on our security group

![alt text](<img/1k edit inbound rule to add port 8000 on our security group.PNG>)

Test in Browser to check if the app is running properly

![alt text](<img/1L Test in Browser to check if the app is running properly.PNG>)

-------------------------------------------------


### TASK 4: PUSH THE IMAGE TO DOCKER HUB

----------------------------------------------------

Step 1: Log in to Docker Hub and create a repo - I first went to docker hompage and click dockerhub 


![alt text](<img/2a click dockerhub at docker hompage.PNG>)

Then I Login to docker and click create repo

![alt text](<img/2a Login to docker and click create repo.PNG>)


![alt text](<img/2b create First docker repo with info.PNG>)


![alt text](<img/2c First docker repo.PNG>)


I login to docker from terminal using command 
sudo docker login
Then I inputed my username and password when asked.

I Tagged my Docker Image using the command : sudo docker tag flask-application:1.0.0 walemeg/flask-application:1.0.0

(Note : teh snap shots here are missing because my git bash closed before I could save the pictures)

and I verified the push in the repository created in my dockerhub

![alt text](<img/3a Verified pushed images in dockerhub.PNG>)

Below is the link to my tagged image on docker hub

https://hub.docker.com/r/walemeg/flask-application/tags