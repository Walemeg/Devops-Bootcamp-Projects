# PROJECT TITLE : To Setup Multiple Static Website Using Nginx Virtual Host

The goal is to learn the concept of subdomains and hosting multiple websites on a single server using Nginx Virtual Host configuration.


### Below are the respective task for the Project 

|S/N | Project Tasks                                                                   |
|----|---------------------------------------------------------------------------------|
- Task 1: Spin up a Ubuntu server & assign an elastic IP to it.
- Task 2: SSH into the server and install and configure Nignx on a server.
- Task 3: Create two website directories with two different website templates.
- Task 4: Create two subdomains
- Task 5: Add the IP of the server as A record to the two subdomains.
- Task 6: Configure the Virtual host to point two subdomains to two different website directories.
- Task 7: Validate the setup by accessing the subdomains.
- Task 8: Create a certbot SSL certificate for the root Domain.
- Task 9: Configure certbot on Nginx for the two websites.
- Task 10: Validate the subdomain websites’ SSL using OpenSSL utility.      |


## Task-1 
### Spin up a Ubuntu server and assign an elastic IP to it

- I clicked on **EC2** within the AWS management console and  **Launch Instance**

- I **Named** my instance and selected the **Ubuntu** AMI.

- I clicked the **Create new key pair** button to generate a key pair for secure connection to my instance.
- I entered a **Key pair name** and clicked on **Create key pair**.

- I enabled **SSH**, **HTTP**, and **HTTPS** access, then proceeded to click **Launch instance**.

![alt text](<img/5 SSH HTTP.PNG>)


- I clicked on **View all instances** and **created instance**.

![alt text](<img/7 created instance.PNG>)


- Clicked on the **Connect** button.

![alt text](<img/8 Connect to instance.PNG>)

- Copied the command provided under **`SSH client`**.

![alt text](<img/9 SSH Client.PNG>)

- I opened a terminal and located the folder where my **`.pem`** file was downloaded, I used the CD command to move to the folder, and pressed Enter. I also pasted the copied the command provided under **`SSH client`**

![alt text](<img/10 Open terminal.PNG>)


### Create And Assign an Elastic IP

- Returned to my AWS console and clicked on the **menu icon** to open the dashboard menu. Selected **Elastic IPs** under **Network & Security**.

![alt text](<img/11-12 Elastic ip.PNG>)

- Clicked on the **Allocate Elastic IP address** button.

![alt text](<img/13 Allocate IP address.PNG>)

- I kept the settings unchanged and proceed to click **Allocate**.

![alt text](<img/14 allocate.PNG>)

- I **Associated this Elastic IP address** with my running instance.

![alt text](<img/15 Associate the public address with instance.PNG>)

- I select the instance I wish to associate with the elastic IP address, and then clicked on **Associate**.

![alt text](<img/16 select instance to be associated with elastic ip.PNG>)


## Task 2: 
### SSH into the server and install and configure Nignx on a server.

 I SSH it into my instance again and paste the **command** into my terminal and then press Enter. When prompted, I type **"yes"** and press Enter to connect.

![alt text](<img/17 ssh into new instance with elastic IP address.PNG>)

---

### Install Nginx and Setup Your Website

- I executed the following commands.

`sudo apt update`
`sudo apt upgrade`
`sudo apt install nginx`

- I used  **`sudo systemctl start nginx`** command for starting my Nginx server.  
- I used **`sudo systemctl enable nginx`** for enabling the server to start on boot.
- I used **`sudo systemctl status nginx`** command to confirm if it's running.

![alt text](<img/18 Nginx installed.PNG>)

- I went back to my EC2 dashboard and copied my **Public IPv4 address**.

![alt text](<img/19 copied IPv4 address.PNG>)

- I copied my instances **Public IPv4 address** in a web browser to view the default Nginx startup page.

![alt text](<img/20 instance Public IPv4 address in web browser.PNG>)

## Task 3, 4 & 5
### Create two website directories with two different website templates and two subdomains

- I downloaded two website template URL from tooplate.com; "waso_strategy" and "Wedding Lite"

- I Scrolled down to the download section, right-clicked to open the menu, and selected **Inspect** from the options.

- I Selected the **Network** tab.

- Clicked the **Download** button and did the needful.

I repeated the same thing for the 2nd website


Waso Strategy
![alt text](<img/21b Waso strategy website template download.PNG>)

