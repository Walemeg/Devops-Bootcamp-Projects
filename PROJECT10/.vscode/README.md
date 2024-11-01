# PROJECT 10

## DEPLOYMENT OF PROMETHEUS STACK USING DOCKER COMPOSE AND TERRAFORM


### PROJECT OVERVIEW
---

This project deploys Prometheus Stack on AWS using Docker Compose and Terraform, configuring Grafana, Node Exporter Dashboard, and simulating Alert Manager alerts.

### PROJECT COMPONENTS


![alt text](<img/Prometheus Stack Architecture & Workflow.PNG>)
***Prometheus Stack Architecture & Workflow***

- Prometheus: Prometheus is an open-source monitoring system designed to collect metrics from various sources, such as application and system performance data, and store them in a time-series database. It scrapes metrics from exporters, processes them, and provides this data to visualization tools like Grafana for real-time monitoring and alerting.

- Alert Manager: Alert Manager is a component of Prometheus that handles alerts based on predefined rules. It routes alerts to various notification channels, such as email, Slack, or other messaging platforms, and provides a centralized dashboard for managing and viewing alert statuses, helping teams quickly respond to potential issues.

- Node Exporter: Node Exporter is an agent for Prometheus that collects hardware and OS-level metrics from Linux systems, such as CPU usage, memory consumption, and disk I/O. These metrics are exposed at the /metrics endpoint, allowing Prometheus to scrape and monitor server performance effectively.

- Grafana: Grafana is a powerful data visualization and monitoring tool that integrates with Prometheus to create interactive and customizable dashboards. It supports various data sources and provides a user-friendly interface for visualizing complex data sets, helping teams analyze and understand system performance and trends.

- Terraform: Terraform is an open-source Infrastructure as Code (IaC) tool developed by HashiCorp that allows developers to define and provision infrastructure using a high-level configuration language. It supports multiple cloud providers and helps automate the creation, updating, and versioning of infrastructure resources, making infrastructure management efficient and repeatable.

- Docker: Docker is a platform for developing, shipping, and running applications in containers, which are lightweight and portable units that package an application and its dependencies. Docker ensures consistency across different environments, simplifies the deployment process, and isolates applications, making it easier to manage and scale.

- Docker Compose: Docker Compose is a tool for defining and running multi-container Docker applications using a simple YAML file. It allows users to configure application services, networks, and volumes, making it easy to deploy and manage multi-container environments by handling service dependencies and orchestration.

-------


#### PRE-REQUISITES

1. AWS account with EC2, VPC, and IAM permissions
2. Terraform installed on your system
3. Docker installed on your system
4. Docker Compose installed on your system.

-----

### PROJECT WORKFLOW
---

The project workflow begins with creating an AWS EC2 instance, which serves as a temporary environment to install Terraform. This temporary instance (with necessary IAM roles "AmazonVPCFullAccess, AmazonEC2FullAccess") attached enables the installation and utilization of Terraform to provision an AWS EC2 instance and Security Group, creating the necessary infrastructure for Prometheus Stack. This infrastructure will ultimately support the deployment and operation of Prometheus Stack.

Once provisioned, the new EC2 instance is accessed via SSH to install Docker and Docker Compose, enabling containerization and simplified deployment of Prometheus Stack's components. Docker provides a lightweight and portable way to deploy applications in containers, while Docker Compose streamlines the deployment and management of multiple containers.

The Prometheus Stack repository is then cloned to obtain configuration files, which are updated to monitor AWS infrastructure. This involves configuring Prometheus, Alert Manager, and Node Exporter to collect metrics and trigger alerts. Docker Compose subsequently deploys Prometheus Stack containers, comprising Prometheus, Alert Manager, Node Exporter, and Grafana.

Grafana is configured to visualize Prometheus metrics, allowing for real-time monitoring of system performance and health. To verify alerting functionality, high CPU usage is simulated, triggering Alert Manager alerts. Finally, high storage and CPU alerts are configured, ensuring administrators receive notifications of potential issues.

