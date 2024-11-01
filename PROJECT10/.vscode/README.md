# PROJECT 10
## DEPLOYMENT OF PROMETHEUS STACK USING DOCKER COMPOSE AND TERRAFORM

#### PROJECT OVERVIEW
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


#### Pre-requisites

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

### PROJECT TASK DETAILS
----

#### TASK 1: CREATE A TERRAFORM EC2 INSTANCE

Create an EC2 instance and attach necessary IAM roles to Prepare environment for Terraform deployment.

Detailed Function

- Create an EC2 instance (Ubuntu 22.04)

![alt text](<img/1a Terraform Instance.PNG>)

- Attach IAM roles: AmazonVPCFullAccess, AmazonEC2FullAccess

![alt text](<img/1b select role and create roles.PNG>)

![alt text](<img/1c select AWS service and EC2.PNG>)

![alt text](<img/1d select AmazonVPC full access.PNG>)

![alt text](<img/1e select AmazonEC2 full access.PNG>)

![alt text](<img/1F IAM role name.PNG>)

![alt text](<img/1g modify IAM role.PNG>)

![alt text](<img/1h update IAM role.PNG>)


#### TASK 2: CONNECT TO EC2 INSTANCE

Connect to EC2 instance using SSH inorder to Install Terraform.

Detailed Function

- SSH into instance: ssh -i "AWS_Key1.pem" ubuntu@35.90.14.43

![alt text](<img/2a SSH to instance.PNG>)

Clone the project repository to your instance. The project code is present in the prometheus-observability-stack folder. cd into the folder.

git clone https://github.com/TobiOlajumoke/prometheus-observability-stack


![alt text](<img/2b git clone prometheus-observability-stack.PNG>)

![alt text](<img/2c cd prometheus-observability-stack_structure.PNG>)

----

#### TASK 3: INSTALL TERRAFORM

Install Terraform to Provision AWS infrastructure.
install terraform , initialise and validate

Detailed Action

- Install Terraform: sudo snap install terraform --classic

![alt text](<img/2d install terraform_nitialise_validate.PNG>)


TASK 4: PROVISION SERVER USING TERRRAFORM

Provision AWS EC2 and Security Group using Terraform inorder to Create infrastructure for Prometheus Stack.

Detailed Function

- Modify ec2.tfvars file in terraform-aws/vars folder
- Replace values highlighted in bold with your AWS account and region details
- Update AMI ID if necessary (dependent on region)

![alt text](<img/2f Modify the values of ec2.tfvars file present in the terraform_aws.PNG>)


- Navigate to terraform-aws directory: cd ../prometheus-stack
- Format Terraform configuration: terraform fmt
- Initialize Terraform: terraform init
- Validate Terraform configuration: terraform validate

![alt text](<img/2g provision the AWS EC2 & Security group using Terraform.PNG>)


- Execute and apply Terraform plan: terraform plan --var-file=../vars/ec2.tfvars

![alt text](<img/2h terraform plan.PNG>)


![alt text](<img/2i terraform plan output.PNG>)

New instance for monitoring created.

![alt text](<img/2j Dev monitoring instance.PNG>)

----

#### TASK 5 & 6: CONNECT TO NEW INSTANCE AND INSTALL DOCKER AND DOCKER COMPOSE

Connect to new EC2 instance via SSH to Configure Prometheus Stack.

Install Docker and Docker Compose to Run Prometheus Stack containers.

Detailed Function

- SSH into instance: ssh -i "your-key.pem" ubuntu@your-ec2-public-ip

![alt text](<img/3a SSH to devops monitoring instance.PNG>)

![alt text](<img/3b check the cloud-init logs to see if the user data script is runing well.PNG>)

Docker and Docker compose previosuly installed, so I only verified it

![alt text](<img/3c verify the docker and docker-compose versions.PNG>)


TASK 7: DEPLOY PROMETHEUS STACK USING DOCKER COMPOSE

Deploy Prometheus Stack using Docker Compose so as to Run Prometheus Stack containers.

Detailed Function

- Clone repository: git clone 
- Navigate to repository: cd prometheus-observability-stack
- Execute make command: make all

![alt text](<img/3d clone the project code repository to the server to start deploy prometheus stack.PNG>)

- Deploy stack: sudo docker-compose up -d

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

TASK 8: VALIDATE PROMETHEUS NODE EXPORTER METRICS

Validate Prometheus Node Exporter metrics so as to Verify Prometheus configuration.

Detailed Function

- Access Prometheus: http://your-ec2-public-ip:9090
- Validate targets, rules, and configurations

![alt text](<img/3f_1 Prometheus app.PNG>)

![alt text](<img/4a Prometheus rules & config validation.PNG>)

![alt text](<img/4b Prometheus configuration.PNG>)

![alt text](<img/4c Prometheus rules.PNG>)

![alt text](<img/4d Prometheus target.PNG>)


TASK 9: CONFIGURE GRAFANA

Configure Grafana so as to Visualize Prometheus metrics.

Detailed Function

- Access Grafana: http://your-ec2-public-ip:3000
- Create a new dashboard
- Add Prometheus data source
- Import Node Exporter dashboard


![alt text](<img/3f_3 Grafana app.PNG>)

![alt text](<img/5a  click connection to add prometheus URL as the data source.PNG>)

![alt text](<img/5b click add new data source_prometheus.PNG>)


![alt text](<img/5c add prometheus server url.PNG>)

![alt text](<img/5d click save & test.PNG>)

![alt text](<img/5e successful prometheus API queried.PNG>)




TASK 10: CONFIGURE NODE EXPORTER DASHBOARD

Configure Node Exporter dashboard to Monitor node metrics.

Detailed Action taken

- Import Node Exporter dashboard
- Configure dashboard settings

![alt text](<img/6a Click create dashboard.PNG>)

![alt text](<img/6b Import a dashboard_click discard.PNG>)

![alt text](<img/6c load 10180.PNG>)

![alt text](<img/6d select prometheus as data source.PNG>)

![alt text](<img/6e node exporter metrics after dashboard import.PNG>)

![alt text](<img/6f dashboard import_host overview_memory details_cpu details_network details_disk.PNG>)


TASK 11: SIMULATE & TEST ALERT MANAGER ALERTS

Simulate and test Alert Manager alerts in order to Verify alerting functionality.
High storage and CPU alerts will be tested to Notify administrators of potential issues.

Detailed Function

- Simulate high CPU usage: stress --cpu 8 --timeout 60
- Verify Alert Manager alerts
- Simulate high storage usage

![alt text](<img/7a Alerts section in prometheus.PNG>)

![alt text](<img/7b testing_simulating alerts_runing linux commands in the prometheus server.PNG>)


![alt text](<img/7c 1 result of testing command in the Alert manager UI to confirm the fired alerts.PNG>)

![alt text](<img/7c 2 detailed result.PNG>)

*Rolling back the changes
![alt text](<img/7d 1 rollback the changes_check the fired alerts reverted.PNG>)


*result of rollback command on alert GUI*
![alt text](<img/7d 2 result of rollback command on alert GUI.PNG>)

------

TASK 12: SETUP CLEANUP

To tear down the monitoring setup using terraform command from workstation.

Detailed Function

- cd prometheus-observability-stack/terraform-aws/prometheus-stack
- terraform destroy --var-file=../vars/ec2.tfvars


*Terraform Command applied*
![alt text](<img/8a command to destroy terraform setup.PNG>)

*Comand output*
![alt text](<img/8b destroyed output.PNG>)

----
