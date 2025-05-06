## PROJECT 15

### PROJECT TITLE : CI/CD AUTOMATION OF A SCALABLE FINTECH ON AWS

---

### PROJECT OVERVIEW

The goal of this project is to create an *Automated CI/CD Pipeline* for a fintech application, enabling efficient testing, building, and deployment. This pipeline ensures that every time developers push code changes to GitHub:
1. *Continuous Integration (CI)*: The code is automatically tested and validated.
2. *Continuous Deployment (CD)*: The application is deployed to a cloud platform (AWS in this case) without manual intervention.
3. *Infrastructure as Code (IaC)*: Terraform is used to automate the provisioning of cloud infrastructure, ensuring scalability, security, and consistency.

This automation addresses several challenges:
- *Efficiency*: Reduces manual effort in testing and deployment.
- *Security*: Ensures sensitive credentials (e.g., AWS keys) are securely managed using GitHub Secrets.
- *Scalability*: Infrastructure can be dynamically provisioned and destroyed as needed.
- *Reliability*: Automated tests ensure only functional code is deployed.

By implementing this solution, EliteOperation can streamline its development process, reduce errors, and improve operational efficiency.

---

#### Pre-requisites and Tools
Before starting, ensure you have the following:
1. *Pre-requisites*:
   - A GitHub account with repository access.
   - An AWS account with permissions to create EC2 instances, load balancers, and databases.
   - Python installed locally (python3).
   - Terraform installed locally (terraform).
   - VSCode or any text editor for writing code.
2. *Tools to Install*:
   - pytest: For unit testing the Python application.
   - flake8: For linting Python code.
   - pip: Python package manager.
   - AWS CLI: To interact with AWS resources locally if needed.

---

### PROJECT WORKFLOW
The workflow begins by setting up the project structure and writing the Python application code in the app folder. Simultaneously, I create corresponding test cases in the tests folder to validate the functionality of the application. These tests are run locally using pytest to ensure correctness before integrating them into the pipeline.

Next, I configure GitHub Actions workflows in the .github/workflows folder. The ci.yml file defines the Continuous Integration process, which runs tests automatically whenever code is pushed to the main branch. This ensures that only functional code progresses further in the pipeline. The cd.yml file handles Continuous Deployment by provisioning AWS infrastructure using Terraform scripts located in the terraform folder. These scripts define the infrastructure as code, ensuring consistency and scalability.

Before deploying, AWS credentials are securely stored in GitHub Secrets to authenticate Terraform's access to AWS resources. Once pushed to GitHub, the workflows execute sequentially: first testing the application, then provisioning the infrastructure, and finally deploying the application to an AWS EC2 instance.

The README file documents the project, explaining its purpose, setup instructions, and how to use it. By following this flow, developers can focus on writing code while the pipeline handles testing, infrastructure management, and deployment.

---

#### PROJECT STRUCTURE*

eliteoperation-app/
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── cd.yml
├── app/
│   ├── __init__.py
│   ├── app.py
│   └── requirements.txt
├── tests/
│   └── test_app.py
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── .gitignore
├── pytest.ini
└── README.md


---

### IMPLEMENTATION TASK AND STEPS

#### TASK 1: Project Setup
##### Step 1: Create the Directory Structure*
- Purpose: This step creates a new directory for our project.
- Command: mkdir eliteoperation-app

##### Step 2: Write Application Code
- Purpose: To creates a new file to write our application code.
- Command: touch your_app/app.py

##### Step 3: Run Tests Locally
- Purpose: To installs dependencies and runs tests locally. Ensures everything is working as expected.
- Command: pip install -r requirements.txt && pytest tests/

#### TASK 2: CI/CD Pipeline with GitHub Actions
##### Step 1: Configure GitHub Actions Workflow
- Purpose: This step creates a new file for our GitHub Actions workflow for automate testing, building, and deployment.
- Command: touch .github/workflows/ci.yml

##### Step 2: Push to GitHub
- Purpose: Push code to GitHub to trigger the GitHub Actions workflow.
- Command: git add . && git commit -m "Initial commit" && git push origin main

#### TASK 3: Infrastructure as Code (IaC) with Terraform
##### Step 1: Create Terraform Configuration Files
- Purpose: creates new files for our Terraform configuration to manage cloud infrastructure on AWS.
- Command: touch terraform/main.tf terraform/variables.tf terraform/outputs.tf

##### Step 2: Run Terraform Lifecycle
- Purpose:  This step initializes and applies the Terraform configuration to create AWS instances.
- Command: terraform init && terraform apply

#### TASK 4: Security and Documentation
##### Step 1: Store AWS Credentials Securely
- Purpose: Store AWS credentials securely using GitHub Secrets.

##### Step 2: Add, Commit, and Push New Additions to GitHub
- Purpose: Update the repository with new changes.
- Command: git add . && git commit -m "Updated README and Terraform files" && git push origin main

---
### DETAILED TASK IMPLEMENTATION
---
#### 1: PROJECT SETUP*

---

#### 1: Create the Directory Structure*
Using the commands below, I created a structure for the application by first creating a new directory for our project and organizing it into folders for the 
application code, tests, Terraform scripts, and GitHub workflows.
- *Commands*:   mkdir eliteoperation-app
  cd eliteoperation-app
  mkdir app tests terraform .github/workflows
  
- *Output*: A directory structure like this:
  
  eliteoperation-app/
  ├── app/
  ├── tests/
  ├── terraform/
  └── .github/workflows/

  ![alt text](<img/C1-S1 Project directory structure.PNG>)
 ![alt text](<img/C1-S1 Project directory structure_PC_2.PNG>)
  ![alt text](<img/C1-S1 Project directory structure_PC.PNG>) 

---

