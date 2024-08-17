# PROJECT-1 : To Setup a Static Website Using Nginx

The goal of the project is to have a fully deployed and secure static website ready !

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

- Locate and click on **EC2** within the AWS management console.

![alt text](<img/1 EC2.PNG>)

- Click on **Launch Instance**

![alt text](<img/2 Launch EC2.PNG>)

- **Name** your instance and select the **Ubuntu** AMI.

![alt text](<img/3 Ubuntu AMI.PNG>)

- Click the **Create new key pair** button to generate a key pair for secure connection to your instance.
- Enter a **Key pair name** and click on **Create key pair**.

![alt text](<img/4 create key pair.PNG>)



- Enable **SSH**, **HTTP**, and **HTTPS** access, then proceed to click **Launch instance**.

![alt text](<img/5 SSH HTTP.PNG>)


- Click on **View all instances** and **created instance**.

![alt text](<img/7 created instance.PNG>)


- Click on the **Connect** button.

![alt text](<img/8 Connect to instance.PNG>)

- Copy the command provided under **`SSH client`**.

![alt text](<img/9 SSH Client.PNG>)

- Open a terminal in the directory where your **`.pem`** file was downloaded, paste the command and press Enter.

![alt text](<img/10 Open terminal.PNG>)


### Create And Assign an Elastic IP

- Return to your AWS console and click on the **menu icon** to open the dashboard menu. Select **Elastic IPs** under **Network & Security**.

![alt text](<img/11-12 Elastic ip.PNG>)

- Click on the **Allocate Elastic IP address** button.

![alt text](<img/13 Allocate IP address.PNG>)

- Keep the settings unchanged and proceed to click **Allocate**.

![alt text](<img/14 allocate.PNG>)

- **Associate this Elastic IP address** with your running instance.

![alt text](<img/15 Associate the public address with instance.PNG>)

- Select the instance you wish to associate with the elastic IP address, then click on **Associate**.

![alt text](<img/16 select instance to be associated with elastic ip.PNG>)

> [!NOTE]
Because IP address for my instance has been updated to the elastic IP associated with it. I will need to SSH into my instance again and paste the **command** into your terminal and then press Enter. When prompted, type **"yes"** and press Enter to connect.

![alt text](<img/17 ssh into new instance with elastic IP address.PNG>)

---

### Install Nginx and Setup Your Website

- Execute the following commands.

`sudo apt update`
`sudo apt upgrade`
`sudo apt install nginx`

- I used  **`sudo systemctl start nginx`** command for starting my Nginx server.  
- I used **`sudo systemctl enable nginx`** for enabling the server to start on boot.
- I used **`sudo systemctl status nginx`** command to confirm if it's running.

![alt text](<img/18 Nginx installed.PNG>)

- Go back to your EC2 dashboard and copy your **Public IPv4 address**.

![alt text](<img/19 copied IPv4 address.PNG>)

- Visit your instances **Public IPv4 address** in a web browser to view the default Nginx startup page.

![alt text](<img/20 instance Public IPv4 address in web browser.PNG>)

- Download your website template from your preferred website by navigating to the website, locating the template you want, and obtaining the download URL for the website.


---

**How to I obtained the website template URL from tooplate.com:**

