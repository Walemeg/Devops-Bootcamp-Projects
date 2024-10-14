# PROJECT 8
## SETTING UP A VIRTUAL PRIVATE CLOUD (VPC) USING TERRAFORM AS IAC

### PROJECT OVERVIEW
---

A VPC is a virtual network that can contain EC2 instances as well as network resources for other AWS services. By default, every VPC is isolated from all other networks. A VPC can exist only within an AWS region. When you create a VPC in a region, it won’t show up in any other regions.

Terraform is an Infrastructure as Code (IaC) tool for building, changing and managing cloud and on-premises infrastructure. It codifies APIs into declarative configuration files that can be shared, reused and versioned.

Key Features:

1. Infrastructure as Code (IaC): Define infrastructure configurations in human-readable files.
2. Multi-Cloud Support: Manage resources across AWS, Azure, Google Cloud, and other providers.
3. Declarative Syntax: Specify desired infrastructure state, not the steps to create it.
4. State Management: Tracks infrastructure changes and updates.
5. Modules: Reuse and share infrastructure code.
6. Extensive Library: Supports 100s of cloud and on-premises services.

Benefits:

1. Version Control: Track infrastructure changes.
2. Consistency: Ensure identical environments.
3. Reusability: Share infrastructure code.
4. Automation: Provision infrastructure quickly.
5. Collaboration: Manage infrastructure as a team.
6. Compliance: Enforce security and regulatory standards.

Terraform Workflow:

1. Write: Define infrastructure in configuration files.
2. Plan: Preview infrastructure changes.
3. Apply: Provision or update infrastructure.
4. Destroy: Delete infrastructure.


###### ***Terraform workflow***

![alt text](<img/Terraform pix.JPG>)

------

****This project**** aims to automatically implement a secure, scalable, and available VPC infrastructure on Amazon Web Services (AWS) using Terraform.    

The VPC will have 15 subnets, one per Availability Zone, and will utilize an Internet Gateway, NAT Gateway, route tables, and Network ACLs for secure communication as shown in the requirement below

-----
### Project Design Requirements:]
-----

CIDR Block: 10.0.0.0/16   
Region: us-west-2   
Availability Zones: us-west-2a, us-west-2b, us-west-2c   
Subnets: 15 Subnets (One per availability Zone)   
Public Sunets (3)   
App Subets (3)   
DB Subnets (3)   
Management Subnet (3)   
Platform Subnet (3)   
NAT Gateway for Private subnets   
Internet Gateway for public subnets.   


----------
### Project Work Flow
-----

The workflow for setting up a Virtual Private Cloud (VPC) using Terraform begins with, you create an Amazon Web Services (AWS) EC2 instance using the AWS Management Console.  
Next, you launch the instance and wait for it to initialize.
Once initialized, you retrieve the instance's public IP address from the AWS Management Console.   
Then, using Git Bash, you establish a secure connection to the instance via SSH, utilizing the instance's public IP address and your private key.   
After successfully connecting, you update the package list and install necessary dependencies.   
Subsequently, you clone your Terraform repository from GitHub using the command "git clone (git clone https://github.com/TobiOlajumoke/Terraform-VPC.git)".   
This repository contains your Terraform configuration files for creating the VPC infrastructure.   
You then navigate into the cloned repository and initialize Terraform using "terraform init".   
Following initialization, you apply the Terraform configuration to create the VPC, subnets, security groups, and other resources.      
Finally, you verify the successful deployment of your VPC infrastructure.

-----
### PROJECT TASK
----

This project task involves setting up an AWS VPC with 15 subnets, one per Availability Zone, Internet Gateway, routing table, NAT gateway, and Network ACL.


AWS VPC Implementation with Terraform Workflow

#### TASK 1: Create AWS Instance , Attach IAM role and Establish SSH Connection

Create an instance in AWS, selecting the desired operating system, instance type and configuration settings. Record the instance's public IP address and SSH credentials. Open Git Bash, navigate to your preferred directory and establish an SSH connection to the instance using the command ssh -i "path/to/your/key.pem" ubuntu@public-ip-address.

#### TASK 2: Install Terraform and Clone Terraform Repository

