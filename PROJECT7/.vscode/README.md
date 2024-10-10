# PROJECT 7
## DESIGN OF A VIRTUAL PRIVATE CLOUD (VPC) INFRASTRUCTURE

### PROJECT OVERVIEW

A VPC is a virtual network that can contain EC2 instances as well as network resources for other AWS services. By default, every VPC is isolated from all other networks. A VPC can exist only within an AWS region. When you create a VPC in a region, it won’t show up in any other regions.

You control various aspects of your Amazon VPC, including selecting your own IP address range; creating your own subnets; and configuring your own route tables, network gateways, and security settings.

Note: You can have 100s of VPCs per Region for your needs even though the default quota is 5 VPCs per Region.

****This project**** aims to design and implement a secure, scalable, and highly available VPC infrastructure on Amazon Web Services (AWS) for a specified region (us-west-2) with multiple Availability Zones (us-west-2a, us-west-2b, us-west-2c). The VPC will have 15 subnets, one per Availability Zone, and will utilize an Internet Gateway, NAT Gateway, route tables, and Network ACLs for secure communication.




- ### Project Objectives:

1. Design a VPC architecture that meets business requirements.
2. Configure subnets, route tables, and Network ACLs for secure communication.
3. Implement Internet Gateway and NAT Gateway for internet access.


##### Typical VPC Architectures

![alt text](<img/VPC pix 3.PNG>)

![alt text](<img/VPC pix1.PNG>)

![alt text](<img/VPC pix3.JPG>)


- ### Project Components

 key components and features of an AWS VPC:


#### 1) Internet Gateway:

An Internet Gateway (IGW) is a logical connection between an Amazon VPC and the Internet.   
It is not a physical device and Only one can be associated with each VPC.   
When we create IGW, it will not be attached to the VPC automatically, we need to explicitly attach it to the VPC.
If a VPC does not have an Internet Gateway, then the resources in the VPC cannot be accessed from the Internet.   
It does not limit the bandwidth of Internet connectivity. (The only limitation on bandwidth is the size of the Amazon EC2 instance, and it applies to all traffic — internal to the VPC and out to the Internet.)

#### 2) AWS Router: 
Each AWS VPC has a VPC router.   
The primary function of this VPC router is to take all of the route tables defined within that VPC and then direct the traffic flow within that VPC, as well as to subnets outside of the VPC, based on the rules defined within those tables.

#### - Route Table and how does it work?

A set of rules, called routes, are used to determine where network traffic is directed.   
A route table is used to determine where network traffic from your subnet or gateway is directed. In simple terms, the route table tells network packets which way they need to go to get to their destination.   
A subnet can only be associated with one route table at a time, but you can associate multiple subnets with the same subnet route table.   
Every route table contains a local route for communication within the VPC. This route is added by default to all route tables.


Main route table :  
The first entry is the default entry for local routing in the VPC.   
This entry enables the instances in the VPC to communicate with each other over IPv4.
The second entry routes all other IPv4 subnet traffic from the private subnet to your network over the virtual Nat gateway (for example, nat-1a2b3c4d)
The Main route table automatically comes with your VPC that is used by default for any subnet that isn’t explicitly associated with a routing table.

