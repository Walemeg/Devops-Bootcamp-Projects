# PROJECT TITLE : To Setup Multiple Static Website Using Nginx Virtual Host

The goal is to learn the concept of subdomains and hosting multiple websites on a single server using Nginx Virtual Host configuration.

### In this project, I will be:

- Building with Nginx: I'll set up Nginx as my web server, the engine that delivers my website content to the world.

- Connecting with Route53: I'll configure Amazon Route53, the DNS service that directs visitors to your website's location.

- Securing with Certbot: I'll implement HTTPS encryption using Certbot, ensuring a safe and secure connection for your website.

By the end of this project, I'll have a fully deployed and secure static website

## Below are the respective task for Project 1

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

### Next ; I downloaded my "Mini Finance" website template from www.tooplate.com]


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

I repeated the same thing for the 2nd website

---

-To install the unzip tool, run the following command: sudo apt install unzip.
Execute the command to download and unzip your website files sudo curl -o /var/www/html/2098_health.zip https://www.tooplate.com/zip-templates/2098_health.zip && sudo unzip -d /var/www/html/ /var/www/html/2098_health.zip && sudo rm -f /var/www/html/2098_health.zip.


Here's an explanation of the command:
The command sudo curl -o /var/www/html/2098_health.zip https://www.tooplate.com/zip-templates/2098_health.zip && sudo unzip -d /var/www/html/ /var/www/html/2098_health.zip && sudo rm -f /var/www/html/2098_health.zip performs a series of actions to download, unzip, and clean up a website template file. Here’s a breakdown of each part of the command:
1.sudo curl -o /var/www/html/2098_health.zip https://www.tooplate.com/zip-templates/2098_health.zip:
osudo: Runs the command with superuser (root) privileges.
ocurl -o /var/www/html/2098_health.zip: Downloads the file from the specified URL (https://www.tooplate.com/zip-templates/2098_health.zip) and saves it as 2098_health.zip in the /var/www/html directory.
2.&&: Logical AND operator, which ensures that the next command runs only if the previous command succeeds.
3.sudo unzip -d /var/www/html/ /var/www/html/2098_health.zip:
osudo: Runs the command with superuser (root) privileges.
ounzip -d /var/www/html/: Extracts the contents of the zip file into the /var/www/html/ directory.
o/var/www/html/2098_health.zip: Specifies the path to the zip file to be unzipped.
4.&&: Logical AND operator, which ensures that the next command runs only if the previous command succeeds.
5.sudo rm -f /var/www/html/2098_health.zip:
osudo: Runs the command with superuser (root) privileges.
orm -f: Removes (deletes) the specified file forcefully (without prompting for confirmation).
o/var/www/html/2098_health.zip: Specifies the path to the zip file to be deleted.
In summary, this command downloads the website template file, extracts its contents to the web server directory, and then deletes the downloaded zip file to clean up the directory.

Download the 2nd website template by running the following command:
sudo curl -o /var/www/html/2132_clean_work.zip https://www.tooplate.com/zip-templates/2132_clean_work.zip && sudo unzip -d /var/www/html/ /var/www/html/2132_clean_work.zip && sudo rm -f /var/www/html/2132_clean_work.zip

Note : Replace the placeholders in the code with your own website URL. For example, substitute https://www.tooplate.com/zip-templates/2132_clean_work.zip with your chosen URL, and change 2132_clean_work.zip accordingly.
To set up your website's configuration, start by creating a new file in the Nginx sites-available directory. Use the following command to open a blank file in a text editor: sudo nano /etc/nginx/sites-available/cleaning

Copy and paste the following code into the open text editor.
server {
    listen 80;
    server_name example.com www.example.com;

    root /var/www/html/example.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
Edit the root directive within your server block to point to the directory where your downloaded website content is stored.

Configure your second website by creating a new file in the Nginx sites-available directory with the following command: sudo nano /etc/nginx/sites-available/health.

Copy and paste the following code into the open text editor.
server {
    listen 80;
    server_name placeholder.com www.placeholder.com;

    root /var/www/html/placeholder.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}


Edit the root directive within your server block to point to the directory where your downloaded website content is stored.






Create a symbolic link for both websites by running the following command. sudo ln -s /etc/nginx/sites-available/cleaning /etc/nginx/sites-enabled/ sudo ln -s /etc/nginx/sites-available/health /etc/nginx/sites-enabled/

Run the sudo nginx -t command to check the syntax of the Nginx configuration file.
Delete the default files in the sites-available and sites-enabled directories by executing the following commands:
sudo rm /etc/nginx/sites-available/default
sudo rm /etc/nginx/sites-enabled/default
Restart the Nginx server by executing the following command: sudo systemctl restart nginx.



Create An A Record
To make your website accessible via your domain name rather than the IP address, you'll need to set up a DNS record. I did this by buying my domain from Namecheap and then moving hosting to AWS Route 53, where I set up an A record.
Note
Visit Project1 for instructions on how to create a hosted zone.
In route 53, select the domain name and click on Create record.

Paste your IP address① and then click on Create records②.






Click on Create record again, to create the record for your sub domain.

Input the Record name①, paste your IP address② and then click on Create records③.




Repeat the same process while creating your second subdomain record, and confirm that they both exist in the records list.

Open your terminal and run sudo nano /etc/nginx/sites-available/cleaning to edit your settings. Enter the name of your domain and then save your settings.


Run sudo nano /etc/nginx/sites-available/health to edit your settings. Enter the name of your domain and then save your settings.

Restart your nginx server by running the sudo systemctl restart nginx command.
Go to your domain name in a web browser to verify that your website is accessible.


Note
You may notice the sign that says Not secure. Next, you'll use certbot to obtain the SSL certificate necessary to enable HTTPS on your site.

Install certbot and Request For an SSL/TLS Certificate
Install certbot by executing the following commands: sudo apt update sudo apt install python3-certbot-nginx sudo certbot --nginx

Execute the sudo certbot --nginx command to request your certificate. Follow the instructions provided by certbot and select the domain name for which you would like to activate HTTPS.

[NOTE] In this case, SSL certificates were only created for cleaning.cloudghoul.online and health.cloudghoul.online. So, when prompted, I entered the numbers 1 and 3 (corresponding to those two domains) to select them for certificate generation. If we had also created records for www.cleaning.cloudghoul.online and www.health.cloudghoul.online, I could have simply pressed Enter to accept the default selection (all available records). If you try to create a certificate without having created an A record first, you will receive an error message.
Verify the website's SSL using the OpenSSL utility with the command: openssl s_client -connect cleaning.cloudghoul.online:443

Visit https://<domain name> to view your websites.




The End Of Project 2