Within the Git Bash terminal, install rerraform and clone the Terraform repository using `git clone to obtain your Terraform structure.

#### TASK 3: Navigate to Terraform Directory

Navigate to the Terraform Variable directory by running cd Terraform-VPC/devs/ in Git Bash terminal.

#### TASK 4: Initialize Terraform

Initialize Terraform by executing terraform init, setting up the Terraform working directory.

#### TASK 5: Review Terraform Configuration

Review the Terraform configuration files, specifically to understand the infrastructure definition.

#### TASK 6: Plan Infrastructure

Generate an execution plan using terraform plan -var-file=../../vars/dev/vpc.tfvars, previewing the infrastructure changes.

#### TASK 7: Apply Infrastructure

Provision the VPC infrastructure by running terraform apply -var-file=../../vars/dev/vpc.tfvars, creating the defined resources.

#### TASK 8: Verify Infrastructure

Verify the infrastructure deployment by checking the AWS Management Console and testing connectivity to public and private subnets.

#### TASK 9: Destroy Infrastructure (Optional)

Optionally, destroy the provisioned infrastructure using terraform destroy -var-file=../../vars/dev/vpc.tfvars for cleanup or testing purposes.
     


---
### DCUMENTATION

-------------------------------------------------------

### TASK 1: CREATE AWS INSTANCE , ATTACH IAM ROLE AND ESTABLISH SSH CONNECTION 

-----------------------------------------------------
I Launched AWS instance and named it Terraform instance

![alt text](<img/1a Launch Terraform instance.PNG>)

![alt text](<img/1b view instance created.PNG>)

I proceeded to creating an IAM role and attached it to the instance as shown below


![alt text](<img/1aa1 IAM click create.PNG>)

![alt text](<img/1aa2 IAM Select AWS-EC2.PNG>)

![alt text](<img/1aa3 IAM Add permission Amazon EC2 full access and Amazon VPC full access.PNG>)

![alt text](<img/1aa4 IAM EC2_VPC_FULLACCESS-.PNG>)

![alt text](<img/1aa5 IAM VIEW ALL EC2_VPC_FULLACCESS.PNG>)

![alt text](<img/1aa6 IAM modify IAM role.PNG>)

I attached IAM role to your instance

![alt text](<img/1aa7 IAM attach IAM role to your instance.PNG>)

![alt text](<img/1b view instance created.PNG>)

I tried to SSH to my instance but it was timing out as shwon below

![alt text](<img/1b-2 Probs1 connection timeout.PNG>)

I had to troubleshoot to ensure all my instance connections setup; I ensured all the ports in my security group are open to inbound traffic.
I checked the VPC in which the default instance was created. I checked my routing table routes and edited it to add another route connecting to my Internet gateway (IGW) as target (If there is no IGW, then create a new IGW and attach it to the VPC where your instance is).

![alt text](<img/1b-1 Probs2 routing table.PNG>)

![alt text](<img/1b-3 Probs3 internet gateway route was added.PNG>)


I saved the changes and did my SSH connection again and it worked as shown 

![alt text](<img/1c SSH to instance.PNG>)


----------------------------------------------

### TASK 2: INSTALL TERRAFORM AND CLONE TERRAFORM REPOSITORY

---------------------------------------------------


I proceeded to install install terraform using the command below in git bash

***sudo install terraform***

![alt text](<img/1d sudo install terraform.PNG>)

I then cloned the Terraform repository from github using `git clone to obtain expected Terraform structure using the command be,low

***git clone https://github.com/TobiOlajumoke/Terraform-VPC.git***

![alt text](<img/1f clone VPC repo to give VPC terraform structure.PNG>)