Throughout this workflow, Terraform manages infrastructure provisioning, Docker provides containerization, Docker Compose manages container deployment, Prometheus collects metrics, Grafana visualizes data, and Alert Manager sends notifications. This comprehensive monitoring and alerting solution enables proactive maintenance and optimization of AWS infrastructure, ensuring administrators are notified of potential issues before they become critical.



-----
#### PROJECT TASK OVERVIEW
----

Task 1: Create an EC2 Instance and Attach IAM Roles  
Task 2: Connect to EC2 Instance via SSH  
Task 3: Install Terraform  
Task 4: Provision Server Using Terraform  
Task 5: Connect to New EC2 Instance via SSH  
Task 6: Install Docker and Docker Compose  
Task 7: Deploy Prometheus Stack using Docker Compose  
Task 8: Validate Prometheus Node Exporter Metrics  
Task 9: Configure Grafana  
Task 10: Configure Node Exporter Dashboard  
Task 11: Simulate & Test Alert Manager Alerts
         (Test High Storage & CPU Alert)  
Task 12: Setup Cleanup



----

### PROJECT TASK DOCUMENTATION
----

### TASK 1: CREATE A TERRAFORM EC2 INSTANCE

Create an EC2 instance and attach necessary IAM roles to Prepare environment for Terraform deployment.

Detailed Action

- Create an EC2 instance (Ubuntu 22.04) for Terraform installation; which is used to provisions an AWS EC2 instance and Security Group, creating the necessary infrastructure for Prometheus Stack

*1a Terraform Instance*
![alt text](<img/1a Terraform Instance.PNG>)

- Attach IAM roles: AmazonVPCFullAccess, AmazonEC2FullAccess

*1b select role and create roles*
![alt text](<img/1b select role and create roles.PNG>)

*1c select AWS service and EC2*
![alt text](<img/1c select AWS service and EC2.PNG>)

*1d select AmazonVPC and EC2 full access*
![alt text](<img/1d select AmazonVPC full access.PNG>)

![alt text](<img/1e select AmazonEC2 full access.PNG>)

*Attach IAM role*
![alt text](<img/1F IAM role name.PNG>)

![alt text](<img/1g modify IAM role.PNG>)

![alt text](<img/1h update IAM role.PNG>)

----

### TASK 2 & 3: CONNECT TO EC2 INSTANCE AND INSTALL TERRAFORM

Connect to EC2 instance using SSH inorder to Install Terraform so as to Provision AWS infrastructure. (install terraform , initialise and validate)

Detailed Function

- SSH into instance: ssh -i "AWS_Key1.pem" ubuntu@35.90.14.43

![alt text](<img/2a SSH to instance.PNG>)

Clone the project repository to your instance. The project code is present in the prometheus-observability-stack folder. cd into the folder.

git clone https://github.com/TobiOlajumoke/prometheus-observability-stack


![alt text](<img/2b git clone prometheus-observability-stack.PNG>)

*Graphical view of the prometheus-observability-stack_structure using VScode*
![alt text](<img/2c cd prometheus-observability-stack_structure.PNG>)

----
Note : Explanation of the project files in prometheus-observability-stack.

----

- The alertmanager folder contains the alertmanager.yml file which is the configuration file. 

- The prometheus folder contains alertrules.yml which is responsible for the alerts to be triggered from the prometheus to the alert manager. The prometheus.yml config is also mapped to the alert manager endpoint to fetch, Service discovery is used with the help of a file file_sd_configs to scrape the metrics using the targets.json file.

- Terraform-aws directory allows you to manage and isolate resources effectively. modules contain the reusable terraform code. These contain the Terraform configuration files (main.tf, outputs.tf, variables.tf) for the respective modules.

- The ec2 module also includes user-data.sh script to bootstrap the EC2 instance with Docker and Docker Compose. The security group module will create all the inbound & outbound rules required.

