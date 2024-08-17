# PROJECT TITLE : To Setup a Static Website Using Nginx

The goal of the project is to have a fully deployed and secure static website ready !

### In this project, I will be:

 Building with Nginx: I'll set up Nginx as my web server, the engine that delivers my website content to the world.

 Connecting with Route53: I'll configure Amazon Route53, the DNS service that directs visitors to your website's location.

 Securing with Certbot: I'll implement HTTPS encryption using Certbot, ensuring a safe and secure connection for your website.

By the end of this project, I'll have a fully deployed and secure static website

## Below are the respective task for Project 1

|S/N | Project Tasks                                                                   |
|----|---------------------------------------------------------------------------------|
| 1  |Buy a domain name from a domain Registrar                                        |
| 2  |Spin up an Ubuntu server & assign an elastic IP to it                            |
| 3  |SSH into the server and install Nginx                                            |
| 4  |Download freely HTML website files(too plate) or use your personal code          |
| 5  |Copy the website files to the Nginx website directory                            |
| 6  |Validate the website using the server IP address                                 |
| 7  |In Route53, create an A record and add the Elastic IP                            |
| 8  |Using DNS verify the website setup                                               |
| 9  |Install certbot and Request For an SSL/TLS Certificate                           |
| 10 |Validate the website SSL using the OpenSSL utility                               |


### Create An Ubuntu Server

- I searched and clicked on **EC2** within the AWS management console.

![alt text](<img/1 EC2.PNG>)

- I clicked on **Launch Instance**

![alt text](<img/2 Launch EC2.PNG>)

- I **Named** my instance and selected the **Ubuntu** AMI.

![alt text](<img/3 Ubuntu AMI.PNG>)

- I clicked the **Create new key pair** button to generate a key pair for secure connection to my instance.
- I entered a **Key pair name** and clicked on **Create key pair**.

![alt text](<img/4 create key pair.PNG>)



- I enabled **SSH**, **HTTP**, and **HTTPS** access, then proceeded to click **Launch instance**.

![alt text](<img/5 SSH HTTP.PNG>)


- I clicked on **View all instances** and **created instance**.

![alt text](<img/7 created instance.PNG>)


- Clicked on the **Connect** button.

![alt text](<img/8 Connect to instance.PNG>)

- Copied the command provided under **`SSH client`**.

![alt text](<img/9 SSH Client.PNG>)

- I opened a terminal and located the folder where my **`.pem`** file was downloaded, I used the CD command to move to the folder and pressed Enter.

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

> [!NOTE]
Because IP address for my instance has been updated to the elastic IP associated with it. I SSH it into my instance again and paste the **command** into my terminal and then press Enter. When prompted, I type **"yes"** and press Enter to connect.

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

- I downloaded my "Mini Finance" website template from www.tooplate.com


---

**How to I obtained "Mini Finance" website template URL from tooplate.com:**

