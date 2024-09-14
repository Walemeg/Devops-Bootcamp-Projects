# PROJECT 5
## SETUP SERVICE DISCOVERY USING NGINX AND CONSUL

### GOAL

The goal of this project is to design and implement a scalable, highly available, and efficient service discovery mechanism, enabling seamless communication between microservices in a distributed system. This project aims to address the challenges of managing multiple services, instances, and configurations in a dynamic cloud environment. By integrating Nginx and Consul, the project seeks to provide a robust solution for:

- Automating service registration and discovery
- Load balancing traffic across multiple service instances
- Monitoring service health and availability
- Managing dynamic configurations and updates
- Ensuring high availability and fault tolerance

Objectives:

1. Service Discovery: Implement a service discovery mechanism that allows microservices to register themselves and be discovered by other services.
2. Load Balancing: Use Nginx to distribute traffic across multiple instances of a service, ensuring high availability and scalability.
3. Health Checks: Integrate health checks to monitor service instances and automatically remove unhealthy instances from the load balancer.
4. Dynamic Configuration: Utilize Consul's key-value store to manage and update Nginx configurations dynamically, without requiring manual intervention.
5. High Availability: Ensure the service discovery mechanism is highly available and fault-tolerant, using Consul's distributed architecture.



### Key Components of this project:
For this project I will be using  four instances/servers (one Consul server, two backend servers(nginX) and one load balance server(nginX))

1. Consul Server:
    - Function: Service discovery and configuration management
    - Provides a centralized registry for services
    - Stores service information in a key-value store
    - Offers health checking and monitoring capabilities

2. Two Backend Servers (Nginx):
    - Function: Serve applications/services and register with Consul
    - Register themselves with Consul using a script
    - Send heartbeats to Consul to indicate health
    - Serve applications/services through Nginx reverse proxy

3. Load Balancder (Nginx):
    - Function: Reverse proxy and load balancing
    - Serves as a reverse proxy for applications/services on backend servers
    - Load balances traffic across backend servers

#### Project Work Flow
- **The Consul Server** receives registration requests from Backend Server 1 and Backend Server 2, stores service information in the key-value store and Provides service discovery and health checks to Load Balancer (Nginx)

- **The Backend Server1 and Backend Server2** register themselves with Consul Server , Send heartbeats to Consul Server to indicate health and  receive traffic from Load Balancer.

- **The Load Balancer** queries Consul Server for available backend services ,uses Consul health checks to determine backend server health and load balances traffic across healthy backend servers

#### Key Concepts Covered

AWS (EC2 and Route 53) , Linux(Ubuntu)
Nginx , Consul Environment Setup , Service Registration with Consul , Health Checks and Failover , Load Balancing , Monitoring and Logging , Testing and Validation.

### PROJECT TASK

Task 1: Deploy 4 Ubuntu Server 
- one Consul server, two backend servers and one load balance server.
- allow required ports in their security group and Set up architecture

Task 2: Setup Consul Server

- Install and configure Consul as the service discovery server
- Set up Consul cluster and datacenter
- Configure Consul key-value store
- Start Consul service

Task 3: Setup two Backend Server

- Install and configure Nginx as a reverse proxy for the application/service on both servers
- Register the service with Consul using a script
- Configure the service to heartbeats to Consul
- Start the service
- Configure Nginx to serve the application/service

Task 4: Setup Load Balancer

- Install and configure Nginx as the load balancer
- Configure Nginx to use Consul for service discovery
- Set up Nginx to load balance traffic across Backend Server 1 and Backend Server 2
- Configure Nginx to use Consul health checks for backend servers
- Start Nginx service

Task 5 : Validate Service Discovery Setup




### DCUMENTATION

### TASK-1 : DEPLOY FOUR UBUNTU SERVER 

- I launceed four Unbuntu server named Consul, BE1, BE2 and LB respectively as shown below.


![alt text](<img/1e all instances renamed.PNG>)

- The next things is to open all required Ports In The Security Group.
(The Consul service requires the specific ports below to be opened to function correctly) 

Consul Servers ports

![alt text](<img/7aaaaaaaa consul servers ports.PNG>)


