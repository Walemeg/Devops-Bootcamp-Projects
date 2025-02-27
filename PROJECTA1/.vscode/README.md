# PROJECT TITLE 1 : 
## AUTOMATING THE DEPLOYMENT AND MANAGEMENT OF EILTE CORPORATION'S  WEB-SERVER AND DATABASE SERVER USING ANSIBLE

### PROJECT FOCUS : 
Installation Nginx on Eilte’s corperations Web servers and Apache on their database servers using Ansible playbooks and roles:

The project  involves Setting up five EC2 instances on AWS, one for the controlling node and two instances each for web servers and database servers. The controlling node will be used to install Ansible, which will then be used to connect to the web servers and database servers using SSH keys. Ansible playbooks and roles will be created to automate the setup of web servers and database servers with Nginx and Apache, respectively. The playbooks will also retrieve the total free disk space and available disk space on each server.


### EILTE CORPORATION PROJECT WORKFLOW DIAGRAM


![alt text](<img/EILTE CORPORATION DIAGRAM.png>)


### EILTE CORPORATION PROJECT WORKFLOW INCLUDE:

### 1) Infrastructure Setup:

Five EC2 instances are created on AWS: one for the Ansible control node, two for web servers, and two for database servers. The control node is the central machine from which Ansible will manage the other instances. The web servers will host Nginx, while the database servers will host Apache.

### 2) IP Addresses: 

Public IPs are used for this task to facilitate SSH access from the control node to the managed nodes (web and database servers). This is necessary because the control node needs to communicate with the managed nodes over the internet.

### 3) Ansible Installation:

Ansible is installed on the control node. This is the machine from which all automation tasks will be executed. Ansible uses SSH to connect to the managed nodes (web and database servers), so SSH keys are generated and shared to enable passwordless authentication.

### 4) Playbook and Role Creation:

Playbooks are written to define the tasks to be executed on the web and database servers. Roles are used to organize the playbooks into reusable components. For example:

A role for web servers will install Nginx, check disk space, and ensure necessary packages are installed.

A role for database servers will install Apache, check disk space, and ensure necessary packages are installed.

### 5) Execution and Monitoring:

The playbooks are executed from the control node. Ansible connects to the web and database servers via SSH and performs the tasks defined in the playbooks.

Disk space and memory usage are monitored using commands like df and free to ensure the servers have sufficient resources.

### 6) Verification:

After the playbooks are executed, the control node verifies that Nginx and Apache are installed and running on the respective servers. It also checks that the disk space and memory usage commands return the expected results.

This workflow ensures that EliteCorpearations' web and database servers are configured consistently, efficiently, and securely.


-----
## PROJECT TASK:

## TASK 1: LAUNCH AWS EC2 INSTANCES FOR FIVE SERVERS

- I Logged in to the AWS Management Console, navigated to the EC2 dashboard , configured the instance details and clicked on "Launch Instance" as shown below.and select the desired Amazon Machine Image (AMI).

![alt text](<img/1a five instances launched.PNG>)

![alt text](<img/1b five instances renamed.PNG>)


### Public IPs for the five servers

- ControllerIP: 18.212.28.70
- Webserver1: 184.72.99.168
- Webserver2: 44.211.155.76
- Database Server1: 18.212.16.44
- Database Sever2: 18.212.166.12

## TASK 2: INSTALLATION OF ANSIBLE ON THE CONTROLLING NODE
- I started by connected through SSH into the controlling node using the key pair (AWS1.pem) as shown below

![alt text](<img/2a ssh conncetion to controller node.PNG>)


- Next is to Install Ansible on the Controlling Node by first Updating the package list and then installing Ansible using the commands below

$ sudo apt update
$ sudo apt install ansible

![alt text](<img/2b Ansible installation.PNG>)

![alt text](<img/2c Ansible installation confirmed.PNG>)


## TASK 3: SSH KEY GENERATION AND CONNECTION TO TARGET SERVERS- 

- I started by Generating SSH keys using the ssh-keygen command.

$ ssh-keygen 

![alt text](<img/3a SSH Key generation.PNG>)

- The generated public key was updated in the target server's authorised key

![alt text](<img/3c vi editing the four servers with controller keygen.PNG>)

