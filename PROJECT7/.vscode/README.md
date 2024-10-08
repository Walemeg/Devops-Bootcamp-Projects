# PROJECT 7
## DESIGN OF A VIRTUAL PRIVATE CLOUD (VPC) INFRASTRUCTURE

### PROJECT OVERVIEW

A VPC (Virtual Private Cloud) in AWS (Amazon Web Services) is a service that allows you to launch AWS resources in a logically isolated virtual network that you define.

This project aims to design and implement a secure, scalable, and highly available VPC infrastructure on Amazon Web Services (AWS) for a specified region (us-west-2) with multiple Availability Zones (us-west-2a, us-west-2b, us-west-2c). The VPC will have 15 subnets, one per Availability Zone, and will utilize an Internet Gateway, NAT Gateway, route tables, and Network ACLs for secure communication.




- ### Project Objectives:

1. Design a VPC architecture that meets business requirements.
2. Configure subnets, route tables, and Network ACLs for secure communication.
3. Implement Internet Gateway and NAT Gateway for internet access.


- ### Project Components

 Here are the key components and features of an AWS VPC:

1) Subnets: A VPC can be divided into multiple subnets, which are sections of the VPC IP address range where you can place groups of isolated resources. Subnets can be either public (with access to the internet) or private (isolated from the internet).

2) IP Addressing: You can choose your own IP address range for the VPC using IPv4 or IPv6 addresses.

3) Route Tables: Control the routing of network traffic within the VPC and between subnets. You can create custom route tables to direct traffic as needed.

4) Internet Gateway: A VPC component that allows communication between instances in your VPC and the internet. You need to attach an Internet Gateway to a VPC to enable internet access for your instances.

5) NAT Gateway and NAT Instances: These allow instances in a private subnet to connect to the internet or other AWS services without exposing themselves to incoming traffic from the internet.

6) Security Groups: Act as virtual firewalls for your instances to control inbound and outbound traffic at the instance level.

7) Network ACLs (Access Control Lists): Provide an additional layer of security, controlling traffic at the subnet level.

8) VPC Peering: Allows you to connect one VPC with another VPC to route traffic between them using private IP addresses.

9) VPN Gateway: Enables you to establish a secure connection between your VPC and your on-premises network.

10) VPC Endpoints: Allow you to privately connect your VPC to supported AWS services and VPC endpoint services powered by AWS PrivateLink without requiring an Internet Gateway, NAT device, VPN connection, or AWS Direct Connect connection.

11) AWS Direct Connect: Provides a dedicated network connection from your premises to AWS, enhancing security and reducing network costs.

Example of Real-Life Applications:

1. E-commerce Website Hosting: Host an e-commerce website securely, ensuring customer data protection.
2. Financial Institution's Data Warehouse: Create a secure data warehouse for financial data, ensuring compliance.
3. Healthcare Organization's Research Environment: Establish a secure research environment for sensitive patient data.


### Project Design Requirements:

1. CIDR Block: 10.0.0.0/16.
2. Region: us-west-2.
3. Availability Zones: us-west-2a, us-west-2b, us-west-2c.
4. Subnets: 15 (one per Availability Zone).


### Technical Considerations:

1. VPC size and CIDR block allocation.
2. Subnet sizing and allocation.
3. Route table configuration.
4. Network ACL configuration.
5. Security group configuration.



### Project Work Flow

The workflow for setting up a Virtual Private Cloud (VPC) begins with creating a VPC, which defines the virtual network foundation. Next, the VPC is divided into smaller, manageable networks through subnet creation. To enable internet access, an Internet Gateway is created and then attached to the VPC.

The attachment connects the VPC to the internet, allowing for incoming and outgoing traffic. Route Tables are then created to define traffic routing rules within the VPC. A NAT Gateway is set up to allow outbound internet access from private subnets.

Network Access Control Lists (ACLs) are configured to control inbound and outbound traffic at the subnet level. With the network infrastructure in place, EC2 instances are launched within the VPC.

Security Groups are then configured to define instance-level security rules, controlling inbound and outbound traffic.


### PROJECT TASK

This README guides you through setting up an AWS VPC with 15 subnets, one per Availability Zone, Internet Gateway, routing table, NAT gateway, and Network ACL.


TASK 1: Create VPC

Create a new VPC with the specified CIDR block. It Defines the virtual network boundaries.

Steps:

1. Log in to the AWS Management Console.
2. Navigate to the VPC dashboard.
3. Click "Create VPC".
4. Enter:
    - IPv4 CIDR block: 10.0.0.0/24
    - VPC name: 
5. Click "Create VPC".

TASK 2: Create Internet Gateway (IGW)

Create an Internet Gateway to enable internet access. This provides internet connectivity to the VPC.

Steps:

1. Navigate to the Internet Gateways dashboard.
2. Click "Create internet gateway".
3. Enter a name for the IGW.
4. Click "Create internet gateway".



TASK 3: Create Subnets

Create 15 subnets, one per Availability Zone. i.e Divide the VPC into smaller, isolated networks.

Steps:

1. Navigate to the Subnets dashboard.
2. Click "Create subnet".
3. Enter details for each subnet:


| Subnet Name | Availability Zone | CIDR Block | Type |
| --- | --- | --- | --- |
| Prod-Web-Public-2a | us-west-2a | 10.0.0.0/28 | Public |
| ... | ... | ... | ... |


TASK 4: Create Routing Table

Create a routing table for public subnets.
This is required to define routing rules for the VPC.

Steps:

1. Navigate to the Route Tables dashboard.
2. Click "Create route table".
3. Enter a name for the route table.
4. Add routes:




TASK 5: Attach IGW to VPC

Attach the IGW to the VPC. This enables internet access for the VPC.

Steps:

1. Navigate to the VPC dashboard.
2. Select the VPC.
3. Actions > Attach internet gateway.
4. Select the IGW created in Step 3.




TASK 6: Create NAT Gateway

Create a NAT gateway for private subnets.
This enables outbound internet access for private subnets.

Steps:

1. Navigate to the VPC dashboard.
2. Click "Create NAT gateway".
3. Select the subnet for the NAT gateway.
4. Click "Create NAT gateway".


TASK 7: Configure Network ACL

Configure Network ACL for subnets to control inbound and outbound traffic.

Steps:

1. Navigate to the Network ACLs dashboard.
2. Click "Create network ACL".
3. Enter a name for the network ACL.
4. Configure inbound and outbound rules.




### DCUMENTATION

-------------------------------------------------------

### TASK-1 : CREATE A VIRTUAL PRIVATE CLOUD 

-----------------------------------------------------

To create a new VPC with the specified CIDR block, I Logged in to the AWS Management Console and navigated Navigate to the VPC dashboard and Clicked "Create VPC".


![alt text](<img/1a Select & Create VPC.PNG>),

I entered the VPC name and updated the IPv4 CIDR block as 10.0.0.0/24 and Clicked "Create VPC".

![alt text](<img/1b Update setting & Create VPC.PNG>)

![alt text](<img/1c VPC details.PNG>)

----------------------------------------------

### TASK 2: CREATE INTERNET GATEWAY(IGW)

---------------------------------------------------


Internet Gateway was created to enable internet access. This provides internet connectivity to the VPC.

Steps followed:

I navigate to the Internet Gateways dashboard and Clicked "Create internet gateway".


![alt text](<img/2a Select Internet gateway.PNG>)

![alt text](<img/2b Name & create internet gateway.PNG>)

I entered a name for the IGW and Click "Create internet gateway".

![alt text](<img/2c Click attach to a VPC.PNG>)

![alt text](<img/2d select from available VPC and attach the IGW.PNG>)

![alt text](<img/2e result of attached IGW.PNG>)


---------------------------------------------------

### TASK 3: CREATE SUBNETS

-------------------------------------------------

I Createed 15 subnets, one per Availability Zone.

I navigated to the Subnets dashboard and clicked  "Create subnet".
I selected the VPC Im working with and created the subnet

![alt text](<img/3a creating subnet.PNG>)

![alt text](<img/3b select VPC to use.PNG>)

I proceeded to create public subnet on the various availability using the information below

![alt text](<img/public subnet.PNG>)


![alt text](<img/3c select 1st subnet and click create another.PNG>)

![alt text](<img/3d 2nd web subnet.PNG>)

![alt text](<img/3e 3rd web subnet.PNG>)

I followed the steps above and created subnet for application , database , management and platform using the information below. (though had to create the subnets for the various availability zone one by one to avoid CIDR overlap error)

![alt text](<img/subnet fpr APP_DB_Managemnt_platform.PNG>)

Below is my final result for all subnets created

![alt text](<img/3g All subnet created.PNG>)


-------------------------------------------------

### TASK 4: CREATING ROUTING TABLE

-----------------------------------------------------

I created a routing table for all the subnets (public , application , database , management and platform).
This is required to define routing rules for the VPC.

I navigate to the Route Tables dashboard.Clicked "Create route table" 

![alt text](<img/4a Routing table.PNG>)

I entered a name for the route table , selected the VPC and clicked "create Route tabel"

![alt text](<img/4b Routing table attached to VPC.PNG>)

I clicked on the created route table , slected the subnet associateion and clicked on edit subnet association. I selected the availability zones linked to each subnet and saved association

![alt text](<img/4c edit subnet association.PNG>)