#### Step 2: Write Application Code
 This step creates a new file for our application code. The app.py file contains simple functions for addition and multiplication.

- *Commands*:
  - Create app/__init__.py (empty file for Python package recognition) using the command
    
    touch app/__init__.py

![alt text](<img/C1-S2 init__.py _ empty file for Python package.PNG>)

  - Create app/app.py using command "
  
  touch app/app.py    
  and write a simple python code in the file
    
    python
    def add(a, b):
        return a + b

    def multiply(a, b):
        return a * b
    

![alt text](<img/C1-S2b python file app,py.PNG>)

![alt text](<img/C1-S2c python file app.py with addition and multiplication functions.PNG>)

---

#### STEP 3: Run Tests Locally
This step ensures the application logic works as expected before integrating it into the pipeline. The pytest command runs the tests in the tests folder, and the output confirms whether the tests passed or failed.

  a) First create a test_app.py file in the test folder using the command 
  
  touch tests/test_app.py


Then write the python test code in the app

    
    import pytest
    from app.app import add, multiply

    def test_add():
        assert add(2, 3) == 5

    def test_multiply():
        assert multiply(2, 3) == 6

 ![alt text](<img/C1-S3b Run test locally_create test python file.PNG>)


  b) To run test locally, create requirement.txt file in Elite directory using command touch requirement.txt 
  and add 
    
    pytest
    flake8
    
![alt text](<img/C1-S3a Run test locally_create requirement txt file in Elite directory.PNG>)


  c) Run tests by using the commands below
    
    pip install -r requirements.txt
    pytest tests/

![alt text](<img/C1-S3bc Run test and Result.PNG>)


---

### TASK 2: CI-CD PIPELINE WITH GITHUB ACTIONS

---

#### STEP 1: Configure GitHub Actions Workflow
- The goal is to automate testing using GitHub Actions workflows. To achieve these, two files are created which defines the automated workflows for testing and deploying the application. The ci.yml file runs tests on every push, and the cd.yml file deploys the application to AWS using Terraform.

a) First create CI yml file in the github/worlflow directory using 
  
  touch .github/workflows/ci.yml   and write the code below in it.
  
    yaml
    name: CI Pipeline
    on:
      push:
        branches:
          - main
    jobs:
      test:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v3
          - uses: actions/setup-python@v4
            with:
              python-version: '3.9'
          - run: |
              pip install -r requirements.txt
              pytest tests/

![alt text](<img/C2-S1a Create CI yml file.PNG>)


  b) secondly create CD yml file in the github/worlflow directory using for deployment using the command 

touch .github/workflows/cd.yml  and write the code below in it.


    yaml
    name: CD Pipeline
    on:
      push:
        branches:
          - main
    jobs:
      deploy:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v3
          - uses: aws-actions/configure-aws-credentials@v2
            with:
              aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
              aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
              aws-region: us-east-1
          - run: |
              cd terraform
              terraform init
              terraform apply -auto-approve
    

![alt text](<img/C2-S1b Create CD yml file.PNG>)

---

#### STEP 2: Push to GitHub
This step uploads the project to GitHub, triggering the CI/CD pipelines defined in the .github/workflows folder.

- *Commands*:
  bash
  git init
  git add .
  git commit -m "Initial commit"
  git branch -M main
  git remote add origin https://github.com/Walemeg/eliteoperation-app.git
  git push -u origin main
  
![alt text](<img/C2-S2 push to Github repository_forced pushed.PNG>)

![alt text](<img/C2-S2b All pushed item on github.PNG>)

![alt text](<img/C2-S2c CI yml file running on github action.PNG>)




---

### TASK 3: INFRASTRUCTURE AS A CODE WITH TERRAFORM

---

#### STEP 1: Write Terraform Scripts

Terraform files are created to define the cloud infrastructure. The main.tf file provisions an EC2 instance, while variables.tf and outputs.tf manage configuration and outputs.

    
![alt text](<img/C3-S1 Terraform folder and files created.PNG>)

![alt text](<img/C3-S1b AWS instance information.PNG>)



![alt text](<img/C3-S1c Terraform main_tf file.PNG>)

  - terraform/main.tf:
    hcl
    provider "aws" {
      region = "us-east-1"
    }

    resource "aws_instance" "example" {
      ami           = "ami-0c02fb55956c7d316"
      instance_type = "t2.micro"
    }

![alt text](<img/C3-S1d Terraform Variables_tf file.PNG>)

  - terraform/variables.tf:
    hcl
    variable "region" {
      default = "us-east-1"
    }

![alt text](<img/C3-S1e Outputs_tf file.PNG>)

  - terraform/outputs.tf:
    hcl
    output "instance_id" {
      value = aws_instance.example.id
    }



---

#### STEP 2: Run Terraform Lifecycle
This step initializes Terraform and provisions the AWS resources defined in the main.tf file.

- *Commands*:
  bash
  cd terraform
  terraform init
  terraform apply
  
![alt text](<img/C3-S2a Terraform init and plan.PNG>)

![alt text](<img/C3-S2b Terraform apply result.PNG>)

![alt text](<img/C3-S2c Output_An EC2 instance created in AWS.PNG>)


---

### *CATEGORY 4: FINALISING AND PUSHING TO GITHUB*

---

#### STEP 1: Add Secrets to GitHub
This step ensures Terraform can authenticate with AWS securely.

- *Steps*:
  1. Go to your GitHub repository settings.
  2. Navigate to "Secrets and variables" > "Actions".
  3. Add AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY.
- *Output*: Secrets securely stored in GitHub.

![alt text](<img/C4-S1 Add secret keys to repository.PNG>)

---

  # EliteOperation Fintech App
  This project automates the CI pipeline for a fintech application using GitHub Actions and Terraform.
  