- I visited [**Tooplate**](https://www.tooplate.com/) and selected "Mini Finance"

![alt text](<img/21a website template from tooplate.PNG>)

- I Scrolled down to the download section, right-clicked to open the menu, and selected **Inspect** from the options.

- I Selected the **Network** tab.

- Clicked the **Download** button.

- I saw the **"Mini Finance" zip folder** appear and I right-click again.

![alt text](<img/21b website template download.PNG>)


- I pasted the URL into a notebook to use alongside the **`curl`** command when downloading the website content to my machine.

![alt text](<img/21c notepad.PNG>)

---

- I ran this command **`sudo curl -o /var/www/html/2137_barista_cafe.zip https://www.tooplate.com/zip-templates/2137_barista_cafe.zip`** to download the websites file to my html directory.

![alt text](<img/22 curl command for website template.PNG>)


- To install the unzip tool, I ran the following command: **`sudo apt install unzip`**.

- I navigated to the web server directory by running the following command: **`cd /var/www/html`**.

- I unziped the contents of your website by running **`sudo unzip 2135_mini_finance.zip`**.

![alt text](<img/23 install unzip tool_unzip template.PNG>)


- I updated my nginx configuration by running the command **`sudo nano /etc/nginx/sites-available/default`**. Then, edited the **`root`** directive within your server block to point to the directory where my downloaded website content is stored.

![alt text](<img/23 update nginx config with nano.PNG>)

- I restarted Nginx to apply the changes by running: **`sudo systemctl restart nginx`**.

- I opened a web browser and went to my **Public IPv4 address/Elastic IP address** to confirm that my website is working as expected.

![alt text](<img/23c web browser test with my Public IPv4 address or Elastic IP address.PNG>)

---

### Create An A Record

To make my website accessible via your domain name rather than the IP address, I'll set up a DNS record. I did this by buying my domain from Namecheap and then moving hosting to AWS Route 53, where I set up an A record.

![alt text](<img/24 Domain name.PNG>)


- I went back to my AWS console, searched for **Route 53①**, and then choose **Route 53②** from the list of services shown.

![alt text](<img/25 Route 53.PNG>)

- Selected **Create hosted zones①** 

![alt text](<img/26 Create Hosted Zone.PNG>)

- I entered my **Domain name①**, choose **Public hosted zone②** and then click on **Create hosted zone③**.

![alt text](<img/27 Hosted Zone details.PNG>)

- Selected the **created hosted zone①** and copied the assigned **Values②**.

![alt text](<img/28 nameservers in domain.PNG>)

- I went back to my domain registrar and selected **Custom DNS** within the **NAMESERVERS** section.

- I pasted the values copied from Route 53 into the appropriate fields in my domain.

![alt text](<img/28b nameservers in domain website.PNG>)

- I went back to my AWS console and clicked on **Create record**.
- I pasted my Elastic IP address and then clicked on **Create records** for root domian

![alt text](<img/28c creating record in root domain_.PNG>)

- Below shows a record has been successfully created.

![alt text](<img/28d Record created in root domain.PNG>)

- I Clicked on **create record** again, to create the record for my sub domain.

- I inputed the Record name(**www**), paste your **IP address**, and then click on **Create records**.

![alt text](<img/29 creating record in subdomain.PNG>)

![alt text](<img/29b Record created in sub domain.PNG>)

> [!NOTE]
I make sure to create DNS records for both my root domain and subdomain. I set up a record for the root domain  and another record for the subdomain. These records will direct traffic to my server's IP address, ensuring that both my main site and any subdomains are accessible.

- I opened my terminal and run **`sudo nano /etc/nginx/sites-available/default`** to edit my settings. I entered my domain and subdomain names, then save the changes.

![alt text](<img/30 nano to edit your settings _ input domain and subdomain names.PNG>)

- I restarted my nginx server by running the **`sudo systemctl restart nginx`** command.

- I put my domain name in a web browser to verify that my website is accessible.

![alt text](<img/31 domain name in a web browser.PNG>)

> I notice the sign that says **Not secure**. Next, I used certbot to obtain the SSL certificate necessary to enable HTTPS on your site.

---

### Install certbot and Request For an SSL/TLS Certificate

- I installed certbot by executing the following commands:
**`sudo apt update`**
**`sudo apt install certbot python3-certbot-nginx`**

- I executed the **`sudo certbot --nginx`** command to request my certificate. I followed the instructions provided by certbot and selected the domain name for which I would like to activate HTTPS.

![alt text](<img/32 Install certbot.PNG>)

![alt text](<img/33 Request For an SSL_TLS Certificate.PNG>)

- I verified the website's SSL using the OpenSSL utility with the command: **`openssl s_client -connect qserver.space:443`**

![alt text](<img/34 Verify the website's SSL.PNG>)

- I went to **`https://qserver.space`** to view my website showing it's fully secured.

![alt text](<img/35 final secured website.PNG>)

---
---

#### The End Of Project 1