Wedding Lite
![alt text](<img/21c wedding lite website template download.PNG>)

The two websites template details
![alt text](<img/21d notepad.PNG>)

---

- To install the unzip tool, I ran the command : sudo apt install unzip.

- To execute the command to download and unzip my website files I ran command for the two website templates
  
1. "sudo curl -o /var/www/html/2130_waso_strategy.zip https://www.tooplate.com/zip-templates/2130_waso_strategy.zip && sudo unzip -d /var/www/html/ /var/www/html/2130_waso_strategy.zip && sudo rm -f /var/www/html/2130_waso_strategy.zip" and  
2. sudo curl -o /var/www/html/2131_wedding_lite.zip https://www.tooplate.com/zip-templates/2131_wedding_lite.zip && sudo unzip -d /var/www/html/ /var/www/html/2131_wedding_lite.zip && sudo rm -f /var/www/html/2131_wedding_lite.zip


- Next, I created two sub-domain records in AWS for the two website template downloaded the websites accessible via domain name rather than the IP address.  
The IP of the server was also added as A record to the two subdomains
After creating the records, I confirmed that they both exist in the records list as shown below.


![alt text](<img/22 two domains created in AWS.PNG>)


## Task 6 & 7: 
### Configure the Virtual host to point two subdomains to two different website directories and validate the setup by accessing the subdomains.

- To set up my website's configuration, I started by creating a new file in the Nginx sites-available directory. 
- I used the command below to open a blank file in a text editor: *sudo nano /etc/nginx/sites-available/waso*

- I copied and pasted the following code below into the open text editor.
server {
    listen 80;
    server_name example.com www.example.com;

    root /var/www/html/example.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

- I edited the server name and root directive within your my block to point to the directory where my] downloaded website content is stored.
I repeated same process for the second website as shown below.

Waso_Strategy website configuration
![alt text](<img/24a 1st website configuration.PNG>)

Wedding_lite website configuration
![alt text](<img/24b 2nd website configuration.PNG>)


- I created symbolic link for both websites by running the following command. 
1. *sudo ln -s /etc/nginx/sites-available/cleaningwaso /etc/nginx/sites-enabled/*
2. *sudo ln -s /etc/nginx/sites-available/wedding /etc/nginx/sites-enabled/*

- I ran the *sudo nginx -t* command to check the syntax of the Nginx configuration file.

*Note : When I ran the test, it initially failed but after removing the unecessary files as indicated from teh error prompt, it eventually passed as shown in the picture below.

- I deleted the default files in the sites-available and sites-enabled directories by executing the following commands:
1. *sudo rm /etc/nginx/sites-available/default*
2. *sudo rm /etc/nginx/sites-enabled/default*

- I restarted the Nginx server by executing the command: *sudo systemctl restart nginx*.

![alt text](<img/25a creating symbolic link website.PNG>)

![alt text](<img/25b removing unwanted files in server _ re-testing.PNG>)

- I put my domain names in a web browser to verify that my website is accessible (though still not secure) as shown below.

waso_strategy unsecure website
![alt text](<img/26a waso unsecure website.PNG>)

wedding lite unsecure website
![alt text](<img/26b wedding unsecure website.PNG>)


##  Task 8 , 9 and 10 
### Create a certbot SSL certificate for the root Domain, Configure certbot on Nginx and validate the subdomain websites’ SSL using OpenSSL utility.
 
Next, I use certbot to obtain the SSL certificate necessary to enable HTTPS on my site.

- I install certbot and Request For an SSL/TLS Certificate by executing the following commands: 
1. *sudo apt update sudo apt install python3-certbot-nginx*
2. *sudo certbot --nginx*

- I executed the *sudo certbot --nginx* command to request my certificate and followed the instructions provided by certbot. I selected my domian names for which I would like to activate HTTPS.

![alt text](<img/27 Install python &request for certificate.PNG>)

- I Verified the the two websites' SSL using the OpenSSL utility with the command: *openssl s_client -connect cleaning.cloudghoul.online:443*

![alt text](<img/28a verify waso website SSL.PNG>)

![alt text](<img/28b verify wedding website SSL.PNG>)

#### I visted the below to view my secured website.

### https://waso.qserver.space 
![alt text](<img/29a Final secure website test.PNG>)


### https://wedding.qserver.space
![alt text](<img/29b Final secure website test.PNG>)

The End Of Project 2