- Prometheus-stack Contains the configuration file main.tf required for running the terraform. Vars contains an ec2.tfvars file which contains variable values specific to all the files for the terraform project.

- The Makefile is used update the provisioned AWS EC2’s public IP address within the configuration files of prometheus.yml and targets.json located in the prometheus directory.

- The docker-compose.yml file incorporates various services Prometheus, Grafana, Node exporter & Alert Manager. These services are mapped with a network named ‘monitor‘ and have an ‘always‘ restart flag as well.

-----

- Install Terraform: sudo snap install terraform --classic 

![alt text](<img/2d 1 install terraform.PNG>)


### TASK 4: PROVISION SERVER USING TERRRAFORM

Provision AWS EC2 and Security Group using Terraform inorder to Create infrastructure for Prometheus Stack.

Detailed Function

- Modify ec2.tfvars file in terraform-aws/vars folder of prometheus stack i.e first change directory to terraform-aws/vars
- Replace values highlighted in bold with your AWS account and region details
- Update AMI ID if necessary (dependent on region)

![alt text](<img/2f Modify the values of ec2.tfvars file present in the terraform_aws.PNG>)


- Navigate to terraform-aws/vars/prometheus-stack directory by using the command: cd ../prometheus-stack
- Format Terraform so that the configuration follow a canonical style and ensures consistent formatting of your configuration files: terraform fmt
- Initialize Terraform i.e Initializes a new or existing Terraform working directory; this Prepares your directory for Terraform operations by downloading necessary provider plugins: terraform init
- Validate Terraform configuration: terraform validate

![alt text](<img/2g provision the AWS EC2 & Security group using Terraform.PNG>)


- Execute and apply Terraform plan: terraform plan --var-file=../vars/ec2.tfvars

![alt text](<img/2h terraform plan.PNG>)


![alt text](<img/2i terraform plan output.PNG>)

New instance for monitoring created with terraform.

![alt text](<img/2j Dev monitoring instance.PNG>)

----

### TASK 5 & 6: CONNECT TO NEW INSTANCE AND INSTALL DOCKER AND DOCKER COMPOSE

Connect to new EC2 instance via SSH to Configure Prometheus Stack.

Install Docker and Docker Compose to Run Prometheus Stack containers.

Detailed Function

- SSH into instance: ssh -i "your-key.pem" ubuntu@your-ec2-public-ip

![alt text](<img/3a SSH to devops monitoring instance.PNG>)

![alt text](<img/3b check the cloud-init logs to see if the user data script is runing well.PNG>)

Docker and Docker compose previosuly installed, so I only verified it

![alt text](<img/3c verify the docker and docker-compose versions.PNG>)


### TASK 7: DEPLOY PROMETHEUS STACK USING DOCKER COMPOSE

Deploy Prometheus Stack using Docker Compose so as to Run Prometheus Stack containers.

Detailed Function

Before we deploy using Docker-compose, we first Navigate to prometheus-observability-stack repository: cd prometheus-observability-stack
- Then execute make command: make all
The make command updates server IP in prometheus config file. Because we are running the node exporter on the same server to fetch the server metrics. It also update the alert manager endpoint to the servers public IP address.

![alt text](<img/3d 1 start deploy prometheus stack after cloning.PNG>)



- then I Deployed the stack using: sudo docker-compose up -d

![alt text](<img/3e Bring up the stack using Docker Compose to deploy Prometheus, Alert manager, Node exporter and Grafana.PNG>)

All monitoring Application now available.
with my servers IP address I can access all the apps on different ports as shown below.

Prometheus: http://your-public-ip-address:9090   
Alert Manager: http://your-public-ip-address:9093   
Grafana: http://your-public-ip-address:3000   



![alt text](<img/3f_1 Prometheus app.PNG>)

![alt text](<img/3f_2 Aler manager app.PNG>)

![alt text](<img/3f_3 Grafana app.PNG>)


Now the stack deployment is done. The rest of the configuration and testing will be done the using the GUI.