I selected the security group for each of my instances and proceeded to set inbound rule under the security group. 

I selected each instance, click on Security, and then clicked on the security group ID, Clicked on Edit inbound rules, clicked on Add rule and enter the Port range. The appropriate CIDR block was also chosen as shown below.
 (I started with consul instance and repeated same process for the four instances.)

![alt text](<img/2a select consul security group.PNG>)

- Click on **Edit inbound rules**.

![alt text](<img/2b edit inbound rule.PNG>)

![alt text](<img/2c consul inbound rules edited_port opened.PNG>)


Note :
For the purpose of this project, security measures have been intentionally relaxed by opening SSH, HTTP, HTTPS, and Consul ports to all traffic (0.0.0.0) for rapid development and testing (It's insecure and not recommended for a production setting;  separate security groups should be created for each type of instance (consul server, backend server, load balancer), strictly limit inbound and outbound traffic to necessary ports and IP addressesetc) 


### TASK-2 : SETUP CONSUL SERVER

- I opened my terminal and SSH into the consul server and run ***sudo apt update*** to refresh the package cache.

![alt text](<img/3a SSH to consul server.PNG>)

![alt text](<img/3a1 sudo apt update.PNG>)


- I visited the consul downloads page to copy the installation command.

![alt text](<img/3b consult download page.PNG>)

I execute the following commands (copied from consul page) to install Consul.

***wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg***

***echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list***

***sudo apt update && sudo apt install consul***

![alt text](<img/3c consul installation command executed.PNG>)


- I confirmed Consul installation by checking its version with the ***consul --version command***

(Note : All the Consul server configurations are located in the /etc/consul.d folder).

- Inorder to configure the Consul server, I started by backing up the default configuration file consul.hcl; this was done by renaming it to consul.hcl.back. I exexcuted the backup by running the command ***sudo mv /etc/consul.d/consul.hcl /etc/consul.d/consul.hcl.back*** 

- I generated an encrypted key using the ***consul keygen*** command.

The last three commands applied is shown below

![alt text](<img/3d confirm installation_ backup consul file_rename_encryption key.PNG>)

- I created a new file named consul.hcl in the /etc/consul.d directory, using the command  ***sudo vi /etc/consul.d/consul.hcl***

I copied the content below to the consul.hcl file, replacing <YOUR_ENCRYPTED_KEY> with the encrypted key you generated:

"bind_addr" = "0.0.0.0"
"client_addr" = "0.0.0.0"
"data_dir" = "/var/consul"
"encrypt" = "<YOUR_ENCRYPTED_KEY>"
"datacenter" = "dc1"
"ui" = true
"server" = true
"log_level" = "INFO"
18

![alt text](<img/3f Consul config settings.PNG>)

I saved the file after adding the content using :wq.

Below is the explanation of each configuration setting in the Consul configuration file:

i) bind_addr = "0.0.0.0": Specifies the IP address on which Consul will bind to listen for incoming connections. 0.0.0.0 means Consul will listen on all available network interfaces.

ii) client_addr = "0.0.0.0": Determines the IP address on which the Consul client API will be available. Setting it to 0.0.0.0 allows connections from any IP address.

iii) data_dir = "/var/consul": Specifies the directory where Consul will store its data, such as the state and logs.

iv) encrypt = "<YOUR_ENCRYPTED_KEY>": Sets the encryption key for securing communication between Consul servers and clients. Replace this placeholder with your actual generated encryption key.

v) datacenter = "dc1": Defines the datacenter name that this Consul server will use. Consul uses datacenters to organize services and nodes.

v) ui = true: Enables the Consul Web UI. This provides a graphical interface for interacting with Consul's data.

vi) server = true: Indicates that this instance is a Consul server. Server nodes participate in the consensus protocol and store the state of the system.

vii) log_level = "INFO": Sets the verbosity of the logs. INFO level provides a balance of details, logging general information, warnings, and errors.


- Next step is I ran the following command to start the Consul server in the background: 
***sudo nohup consul agent -dev -config-dir /etc/consul.d/ &**   and also check the status of the Consul server with the command ***consul members***.

![alt text](<img/3g start consul server in background_check status.PNG>)