![alt text](<img/4d three prod_web_public subnet associated to  routing table.PNG>)

I repeated same process for each subnet as shown below

APP ROUTING TABLE
![alt text](<img/4e App Routing table attached to VPC.PNG>)

![alt text](<img/4f three App subnet associated to App routing table.PNG>)


DB ROUTING TABLE
![alt text](<img/4g three DB subnet associated to DB routing table.PNG>)


MANAGEMENT ROUTING TABLE
![alt text](<img/4h three management subnet associated to management routing table.PNG>)


PLATFORM ROUTING TABLE
![alt text](<img/4i three platform subnet associated to platform routing table.PNG>)


ALL THE ROUTING TABLE CREATED

![alt text](<img/4j All RT associated to VPC & Subnet.PNG>)

----------------------------------------

### TASK 5: CREATE NAT GATEWAY AND INTERNET GATEWAY

------------------------------------------

Create a NAT gateway for private subnets.
This enables outbound internet access for private subnets.

I started by navigating to the NAT gateway dashboard and Clicked "Create NAT gateway".

![alt text](<img/5a start NAT gateway.PNG>)

I put teh NAT-GW name and selected the subnet for the NAT gateway , allocated the Elastic IP and Clicked "Create NAT gateway".

![alt text](<img/5b Create NAT-gateway.PNG>)

I went back to routing table, selected the already created route tables and added NAT gateway to them one by one.
I started with Platform routing table; selected it and selected route and edit route.

![alt text](<img/5c add the NAT gateway to Platform route tables.PNG>)

I clicked on add route (under destination), and selected NAT-GW as target and specified my NAT-GW-VPC and saved changes.

![alt text](<img/5d edit & add destination routes to platform RT_select NAT gateway_and save changes.PNG>)

Going back to the platform routing table, I can see the created NAT-GW route active as shown below

![alt text](<img/5e NAT gateway attached to patform RT.PNG>)

#### Note : I repeated the same process for crteating NAT gateway for other routing table as shown below:


NAT GATEWAY FOR MANAGEMENT ROUTING TABLE

![alt text](<img/5F add the NAT gateway to Management route tables.PNG>)

![alt text](<img/5g edit & add destination routes to management RT_select NAT gateway_and save changes.PNG>)


![alt text](<img/5h NAT gateway attached to management RT.PNG>)


NAT GATEWAY CREATED FOR DATABASE ROUTING TABLE

![alt text](<img/5i NAT gateway attached to DB-RT.PNG>)


NAT GATEWAY CREATED FOR APPLICATIONS ROUTING TABLE

![alt text](<img/5J NAT gateway attached to APP-RT.PNG>)

INTERNET GATEWAY CREATED FOR PUBLIC ROUTING TABLE

Internet gateway is being used for public routing table because we want to route traffic using internet gateway service.

The same process was followed like other subnets but NAT gateway was rfeplaced wikth Internet gateway for public subnet

![alt text](<img/5K Internet gateway settings for Public-RT.PNG>)

![alt text](<img/5K NAT gateway attached to Public-RT.PNG>)

![alt text](<img/5L Internet gateway attached to Public-RT.PNG>)

------------------------------------------

### TASK 6: CONFIGURE NETWORK ACL
----------------------------------------

Configure Network ACL for subnets to control inbound and outbound traffic.
Network access control list (NACL) is the native VPC functionality to control the inbound and outbound traffic at the subnet level.

In our architecture, the connection to the DB subnet should be allowed only from the App subnet and management subnet. The public subnet should not have direct access to the DB subnet.

The following are the tables for inbound and outbound rules for the DB NACL.

![alt text](<img/DB _ NACL _Inbound & Outbound Rule.PNG>)


I navigated to the Network ACLs dashboard and clicked "Create network ACL".

![alt text](<img/6a Start creating Network ACL.PNG>)

I entered a name for my network ACL and specified the VPC to use and clicked "create NACL".

![alt text](<img/6b link to VPC and create NACL.PNG>)

I proceeded with configuring the inbound and outbound rules.

I started with inbound rules and I selected edit NACL Inbound rules, updated the inbound rules in tables above and and save changes.
![alt text](<img/6c Select edit NACL Inbound rules.PNG>)

![alt text](<img/6d update NACL Inbound rules and save.PNG>)

Next I went to outbound rules and I selected "edit NACL outbound" rules, updated the inbound rules in tables above and and save changes.

![alt text](<img/6d update NACL outbound rules and save.PNG>)

![alt text](<img/6e Select edit NACL outbound rules.PNG>)

Below are the final result of the NACL Inbound and Outbound rules created

NACL INBOUND RULES
![alt text](<img/7a Inbound rules result.PNG>)

NACL OUTBOUND RULES
![alt text](<img/7b outbound rules result.PNG>)