- Visit [**Tooplate**](https://www.tooplate.com/) and select the website template you prefer.

![alt text](<img/21a website template from tooplate.PNG>)

- Scroll down to the download section, right-click to open the menu, and select **Inspect** from the options.

- Select the **Network** tab.

- Click the **Download** button.

- You’ll see the **website zip folder** appear. Hover your mouse or trackpad pointer over it and right-click again.

![alt text](<img/21b website template download.PNG>)


- Paste the URL into a notebook to use alongside the **`curl`** command when downloading the website content to your machine.

![alt text](<img/21c notepad.PNG>)

---

- Run this command **`sudo curl -o /var/www/html/2137_barista_cafe.zip https://www.tooplate.com/zip-templates/2137_barista_cafe.zip`** to download the websites file to your html directory.

![alt text](<img/22 curl command for website template.PNG>)


- To install the unzip tool, run the following command: **`sudo apt install unzip`**.

- Navigate to the web server directory by running the following command: **`cd /var/www/html`**.

- Unzip the contents of your website by running **`sudo unzip 2135_mini_finance.zip`**.

![alt text](<img/23 install unzip tool_unzip template.PNG>)


- Update your nginx configuration by running the command **`sudo nano /etc/nginx/sites-available/default`**. Then, edit the **`root`** directive within your server block to point to the directory where your downloaded website content is stored.

![alt text](<img/23 update nginx config with nano.PNG>)

- Restart Nginx to apply the changes by running: **`sudo systemctl restart nginx`**.

- Open a web browser and go to your **Public IPv4 address/Elastic IP address** to confirm that your website is working as expected.

![alt text](<img/23c web browser test with my Public IPv4 address or Elastic IP address.PNG>)

---

### Create An A Record

To make your website accessible via your domain name rather than the IP address, you'll need to set up a DNS record. I did this by buying my domain from Namecheap and then moving hosting to AWS Route 53, where I set up an A record.

![alt text](<img/24 Domain name.PNG>)


- Go back to your AWS console, search for **Route 53①**, and then choose **Route 53②** from the list of services shown.

![alt text](<img/25 Route 53.PNG>)

- Select **Create hosted zones①** and click on **Get started②**.

![alt text](<img/26 Create Hosted Zone.PNG>)

- Enter your **Domain name①**, choose **Public hosted zone②** and then click on **Create hosted zone③**.

![alt text](<img/27 Hosted Zone details.PNG>)

- Select the **created hosted zone①** and copy the assigned **Values②**.

![alt text](<img/28 nameservers in domain.PNG>)

- Go back to your domain registrar and select **Custom DNS** within the **NAMESERVERS** section.

- Paste the values you copied from Route 53 into the appropriate fields in your domain.

![alt text](<img/28b nameservers in domain website.PNG>)

- Head back to your AWS console and click on **Create record**.
- Paste your Elastic IP address and then click on **Create records** for root domian

![alt text](<img/28c creating record in root domain_.PNG>)

- Your A record has been successfully created.

![alt text](<img/28d Record created in root domain.PNG>)

- Click on **create record** again, to create the record for your sub domain.

- Input the Record name(**www➀**), paste your **IP address➁**, and then click on **Create records➂**.

![alt text](<img/29 creating record in subdomain.PNG>)

![alt text](<img/29b Record created in sub domain.PNG>)

> [!NOTE]
I make sure to create DNS records for both my root domain and subdomain. This involves setting up an A record for the root domain (e.g., **`example.com`**) and another A record for the subdomain (e.g., **`www.example.com`**). These records will direct traffic to my server's IP address, ensuring that both my main site and any subdomains are accessible.

- Open your terminal and run **`sudo nano /etc/nginx/sites-available/default`** to edit your settings. Enter your domain and subdomain names, then save the changes.

![alt text](<img/30 nano to edit your settings _ input domain and subdomain names.PNG>)

- Restart your nginx server by running the **`sudo systemctl restart nginx`** command.

- Go to your domain name in a web browser to verify that your website is accessible.

![alt text](<img/31 domain name in a web browser.PNG>)

> [!NOTE]
You may notice the sign that says **Not secure**. Next, you'll use certbot to obtain the SSL certificate necessary to enable HTTPS on your site.

---

### Install certbot and Request For an SSL/TLS Certificate

- Install certbot by executing the following commands:
**`sudo apt update`**
**`sudo apt install certbot python3-certbot-nginx`**

- Execute the **`sudo certbot --nginx`** command to request your certificate. Follow the instructions provided by certbot and select the domain name for which you would like to activate HTTPS.

![alt text](<img/32 Install certbot.PNG>)

![alt text](<img/33 Request For an SSL_TLS Certificate.PNG>)

- Verify the website's SSL using the OpenSSL utility with the command: **`openssl s_client -connect qserver.space:443`**

![alt text](<img/34 Verify the website's SSL.PNG>)

- Visit **`https://qserver.space`** to view your website.

![alt text](<img/35 final secured website.PNG>)

---
---

#### The End Of Project 1