Note : I used the -dev flag to indicate that we are running a single Consul server in development mode.

I visited my consul server public IP "54.162.129.70:8500", to access the Consul dashboard as shown below

![alt text](<img/3h Consul dashboard with IP.PNG>)



### TASK-3 : SETUP THE TWO BACKEND SERVERS

Since my Consul server is up and running, my Nginx backend servers more easily using service discovery. It involves installing] Nginx and the Consul agent on all the backend servers. The Consul agent acts like a messenger, automatically registering both the server and the Nginx service running on it with the Consul server, which acts like a central directory.

The configurations below will be applied on both backend servers:

- I SSH into the backend servers and ran command ***sudo apt-get update -y** to update package information on both servers.

![alt text](<img/4a SSH to BE servers-update package.PNG>)

Note : The y flag automatically answers "yes" to any prompts during the installation process, ensuring that the installation proceeds without requiring manual confirmation.

- I then Install Nginx on both instances by running the following command: 
***sudo apt install nginx -y***

![alt text](<img/4b install nginX.PNG>)


Note : after installing Nginx, I will need to navigate to the default HTML directory and modify the index.html file on both servers to differentiate them.

- I executed the command ***cd /var/www/html*** to navigate to the HTML directory and proceeded to open the HTML file with text editor  - ***sudo vi index.html***.

![alt text](<img/4c html directory_sudo.PNG>)

- Next is I copied the HTML content below into the index.html file of the two servers. 
then On the second server,I replaced SERVER-01 with SERVER-02 in the HTML file to differentiate between the two backend servers.

<!DOCTYPE html>
<html>
<head>
	<title>Kanekis Backend Server </title>
</head>
<body>
	<h1>This is Backend SERVER-01</h1>
</body>
</html>

![alt text](<img/4d modify index.html for BE.PNG>)

- I Installed Consul as an agent on the servers by running the commands:

***wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg***

***echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list***

***sudo apt update && sudo apt install consul***


- I then verified that Consul is installed properly by running the command: ***consul --version***

![alt text](<img/4f verify installed consul.PNG>)


I replace the default Consul configuration file "config.hcl" located in "/etc/consul.d" with my custom consul.hcl file by executing the commands 

***sudo mv /etc/consul.d/consul.hcl /etc/consul.d/consul.hcl.back***

***sudo vi /etc/consul.d/consul.hcl***

![alt text](<img/4g1 rename default config file_create new one_edit config.PNG>)


- The I added the contents below to the file and replaced <YOUR_ENCRYPTED_KEY> with my previous encryption key. I also inckuded my Consul server's IP address instead of 34.201.77.72.

"server" = false
"datacenter" = "dc1"
"data_dir" = "/var/consul"
"encrypt" = "<YOUR_ENCRYPTED_KEY>"
"log_level" = "INFO"
"enable_script_checks" = true
"enable_syslog" = true
"leave_on_terminate" = true
"start_join" = ["34.201.77.72"]

![alt text](<img/4g2 new edit config setting.PNG>)

Below is the explanation of the Consul agent configuration settings:

i) server = false: Indicates that this node is not a Consul server, but a client (agent). A Consul server handles requests from other Consul agents, while a client node registers services and performs checks.

ii) datacenter = "dc1": Specifies the datacenter name where the Consul agent operates. This should match the datacenter configuration on the Consul server to ensure proper communication.

iii) data_dir = "/var/consul": Defines the directory where the Consul agent will store its data files. This directory must be writable by the Consul agent process.

iv) encrypt = "<YOUR_ENCRYPTED_KEY>": Provides the encryption key for securing communication between Consul agents and the Consul server. Replace <YOUR_ENCRYPTED_KEY> with the actual key generated using consul keygen.

v) log_level = "INFO": Sets the verbosity of the log output. INFO level provides a balance between detail and readability, showing general information about Consul operations.

vi) enable_script_checks = true: Enables the execution of script-based health checks. When set to true, the Consul agent can run custom scripts to check service health.

vii) enable_syslog = true: Allows logging of Consul messages to the syslog service. When enabled, logs will be sent to the system's logging facility, which can be useful for centralized logging and monitoring.

