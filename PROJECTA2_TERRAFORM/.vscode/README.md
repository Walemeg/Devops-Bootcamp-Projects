# PROJECT TITLE 2 : 
## AUTOMATING EILTE CORPORATION'S AWS INFRASTRUCTURE USING TERRAFORM

### PROJECT FOCUS : 

EilteCorporation is a fast-growing company that wants to automate 
its cloud infrastructure to ensure scalability and reliability. 
Right now, they are deploying their resources manually, 
which is time-consuming and prone to errors.


This focus of the project is to automate EliteCorporation's 
AWS infrastructure using Terraform, ensuring scalability, 
reliability, and flexibility. 
By defining variables, managing state, and modifying/destroying 
resources, the process eliminates manual errors and streamlines cloud operations.


---

### Project Requirements: to set up a basic AWS Infrastructure 

1. Use Terraform to provision an EC2 instance in AWS.
- The instance should run on Ubuntu.
- It should be deployed in a specific region (e.g., us-east-1).
Define and Use Variables

2. Instead of hardcoding values, use variables for:
- AWS region
- Instance type (e.g., t2.micro)
- Key pair name
- Use Terraform State Management

3. After applying your Terraform code, check the Terraform state to verify that your infrastructure is being tracked.
Modify and Destroy Infrastructure

4. Change the instance type and re-apply your configuration.
Once verified, destroy the infrastructure using Terraform.

---------------------

### EILTE CORPORATION PROJECT WORKFLOW 


The project involves automating EliteCorporation's AWS infrastructure 
using Terraform to provision and manage an EC2 instance running 
Ubuntu in a specific region (e.g., us-east-1). 
The workflow begins with setting up the AWS CLI to authenticate and 
connect to AWS, followed by installing Terraform on the local machine.
Visual Studio Code (VS Code) is used as the integrated development environment(IDE)
to write, manage, and execute Terraform configurations. 
A Terraform configuration is created to define the AWS provider,  EC2 instance, and associated resources. Variables are used for the 
 AWS region, instance type, and key pair name to make the configuration reusable and adaptable. 
 Terraform state management ensures that the infrastructure is tracked
  and can be verified after provisioning. 
  Once the EC2 instance is deployed, modifications are made to the 
  instance type, and the configuration is re-applied to demonstrate 
  Terraform's ability to update resources. 
  Finally, the infrastructure is destroyed using Terraform to 
  clean up resources and avoid unnecessary costs. 
  This workflow ensures scalability, reliability, and automation, 
  eliminating the manual deployment process that is prone to errors.


### EILTE CORPORATION PROJECT WORKFLOW STEPS INCLUDE:


#### 1) Connect to AWS Using AWS CLI

- Install AWS CLI.

- Configure AWS CLI with credentials.

#### 2) Install Terraform

- Download and install Terraform.

- Verify Terraform installation.

#### 3) Set Up Visual Studio Code

- Open VS Code and create a project directory.

- Install Terraform extension in VS Code.

#### 4) Create Terraform Configuration

- Define AWS provider and EC2 instance in main.tf.

- Define variables in variables.tf.


#### 5) Initialize and Apply Terraform Configuration

- Run terraform init to initialize Terraform.

- Run terraform apply to provision the EC2 instance.

- Use Terraform State Management

- Run terraform show to verify the Terraform state.

#### 6) Modify and Destroy Infrastructure

- Update the instance type in variables.tf.

- Re-apply the configuration using terraform apply.

- Destroy the infrastructure using terraform destroy.

-----
## PROJECT TASK:

## TASK 1: CONNECT TO AWS USING AWS CLI

- I downloaded and installed the AWS CLI from the official website: https://aws.amazon.com/cli/.



![alt text](<img/1a AWS CLI download and installation guide.PNG>)

- AWS CLI installation confirmed using the command
***AWS --version***

![alt text](<img/1b AWS CLI installation confirmed.PNG>)


- Add necessary permissions for AWS CLI; selecting the IAM user , adding required permissions and creating the access and secret keys.

![alt text](<img/1c-i AWS CLI access_select user.PNG>)

