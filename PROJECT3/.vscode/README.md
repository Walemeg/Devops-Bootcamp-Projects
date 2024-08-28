# PROJECT 3
# SETUP LOAD-BALANCING FOR STATIC WEBSITE USING NGINX


The goal of the project is to demonstrate Layer 7 load balancing and load balancing algorithms using Nginx as a Load Balancer.

It involves setting up a load balancer (reverse proxy) configuration and two other webserver using Nginx. 
The loadbanacer is expected to serve as an intermediary for requests from clients seeking resources from the two webservers behind it. The load balancer is expected to be capable of distributing traffic across the two other servers listed in the upstream block. 

The load balancing function will be observed when the domain name is inserted in the web browser and the web page refreshed.  




### 	PROJECT TASKS
1) Deploy three servers

2) Set up static websites on two servers using Nginx.

3) Use two separate HTML files with distinct content. Deploy one file to each server's index.html location.

4) Set up Nginx on the third server. It will act as a load balancer.

5) Configure Nginx to load and balance traffic between two static websites.

6) Add the Nginx Load balancer IP to the DNS A record.

7)	Try accessing the website. Every time you reload the website you should see a different index.html.




## TASK 1 
### DEPLOY THREE SERVERS

- I clicked on **EC2** within the AWS management console and  **Launch Instance**

- I **Named** my instance and selected the **Ubuntu** AMI. I alos indicated the number of instances as "3"

- I used an existing key pair "AWS1"

- I enabled **SSH**, **HTTP**, and **HTTPS** access, then proceeded to click **Launch instance**.

![alt text](<img/a1 3 instances created at once.PNG>)


- I clicked on **View all instances** and **renamed the three instances based on their function**.

![alt text](<img/a2  teh 3 instances renamed for it's functions.PNG>)

### Create And Assign an Elastic IP

- Returned to my AWS console and clicked on the **menu icon** to open the dashboard menu. Selected **Elastic IPs** under **Network & Security**.

![alt text](<img/11-12 Elastic ip.PNG>)

- Clicked on the **Allocate Elastic IP address** button.

![alt text](<img/13 Allocate IP address.PNG>)

- I kept the settings unchanged and proceed to click **Allocate**.

![alt text](<img/14 allocate.PNG>)

- I **Associated this Elastic IP address** with my running instance.

![alt text](<img/15 Associate the public address with instance.PNG>)

- I select each of the instance I wish to associate with each of the elastic IP address, and then clicked on **Associate**; one instanace at a time.

![alt text](<img/16 select instance to be associated with elastic ip.PNG>)


- All Instances associated to respective IP Addresses

![alt text](<img/all Elastic IP.PNG>)


- I clicked on the **Connect** button for each of the instances and Copied the command provided under **`SSH client`**. I copied it into my notepad for reference

![alt text](<img/9 SSH Client.PNG>)

- For each of the instances, I opened a terminal and located the folder where my **`.pem`** file was downloaded, I used the CD command to move to the folder, and pressed Enter. I pasted the copied command provided under **`SSH client`** for the three instances as shown below

![alt text](<img/a3 SSH Connect for the  servers.PNG>)


## TASK 2 and 3 
### SETTING UP STATIC WEBSITES ON TWO SERVERS USING NGINX.

### i) Creation of two website directories with two different website templates and two subdomains

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

### ii) Installation of Nginx

 - I executed the following commands to install Nginx on the two webservers

`sudo apt update`
`sudo apt upgrade`
`sudo apt install nginx`

- I used  **`sudo systemctl start nginx`** command for starting my Nginx server.  
- I used **`sudo systemctl enable nginx`** for enabling the server to start on boot.
- I used **`sudo systemctl status nginx`** command to confirm if it's running.

![alt text](<img/18 Nginx installed.PNG>)


- I copied my instances **Public IPv4 address** in a web browser to view the default Nginx startup page.


Waso strategy IPv4 address
![alt text](<img/20 instance Public IPv4 address in web browser.PNG>)


Wedding lite IPv4 address
![alt text](<img/a20 nginx with IPv4 ip.PNG>)
---

- To install the unzip tool, I ran the command : sudo apt install unzip.

- To execute the command to download and unzip my website files I ran command for the two website templates
  
1. "sudo curl -o /var/www/html/2130_waso_strategy.zip https://www.tooplate.com/zip-templates/2130_waso_strategy.zip && sudo unzip -d /var/www/html/ /var/www/html/2130_waso_strategy.zip && sudo rm -f /var/www/html/2130_waso_strategy.zip" and  
2. sudo curl -o /var/www/html/2131_wedding_lite.zip https://www.tooplate.com/zip-templates/2131_wedding_lite.zip && sudo unzip -d /var/www/html/ /var/www/html/2131_wedding_lite.zip && sudo rm -f /var/www/html/2131_wedding_lite.zip

![alt text](<img/a21 unzip tool.PNG>)


 
#### iii) I set up the website's configuration for the two webservers.

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

- I put my webservers IPv4 address in a web browser to verify that my website is up and running.

waso_strategy (IPv4 address) website
![alt text](<img/26a waso unsecure website.PNG>)

wedding lite (IPv4 address) website
![alt text](<img/26b wedding unsecure website.PNG>)


##  TASK 4 , 5 and 6 
### NGINX INSTALLATION ON THE THIRD SERVER AND CONFIGURE AS LOAD BALANCER. 
 
i)  **I executed the following commands to install Nginx on the load balancer**