![alt text](<img/3b controller generated key updated in the node's authorized_keys.PNG>)

- I verified trhat I can log into the target web and database servers

![alt text](<img/3d logging from controller to all servers vis ssh.PNG>)


## TASK 4: INVENTORY FILE CREATION AND DEFINING OF SERVERS


- I created a new file in Ansible folder named Inventory using the touch command 

![alt text](<img/4a inventory file created in Ansible.PNG>)

- and I edited the content to define the inventory using the vi command adding the contents below

Groups servers for targeted playbook execution.

[web_servers]
184.72.99.168 
44.211.155.76 

[db_servers]
18.212.16.44 
18.212.166.12 

![alt text](<img/4b servers ip updated in inventory file.PNG>)


## TASK 5 : WEB SERVER AND DATABASE SERVER PLAYBOOK CREATED 

- I created a new file named webserver.yml and databaseserver.yml to define the playbook using the touch command as shown below


![alt text](<img/5a Playbook yml created for webserver and database server.PNG>)


- I edited it and added the following contents to the webserver.yml to define the playbook as shown below

![alt text](<img/5b webservers playbook.PNG>)


- I edited it and added the following contents to the databaseserver.yml to define the playbook as shown below


![alt text](<img/5c databaseservers playbook.PNG>)


----
### Below is the explanation of each step of this Ansible playbook:
----

Step 1: ---
- This is the beginning of the YAML file, indicating the start of the playbook.

Step 2: - name: Configure web servers
- This line defines the name of the play, which is "Configure web servers".
- The - symbol indicates a new play.

Step 3: hosts: web_servers
- This line specifies the hosts that this play will run against, which is the group of servers defined as "web_servers" in the inventory file.

Step 4: become: yes
- This line specifies that this play should be run with elevated privileges (i.e., as root).
- The become keyword is used to escalate privileges.

Step 5: tasks:
- This line begins the definition of the tasks that will be executed as part of this play.

Step 6: - name: Install Nginx
- This line defines a new task, which is to install Nginx.
- The - symbol indicates a new task.

Step 7: apt: name: nginx state: present
- This line specifies the action to take for the "Install Nginx" task.
- The apt module is used to manage packages on Debian-based systems.
- The name parameter specifies the package to install (Nginx).
- The state parameter specifies the desired state of the package (present).

Step 8: - name: Get total free disk space
- This line defines a new task, which is to retrieve the total free disk space.

Step 9: shell: df -h register: disk_space
- This line specifies the action to take for the "Get total free disk space" task.
- The shell module is used to execute a shell command.
- The df -h command retrieves the disk usage statistics in human-readable format.
- The register parameter specifies that the output of the command should be stored in a variable named disk_space.

Step 10: - name: Get available disk space
- This line defines a new task, which is to retrieve the available disk space.

Step 11: shell: free -h register: available_space
- This line specifies the action to take for the "Get available disk space" task.
- The free -h command retrieves the memory and swap usage statistics in human-readable format.
- The register parameter specifies that the output of the command should be stored in a variable named available_space.

Step 12: - debug: msg: "Total free disk space: {{ disk_space.stdout }}"
- This line defines a new task, which is to print the total free disk space.
- The debug module is used to print debug messages.
- The msg parameter specifies the message to print.
- The {{ disk_space.stdout }} syntax is used to access the output of the df -h command stored in the disk_space variable.

Step 13: - debug: msg: "Available disk space: {{ available_space.stdout }}"
- This line defines a new task, which is to print the available disk space.
- The debug module is used to print debug messages.
- The msg parameter specifies the message to print.
- The {{ available_space.stdout }} syntax is used to access the output of the free -h command stored in the available_space variable.


## TASK 6: ANSIBLE PLAYBOOK EXECUTION

- I executed the webserver.yml playbook using the command ansible-playbook -i inventory webserver.yml

![alt text](<img/6a Webserver Playbook execution.PNG>)

- I executed the database.yml playbook using the command ansible-playbook -i inventory databaseserver.yml

![alt text](<img/6b databaseserver Playbook execution.PNG>)


## TASK 7 : DEPLOYMENT VERIFICATION

- I verify that the playbooks executed successfully by checking the output of the ansible-playbook command
- I Verify that the Nginx and Apache services are running on the web servers and database servers, respectively using teh command below

$ systemctl status nginx
$ systemctl status apache2

- Web Server

![alt text](<img/7a weserver1 Nginx status.PNG>)

![alt text](<img/7b weserver2 Nginx status.PNG>)

- Database Server

![alt text](<img/7c databaseserver1 Apache status.PNG>)

![alt text](<img/7d databaseserver2 Apache status.PNG>)


---
---

#### The End Of Project 1