![alt text](<img/1c-ii Add Permission to user.PNG>)

![alt text](<img/1c-iii Review and Add Permission to user.PNG>)

![alt text](<img/1d Add EC2 permission and click next.PNG>)

![alt text](<img/1e Create access Key.PNG>)

![alt text](<img/1f Select access key for AWS CLI.PNG>)

![alt text](<img/1g select create Access key.PNG>)

![alt text](<img/1h Click Add permisison again.PNG>)

![alt text](<img/1i Access and Secret Access Keys.PNG>)

- I proceeded with configuring of AWS CLI by running the command "AWS Configure" , inputed the following credentials (access key , secret key etc), which authenticates the AWS CLI to interact with your AWS account.

![alt text](<img/1j connected to AWS through AWS CLI.PNG>)


## TASK 2:  DOWNLOAD AND INSTALL TERRAFORM ON LOCAL MACHINE

I downloaded Terraform from the official website: https://www.terraform.io/downloads.html and installed Terraform on my local machine andf updated necessary settings on my PC as shown below.

![alt text](<img/2a_i Terraform download from website.PNG>)

- create terraform folder in C_drive

![alt text](<img/2a_ii create terraform folder in C_drive.PNG>)

- create environment variable for Terraform on PC

![alt text](<img/2a_iii create environment variable for Terraform on PC.PNG>)



![alt text](<img/2a_iv environment variable confirmed.PNG>)

- Terraform installation and confirmation

![alt text](<img/2b Terraform installation and confirmation using git bash or powershell.PNG>)



## TASK 3: SET UP VISUAL STUDIO CODE TO CREATE PROJECT DIRECTORY

I opened VS Code terminal, created a directory for the Terraform project using the instance information below:

![alt text](<img/3a Instance informations AMI _Instance type etc.PNG>)

- Create Terraform Configuration

Below is a summary of the Terraform structures and purpose.

![alt text](<img/3 Terraform structures.PNG>)


#### For this project I created a new file name called main.tf and variable.tf and define the AWS provider and EC2 instance:

- Main.tf file with hardcoded value 

![alt text](<img/3b Terraform config for main.tf_hardcoded.PNG>)

- Main.tf file with variables as required in the project instead of hardcoded values

![alt text](<img/3b_i Terraform config for main.tf_variable used instead of hardcoded values.PNG>)

- Variable.tf file
![alt text](<img/3c Terraform config for Variable.tf.PNG>)


- Terraform initialisation with ***Terraform Init*** command

![alt text](<img/3d Terraform initialisation command.PNG>)

![alt text](<img/3d_i Output of  Terraform init.PNG>)

- Terraform Plan

![alt text](<img/3d_ii Output of  Terraform Plan.PNG>)


- Terraform Apply

![alt text](<img/3e_i Output of  Terraform Apply.PNG>)

![alt text](<img/3e_ii Output of  Terraform Apply.PNG>)


- Instance created using Terraform

![alt text](<img/3f Instance created using Terraform.PNG>)


- Terraform state showing monitored infrastructure
***Terraform show*** command to track infrastructure

![alt text](<img/3G Terraform state showing monitored infrastructure.PNG>)

![alt text](<img/3G_i Terraform show command to track infrastructure.PNG>)


## TASK 4: UPDATE INSTANCE RESOURCES USING TERRAFORM (MODIFY AND DESTROY INFRASTRUCTURE)

To demonstrates Terraform's ability to update resources, I updated the variables.tf file to change the instance type to t2.small from t2.micro

![alt text](<img/4a Instance type changed in variable and terraform apply done.PNG>)

- Instance type changes reflected in AWS Instance

![alt text](<img/4b Instance type changes reflected in AWS Instance.PNG>)

- Terrafrom destroy command implemented
![alt text](<img/5a Terrafrom destroyed implemented.PNG>)

![alt text](<img/5a_i Terrafrom destroyed implemented.PNG>)


- AWS instance destroyed

![alt text](<img/5b AWS instance destroyed.PNG>)

---
---

#### The End Of Project 2