Custom Route Table:   
Custom route table that you create for your VPC.   
The first entry is the default entry for local routing in the VPC.   This entry enables the instances in the VPC to communicate with each other.   
The second entry routes all other IPv4 subnet traffic from the public subnet to the internet over the internet gateway (for example, igw-1a2b3c4d
The Destination is the pattern for where the packet is trying to end up and the Target is where the packet should go. For example, packets with a destination within 10.0.0.0/16 should be routed directly inside the VPC.   
In the above VPC architecture, we have defined public and private route tables so let’s understand it.

Public Route Table — Routing to Internet Gateway: The route table which is directly associated with the Internet gateway is called the Public Route table. To make the route table public, we need to add routes destination 0.0.0.0 and Target the Internet Gateway id.


Private Route Table — Routing to Nat Gateway: The route table which is directly associated with the Nat gateway is called the Private Route table. To make the route table private, we need to add routes destination 0.0.0.0 and Target the Nat Gateway id.


#### 4) NAT Gateway:

NAT device enables instances in a private subnet to connect to the Internet or other AWS services but prevents the Internet from initiating connections with the instances.   
NAT gateway always resides inside the public subnet of an Availability Zone.   
Elastic IP must be attached to the NAT gateway while creating.   
NAT devices do not support IPv6 traffic, use an egress-only Internet gateway instead.

#### 5) Subnet:

A subnet is a range of IP addresses in your VPC and it is a logical subdivision of the VPC network.   
The practice of dividing a network into two or more networks is called subnetting.   
AWS provides two types of subnetting one is Public which allows the internet to access the machine and another is private which is hidden from the internet.   
A subnet is a span to a single availability zone.

Public Subnet:   
A public subnet is a subnet that’s associated with a route table (public route table) that has a route to an internet gateway.   
Resources that reside within the public subnet can access the Internet with an Internet gateway.

Private Subnet:   
A public subnet is a subnet that’s associated with a route table (private route table) that has a route to a NAT gateway.   
Resources that reside within the private subnet can access the Internet with a NAT gateway.   
The resources from private subnet such as Mysql DB, private VM can’t be accessed directly from the internet.   
We can use VPN as a service from AWS or can a bastion host in the public subnet to connect the resources in the private subnet.

#### 6) VPC Security
Security within a VPC is provided throughSecurity groups :   
Acts at an Instance level firewall but not at the subnet level.   
Each instance within a subnet can be assigned a different set of Security groups   
Default Security Group allows all outbound traffic   
In the Security group, we can specify only Allow rules, but not deny rules.   
Security Groups are Stateful — If a request is allowed, the response for the request is automatically allowed.   

#### 7) Network access control lists (ACLs):

It is a security layer for your VPC which is applied on the subnet level.   
stateless firewall — you need to allow both inbound and outbound traffic.   
It is an optional layer for your VPC and can set up a Network ACL similar to the security group that adds an additional layer of security to your VPC.
When you create VPC, it creates default Network ACL automatically which includes all inbound and outbound ipv4 traffic.   
You can also create a custom network ACL and associates it with a subnet. By default, a custom Network ACL denies all the inbound and outbound ipv4 traffic until you add rules.   
Act as a firewall for associated subnets, controlling both inbound and outbound traffic at the subnet level   

VPC Flow Logs is a feature that enables you to capture information about the IP traffic going to and from network interfaces in the VPC and can help in monitoring the traffic or troubleshooting any connectivity issues.   
Flow log data is stored using Amazon CloudWatch Logs.   
Flow log can be created for the entire VPC, subnets, or each network interface. If enabled, for the entire VPC or subnet all the network interfaces are monitored
Flow logs do not capture real-time log streams for network interfaces.


#### 8) IP Addresses
Instances launched in the VPC can have Private, Public, and Elastic IP address assigned to them.


- Private IP Addresses:

Private IP addresses are not reachable over the Internet and can be used for communication only between the instances within the VPC
All instances are assigned a private IP address, within the IP address range of the subnet, to the default network interface   
The private IP address is associated with the network interface for its lifetime, even when the instance is stopped and restarted and is released only when the instance is terminated   
Additional Private IP addresses, known as secondary private IP address, can be assigned to the instances and these can be reassigned from one network interface to another

- Public IP address:

Public IP addresses are reachable over the Internet and can be used for communication between instances and the Internet.   
The public IP address assigned to the Instance depends if the Public IP Addressing is enabled for the Subnet.   
The public IP address can also be assigned to the Instance by enabling the Public IP addressing during the creation of the instance.   
The public IP address is assigned from the AWS pool of IP addresses and it is not associated with the AWS account and hence is released when the instance is stopped and restarted or terminated.  

- Elastic IP address:

Elastic IP addresses are static, persistent public IP addresses that can be associated and disassociated with the instance, as required.
The elastic IP address is allocated at a VPC and owned by the account unless released.
A Network Interface can be assigned either a Public IP or an Elastic IP. If you assign an instance, already having a Public IP, an Elastic IP, the public IP is released.
Elastic IP addresses can be moved from one instance to another, which can be within the same or different VPC within the same account
Elastic IP is charged for non-usage i.e. if it is not associated or associated with a stopped instance or an unattached Network Interface


#### 9) VPC Peering , VPN Gateway , VPC Endpoints , AWS Direct Connect
VPC Peering : Allows you to connect one VPC with another VPC to route traffic between them using private IP addresses.

VPN Gateway: Enables you to establish a secure connection between your VPC and your on-premises network.

VPC Endpoints: Allow you to privately connect your VPC to supported AWS services and VPC endpoint services powered by AWS PrivateLink without requiring an Internet Gateway, NAT device, VPN connection, or AWS Direct Connect connection.

AWS Direct Connect: Provides a dedicated network connection from your premises to AWS, enhancing security and reducing network costs.

Example of Real-Life Applications:

1. E-commerce Website Hosting: Host an e-commerce website securely, ensuring customer data protection.
2. Financial Institution's Data Warehouse: Create a secure data warehouse for financial data, ensuring compliance.
3. Healthcare Organization's Research Environment: Establish a secure research environment for sensitive patient data.

-----
## Project Design Requirements:


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

This project task involves setting up an AWS VPC with 15 subnets, one per Availability Zone, Internet Gateway, routing table, NAT gateway, and Network ACL.


#### TASK 1: Create VPC
Create a new VPC with the specified CIDR block. It Defines the virtual network boundaries.   

Steps:
1. Log in to the AWS Management Console.
2. Navigate to the VPC dashboard.
3. Click "Create VPC".
4. Enter:
    - IPv4 CIDR block: 10.0.0.0/24
    - VPC name: 
5. Click "Create VPC".

#### TASK 2: Create Internet Gateway (IGW)

Create an Internet Gateway to enable internet access. This provides internet connectivity to the VPC.

Steps:

1. Navigate to the Internet Gateways dashboard.
2. Click "Create internet gateway".
3. Enter a name for the IGW.
4. Click "Create internet gateway".



#### TASK 3: Create Subnets

Create 15 subnets, one per Availability Zone. i.e Divide the VPC into smaller, isolated networks.

Steps:

1. Navigate to the Subnets dashboard.
2. Click "Create subnet".
3. Enter details for each subnet:


#### TASK 4: Create Routing Table

Create a routing table for public subnets.
This is required to define routing rules for the VPC.

Steps:

1. Navigate to the Route Tables dashboard.
2. Click "Create route table".
3. Enter a name for the route table.
4. Add routes:



#### TASK 5: Attach IGW to VPC

Attach the IGW to the VPC. This enables internet access for the VPC.

Steps:

1. Navigate to the VPC dashboard.
2. Select the VPC.
3. Actions > Attach internet gateway.
4. Select the IGW created in Step 3.



#### TASK 6: Create NAT Gateway

Create a NAT gateway for private subnets.
This enables outbound internet access for private subnets.

Steps:

1. Navigate to the VPC dashboard.
2. Click "Create NAT gateway".
3. Select the subnet for the NAT gateway.
4. Click "Create NAT gateway".


#### TASK 7: Configure Network ACL

Configure Network ACL for subnets to control inbound and outbound traffic.

Steps:

1. Navigate to the Network ACLs dashboard.
2. Click "Create network ACL".
3. Enter a name for the network ACL.
4. Configure inbound and outbound rules.
     


---
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

### TASK 5: CREATE NAT GATEWAY

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