viii) leave_on_terminate = true: Ensures that the Consul agent will automatically deregister itself from the Consul server when the agent process is terminated. This helps maintain accurate service registration and avoids stale entries.

viiii) start_join = ["34.201.77.72"]: Lists the addresses of Consul servers or agents that this Consul client should contact when starting up to join the Consul cluster. Replace 34.201.77.72 with the IP address of your Consul server. This setting helps the agent locate and connect to the Consul server to begin registering services.


- Next, I created a backend.hcl configuration file in the /etc/consul.d directory to register the Nginx service and its health check URLs with the Consul server. This will enable the Consul server to continuously monitor the health of the Nginx service. 

I Used the following command to create and edit the file: 

***sudo vi /etc/consul.d/backend.hcl***.

![alt text](<img/4h1 create BE config in consul dir_to register nginX in the consul server.PNG>)

I added the following contents to the backend.hcl file and save it.

"service" = {
  "Name" = "backend"
  "Port" = 80
  "check" = {
    "args" = ["curl", "localhost"]
    "interval" = "3s"
  }
}

![alt text](<img/4h2 create BE config in consul dir_to register nginX in the consul server.PNG>)

This configuration registers your backend servers with the Consul server and sets up a health check that uses curl to test the service every 3 seconds.

- I verified my configurations by executing the command ***consul validate /etc/consul.d*** and started the Consul agent with the command ***sudo nohup consul agent -config-dir /etc/consul.d/ &***

![alt text](<img/4i verify config and start consul agent.PNG>)


- Inorder to verify that everything is working correctly, I visited my Consul UI and saw that the backend is already listed there meaning that the backend has successfully registered itself with Consul as shown below :

![alt text](<img/4j Consul ip refreshed_shows BE servers registered.PNG>)

![alt text](<img/4j2 BE servers registered on consul.PNG>)

![alt text](<img/4k BE1 details.PNG>)

![alt text](<img/4k BE2 details.PNG>)



### TASK-4 : SETUP LOAD BALANCER

Next, I set up the load balancer to automatically update its backend server information based on the service registry maintained by Consul. To retrieve the backend server details, I will use the consul-template binary. This tool interacts with the Consul server via API calls to fetch the backend server information. It then uses a template to substitute values and generate the loadbalancer.conf file, which is utilized by Nginx.

First , I SSH to my load balance instancer/server , 

![alt text](<img/5a SSH to LB.PNG>)

I proceeded to update the package information and installed unzip with the following commands:

***sudo apt-get update -y***

***sudo apt-get install unzip -y***


![alt text](<img/5b sudo apt undate_install unzip.PNG>)


- I Installed Nginx using the following command

***sudo apt install nginx -y***

![alt text](<img/5c install nginX.PNG>)

I downloaded the consul-template binary using the following command

***sudo curl -L  https://releases.hashicorp.com/consul-template/0.30.0/consul-template_0.30.0_linux_amd64.zip -o /opt/consul-template.zip***