/***Terraform structure***

![alt text](<img/1e target VPC terraform structure.PNG>)

The terraform code structure shown above can be viewed in Gitbash as shown below by moving to each of the directory and viewing the content to reveal what's in the picture above.

I used the command "cd" [***cd terraform-vpc***]  to move to various directiory

![alt text](<img/1g VPC terraform structure shown in Bit Bash.PNG>)


#### Note this structure allows for:   
1. Modular configuration: Reusable modules (e.g., VPC) can be easily integrated.
2. Environment-specific configuration: Variable values can be defined for different environments (e.g., dev, prod).
3. Separation of concerns: Infrastructure configuration is separated from variable values.
This structure appears to be a Terraform configuration for infrastructure as code (IaC). 

Here's a breakdown of function of each part:

#### infra

- Directory containing infrastructure configuration.

#### infra/vpc

- Subdirectory specific to VPC (Virtual Private Cloud) configuration.

#### infra/vpc/main.tf

- Primary Terraform configuration file for VPC.
- Defines resources, providers, and modules.

#### infra/vpc/variables.tf

- File containing variable declarations for VPC configuration.

### modules

- Directory containing reusable Terraform modules.

#### modules/vpc

- Subdirectory containing VPC-specific module.

#### modules/vpc/.tf files*

- Individual Terraform configuration files for VPC components:
    - (endpoints.tf) Configures VPC endpoints.
    - (Internet-Gateway.tf) Configures Internet Gateways.
    - (NACL.tf) Configures Network Access Control Lists (NACLs).
    - (NAT-Gateway.tf) Configures NAT Gateways.
    - (output.tf) Defines output values for the module.
    - (route-tables.tf) Configures route tables.
    - (subnets.tf) Configures subnets.
    - (variables.tf) Declares variables for the VPC module.
    - (var.tf) Configures the VPC itself.

#### vars

- Directory containing variable values.

#### vars/dev

- Subdirectory containing variable values for the dev environment.

#### vars/dev/vpc.tfvars

- File containing variable values specific to VPC configuration in dev environment.


---------------------------------------------------

### TASK 3: NAVIGATE TO TERRAFORM DIRECTORY

-------------------------------------------------
Navigate to the Terraform variable sub-directory containing variable values for the dev environment.

Execute the command   ***cd into vars/dev/vpc.tfvars***

using any text editor "Nano" ***cat vpc.tfvars*** , I checked my VPC config below and change the #tag owner from "Devops" to my name "Adewale"


![alt text](<img/1h 1 using CAT command to read the config file vpc.tvars.PNG>)
![alt text](<img/1h 2 using CAT command to read the config file vpc.tvars.PNG>)


***tag owner changed to Adewale***

![alt text](<img/1i tag owner changed to Adewale.PNG>)


-------------------------------------------------

### TASK 4-7: INITIALIZE TERRAFORM , GENERATE PLAN AND APPLY

----------------------------------------------------

Next , I navigated to the Terraform configuration directory by running ***cd Terraform-VPC/infra/vpc*** in my Git Bash terminal.

This is the directory where your Terraform files for creating the VPC are located. 

- Firstly initialize Terraform using the command   

***terraform init*** 

This step sets up Terraform for the project. It will download any plugins (like AWS) Terraform needs and prepare the project. 

![alt text](<img/2a terraform init to setup terraform.PNG>)

- Secondly is Execute the plan using the command

 ***terraform plan -var-file=../../vars/dev/vpc.tfvars***

The plan command shows you what changes Terraform will make to your AWS resources. The -var-file is pointing to the file that contains specific values (like network ranges) for the development environment (dev). 

#### Note : In these commands:

- ../../vars/dev/vpc.tfvars is the relative path to the vpc.tfvars file.
- ../../ navigates up two directories from infra/vpc to the root directory.
- vars/dev/ navigates down to the dev directory inside vars.
- vpc.tfvars is the file containing variable values.

By using the -var-file option with the relative path, Terraform can access variable values defined in vpc.tfvars while executing commands in the infra/vpc directory.

After running this command, you’ll get an output showing what Terraform plans to create or modify (like the VPC, subnets, and gateways).

![alt text](<img/2b terraform plan listing the changes to make.PNG>)

![alt text](<img/2c terraform plan listing the changes to make.PNG>)


- Thirdly , is to create the VPC and related resources using terraform apply command below

***terraform apply -var-file=../../vars/dev/vpc.tfvars***

This is where you tell Terraform to actually create the resources on AWS. It will use the values from the vpc.tfvars file and make the necessary changes. You’ll see a summary of the changes before proceeding, and you have to confirm (by typing yes) to allow Terraform to create the VPC, subnets, internet gateways, etc.

![alt text](<img/2d terraform apply.PNG>)


![alt text](<img/2e terraform apply completed.PNG>)


### TASK 8: VALIDATE/VERIFY VPC INFRASTRUCTURE

Next is to move over to the AWS Console the check the Resource Map of the VPC.

I Clicked on the created VPC and scroll down to view the Resource Map to see 15 subnets , 6 route tables, internet gateway and NAT gateway as shown below.

![alt text](<img/3a created VPC and Resource Map..PNG>)