----

### TASK 8: VALIDATE PROMETHEUS NODE EXPORTER METRICS

Validate Prometheus Node Exporter metrics by Verify Prometheus configuration.

Detailed Action

- Access Prometheus: http://your-ec2-public-ip:9090
- Validate targets, rules, and configurations

*Prometheus app*
![alt text](<img/3f_1 Prometheus app.PNG>)

*Checking Prometheus config*
![alt text](<img/4a Prometheus rules & config validation.PNG>)

![alt text](<img/4b Prometheus configuration.PNG>)


*Checking Prometheus rules*
![alt text](<img/4c Prometheus rules.PNG>)

![alt text](<img/4d Prometheus target.PNG>)


### TASK 9: CONFIGURE GRAFANA

Configure Grafana so as to Visualize Prometheus metrics.

Detailed Function

- Access Grafana: http://your-ec2-public-ip:3000
- Create a new dashboard
- Add Prometheus data source
- Import Node Exporter dashboard


I looged in to the Grafana app with admin email & password and later updated with my eamil and password.

*Grafana app*
![alt text](<img/3f_3 Grafana app.PNG>)


- I clicked connection to add prometheus URL as the data source

![alt text](<img/5a  click connection to add prometheus URL as the data source.PNG>)
.

- click add new data source_prometheus

![alt text](<img/5b click add new data source_prometheus.PNG>)

- I add prometheus server url

![alt text](<img/5c add prometheus server url.PNG>)

- click save & test

![alt text](<img/5d click save & test.PNG>)

![alt text](<img/5e successful prometheus API queried.PNG>)

----


### TASK 10: CONFIGURE NODE EXPORTER DASHBOARD

Configure Node Exporter dashboard to Monitor node metrics.

Detailed Action taken

- Import Node Exporter dashboard
- Configure dashboard settings

- Click create dashboard

![alt text](<img/6a Click create dashboard.PNG>)

- Import a dashboard and click discard

![alt text](<img/6b Import a dashboard_click discard.PNG>)

- load 10180

![alt text](<img/6c load 10180.PNG>)

- select prometheus as data source

![alt text](<img/6d select prometheus as data source.PNG>)

- Node exporter metrics after dashboard import

![alt text](<img/6e node exporter metrics after dashboard import.PNG>)

![alt text](<img/6f dashboard import_host overview_memory details_cpu details_network details_disk.PNG>)


---

TASK 11: SIMULATE & TEST ALERT MANAGER ALERTS

Simulate and test Alert Manager alerts in order to Verify alerting functionality.
High storage and CPU alerts will be tested to Notify administrators of potential issues.

Detailed Function

- Simulate high CPU usage: stress --cpu 8 --timeout 60
- Verify Alert Manager alerts
- Simulate high storage usage
,
Select Alerts section in prometheus

![alt text](<img/7a Alerts section in prometheus.PNG>)


Testing and simulating alerts by running linux commands in the prometheus server

![alt text](<img/7b testing_simulating alerts_runing linux commands in the prometheus server.PNG>)

Output of testing command in the Alert manager UI to confirm the fired alerts

![alt text](<img/7c 1 result of testing command in the Alert manager UI to confirm the fired alerts.PNG>)

![alt text](<img/7c 2 detailed result.PNG>)

Rolling back the changes

![alt text](<img/7d 1 rollback the changes_check the fired alerts reverted.PNG>)


*result of rollback command on alert GUI*
![alt text](<img/7d 2 result of rollback command on alert GUI.PNG>)

------

### TASK 12: SETUP CLEANUP

To tear down the monitoring setup using terraform command from workstation.

Detailed Function

- cd prometheus-observability-stack/terraform-aws/prometheus-stack
- terraform destroy --var-file=../vars/ec2.tfvars


*Terraform Command applied*
![alt text](<img/8a command to destroy terraform setup.PNG>)

*Comand output*
![alt text](<img/8b destroyed output.PNG>)

----