`sudo apt update`
`sudo apt upgrade`
`sudo apt install nginx`

- I used **`sudo systemctl status nginx`** command to confirm if it's running.

![alt text](<img/a22 nginx installed on a load balancer.PNG>)

ii) I executed sudo nano /etc/nginx/nginx.conf to edit my Nginx configuration file.

I added the following within the http block and substituted <server 1> and <server 2> with the actual private IP addresses of my servers. I also replaced <your domain> www.<your domain> with my root domain and subdomain name. I updated the proxy_pass and the other relevant fields aswell.

    upstream cloudghoul {
    server 1;
    server 2;
    # Add more servers as needed
}

server {
    listen 80;
    server_name <your domain> www.<your domain>;

    location / {
        proxy_pass http://cloudghoul;
    }
}

Nginx Configuration edited
![alt text](<img/a23 nginx configuration edited.PNG>)


- I ran *sudo nginx -t* to check for syntax error.

- I applied the changes by restarting Nginx with command  *sudo systemctl restart nginx*

![alt text](<img/a24 nginx config tested for syntax.PNG>)


## TASK 6
### CREATING RECORD FOR THE LOAD BALANCER TO MAKE WEBSITE ACESSIBLE THROUGH DOMAIN NAME

- Next, I went to the existing hosted zone.

![alt text](<img/a25a existing hosted zone.PNG>)

- I created records for the load balance server in AWS to make the websites accessible via domain name rather than the IP address.  
The elastic IP of the load balancer was also added as A record to the root domain and subdomains

![alt text](<img/a25b root domain.PNG>)

![alt text](<img/a25b sub domain.PNG>)

- After creating the records, I confirmed that they both exist in the records list as shown below.

![alt text](<img/a26 record created.PNG>)

Note : I already included the root domain and subdomain in the server name of the two webservers.


## TASK 7
### ACCESSING THE WEBSITE TO VIEW LOAD BALANCING FUNCTION

I inserted my domain name in the web browser c, I reloaded the web page and could observe the changes (showing unsecure); the load balancer distribute traffic between the two webservers but still unsecured. 
To secure it, I use certbot to obtain the SSL certificate necessary to enable HTTPS on my site.

- I install certbot and Request For an SSL/TLS Certificate by executing the following commands: 
1. *sudo apt update sudo apt install python3-certbot-nginx*
2. *sudo certbot --nginx*

- I executed the *sudo certbot --nginx* command to request my certificate and followed the instructions provided by certbot. I choose blabk since I am choosing all the domian names to activate HTTPS.
- I also renewed my LetsEncrypt certificate by executing the command in dry-run mode: *sudo certbot renew --dry-run*



![alt text](<img/a27aaa install certbot.PNG>)

![alt text](<img/a27bb renew certicicate.PNG>)


#### I visted the below to view my secured website.

I inserted my domain name in the web browser again, I reloaded the web page and could observe the changes (secured); the load balancer distribute traffic between the two webservers as shown below

### www.qserver.space 
![alt text](<img/29a Final secure website test.PNG>)


### www.qserver.space
![alt text](<img/29b Final secure website test.PNG>)

The End Of Project 3