***sudo unzip /opt/consul-template.zip -d  /usr/local/bin/***

![alt text](<img/5d download consul template binary.PNG>)


- I verified the installation of consul-template and checked its version with the following command

***consul-template --version***

- Next I created and edit a file named load-balancer.conf.ctmpl in the /etc/nginx/conf.d directory, using the following command:
 ***sudo vi /etc/nginx/conf.d/load-balancer.conf.ctmpl***

![alt text](<img/5e1 edit LB config setting_consul setting.PNG>)

I Pasted the following content into the file:

upstream backend {

 {{- range service "backend" }} 

  server {{ .Address }}:{{ .Port }}; 

 {{- end }} 

}

server {

   listen 80;

   location / {
      proxy_pass http://backend;

    }
   
}

![alt text](<img/5e2 edit LB config setting.PNG>)

Breakdown of the configuration:

i) upstream backend: This defines a group of backend servers that Nginx can load-balance requests across.
ii) {{- range service "backend" }}: This is a Consul-Template directive that iterates over all services registered with Consul under the name "backend".
iii) server {{ .Address }}:{{ .Port }};: For each backend service, it adds an entry to the upstream block with the server's address and port.
iv) {{- end }}: Ends the iteration block.

v) server: Defines a virtual server that listens for incoming requests.

vii) listen 80: Specifies that this server block will listen on port 80 (the default HTTP port).

viii) location /: Defines a location block for all requests to the root URL (/).

viiii) proxy_pass http://backend: Forwards incoming requests to the upstream group named "backend" defined above. 
Nginx will use the addresses and ports listed in the upstream backend block to balance the requests.

#### Note : This setup keeps Nginx's backend server list in sync with Consul's. It ensures that Nginx always routes traffic to the currently available backend servers.


- Next , I Created a file named consul-template.hcl in the /etc/nginx/conf.d/ directory. This file is used by consul-template to specify details about the Consul server IP and the destination path where the processed load-balancer.conf file will be saved.

I used the following command to create and edit the file: 

***sudo vi /etc/nginx/conf.d/consul-template.hcl***

![alt text](<img/5e1 edit LB config setting_consul setting.PNG>)

I added the below content to the file, and replaced <Consul Server IP> with my Consul server's IP address. This configuration specifies the Consul server details, the path to the template file, the destination for the rendered Nginx configuration, and the command to reload Nginx after updating the configuration.

consul {

 address = "< Consul Server IP>:8500"

 retry {

   enabled  = true

   attempts = 12

   backoff  = "250ms"


 }

}

template {

 source      = "/etc/nginx/conf.d/

 load-balancer.conf.ctmpl"
 
  destination = "/etc/nginx/conf.d/load-balancer.conf"

 perms       = 0600

 command = "service nginx reload"

}

![alt text](<img/5e3 LB consul setting update.PNG>)

- I deleted the default server configuration to disable it (so as to avoid inconsistencies with the server's settings).
I did that by running the following command: 

***sudo rm /etc/nginx/sites-enabled/default***.

- I restarted Nginx to apply the changes by running the following command: 

***sudo systemctl restart nginx***.

- After completing the configurations, I restarted the Consul Template agent using the following command. 

***sudo nohup consul-template -config=/etc/nginx/conf.d/consul-template.hcl &***

![alt text](<img/5f remove default server config_restart nginx_start consul agent.PNG>)


### Upon completion, a load-balancer.conf file will be created with backend server information populated from the Consul service registry.

To view the load balancer.conf file created with backend information I used the ***sudo cat /etc/nginx/conf.d/load-balancer.conf** command as shown below

![alt text](<img/5g sudo Cat to view the LB file created.PNG>)


- I accessed the load balancer IP in my web browser, it displayed the custom HTML content from one of the backend servers. When I you refresh the page, the load balancer will route my request to the other backend server, displaying its custom HTML content.

![alt text](<img/5h2 BE1 viewed fron LB browser.PNG>)

![alt text](<img/5h1 BE2 viewed fron LB browser.PNG>)


This behavior occurs because the load balancer uses a round-robin algorithm by default, distributing incoming requests evenly across all available backend servers.

### TASK-5 : SERVICE DISCOVERY TEST

Now that everything is set up and running, I can test the configuration by observing what happens when I stop one of your backend servers.

I stopped one of the backend servers. The Consul server monitors the health of each registered service. Once a backend server is stopped, Consul will detect the server's unavailability and mark it as unhealthy. The health check for that server will fail, and it will be removed from the load balancer's active pool of servers.

![alt text](<img/6a Services discovery test_BE1 stopped.PNG>)


![alt text](<img/6b Services direscted to 2nd BE2.PNG>)

As a result, the load balancer will only direct traffic to the remaining healthy backend servers. This ensures that your application continues to run smoothly without any disruption to users, demonstrating the effectiveness of your service discovery and health check configuration with Consul and Nginx.


- **Service Check**: These checks are specific to the services running on the nodes (in this case, Nginx). When you stopped Nginx, the service check that monitors the health of Nginx on that particular node would fail, leading to the "all service checks failed" error.

-**Node checks**: These checks monitor the overall health of the node itself, which includes the underlying operating system and possibly other metrics (like CPU, memory, and disk usage). Since stopping Nginx does not necessarily mean the node is unhealthy (the node could still be up and running, responding to pings, etc.), the node checks would still pass.

The End Of Project 5