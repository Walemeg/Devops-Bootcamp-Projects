# PROJECT 4
## SETUP WORDPRESS WEBSITE USING LAMP STACK

### GOAL
The goal of this project is to Setup a WordPress website using the LAMP stack (Linux, Apache, MySQL, PHP) It involves setting up my server environment to installing WordPress and configuring it for optimal performance. 
This documentation involves Step-by-step path I followed for installing and configuring each of the LAMP components and securing database creation for my WordPress website.


### LAMP TOOLS AND EXPECTED FUNCTIONS

The LAMP tools I'll be using for this project are

#### 1) LINUX

Linux is the operating system of the entire infrastructure. It provides a stable and secure environment for hosting the web applications. It's very key to successful deployment and management of the LAMP-based WordPress website; It's coordinates the interaction between Apache (the web server), MySQL (the database), and PHP (the scripting language), ensuring seamless operation and communication between these components.

#### 2) APACHE

Apache is the open-source web server software that handles client requests and serves web content. It will process incoming HTTP requests, fetching and delivering web pages to users' browsers. It supports various modules that includes URL redirection, authentication, and load balancing. Utimately, Apache ensures that your WordPress website runs smoothly and can handle a large number of concurrent users.

#### 3) MYSQL

MySQL is the database management system within the LAMP stack, responsible for storing and managing the data that powers your web applications. It works closely with PHP to handle data operations such as storing user information, retrieving content, and managing transactions. For a WordPress website, It stores all the content, user data, settings, and other essential information, allowing for dynamic content generation and efficient data retrieval.

#### 4) PHP

PHP is the scripting language in the LAMP stack. It's responsible for generating dynamic web content; It processes code on the web server to create HTML content that is then sent to the user's browser. It works with Apache to handle web requests and interact with the MySQL database to retrieve and manipulate data; the interaction with Apache and MySQL enables the WordPress site to be dynamic, interactive, and capable of delivering expected user experience.


#### CONCEPTS COVERED

- AWS (EC2 and Route 53) , - Linux(Ubuntu) , Apache , MySQL , PHP, Wordpress , DNS , SSL (certbot) , OpenSSL command

---

### PROJECT TASK

|S/N |  Tasks                                                       |
|----|---------------------------------------------------------------------|
| 1  |Deploy an Ubuntu Server                                              |
| 2  |Set up your LAMP stack on the server                                 |
| 3  |Configure the wordpress Application                                  |
| 4  |Map the IP address to the DNS A record                               |
| 5  |Validate the WordPress website setup by accessing the web address.   |



## DOCUMENTATION

### TASK-1 DEPLOY AN UBUNTU SERVER 
- An Unbuntu server named "lamp stack was first launced as shown below.

![alt text](<img/1 Launch Instance.PNG>)


- I selected my lamp-stack instance/server and proceeded to set inbound rule for MYSQL under the security group. I clicked on **Security** and select the **Security group**.

![alt text](<img/2 Click inbound rules.PNG>)

- Click on **Edit inbound rules**.

![alt text](<img/2b edit inbound.PNG>)

- I clicked on **Add rule** ,  **Custom TCP** and selected **MySQL/Aurora**; "
- I entered my server's **IP address** "54.175.204.158/32" that restrict MSQL access to and clicked **Save rules** as shown below.
I also specified the source as /32

![alt text](<img/2c Add Rule MSQL.PNG>)

![alt text](<img/2d Inbound rule saved.PNG>)


- I opened my **terminal** and connected to my Ubuntu server via SSH.

![alt text](<img/3 ssh to terminal.PNG>)


### TASK-2 SETUP LAMP STACK ON THE SERVER 

### STEP 1:  Install Apache

I installed Apache, run the following commands together in my terminal.

**`sudo apt update`** and **`sudo apt install apache2`**

![alt text](<img/3b install Apache.PNG>)

- I executed **`sudo systemctl enable apache2`** and **`sudo systemctl status apache2`** together command To enable Apache to start on boot and verify status respectively

![alt text](<img/3c Boot and verify Apache status.PNG>)

- I used the curl command to check if my server is running and accessible both locally and from the Internet by executing  **`curl http://localhost:80`**.

![alt text](<img/3d curl command to check server running.PNG>)

- Inorder to test how my Apache HTTP server responds to requests from the Internet, I Copied my **public IPv4 address** from my EC2 dashboard and opened a web browser to try accessing the URL: **`http://172.31.81.179:80`**. I got the Apavhe page below which shows successful installation.

![alt text](<img/3e test if Apache Http server responds to internet request.PNG>)



---

### STEP 2: Install MYSQL

- To install this software using 'apt', run the command **`sudo apt install mysql-server`**. When prompted, confirm the installation by typing 'Y' and then pressing ENTER.

![9](img/9.png)

- After the installation is complete, log in to the MySQL console by typing: **`sudo mysql`**.

> [!NOTE]
It's recommended that you run a security script included with MySQL to enhance security. Before running the script, set a password for the root user using the default authentication method **mysql_native_password**. For this project, we'll define the password as "**pass**", but you can choose any password you prefer.

- Run the following command to set the password for the root user with the MySQL native password authentication method: **`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'pass';`**. Exit the MySQL shell when you're done by typing **`exit`**.

![10](img/10.png)

- Start the interactive script by running: **`sudo mysql_secure_installation`①**. Answer **y**② for yes, or any other key to continue without enabling specific options.

![11](img/11.png)

- Set your **password validation policy level**.

![12](img/12.png)

> [!NOTE]
I set my password validation policy level to 0 because I don't require much security, as I will be terminating all resources immediately after this project. However, on the job, it's advised to use the strongest level, which is 2.

- Enable MySQL to start on boot by executing **`sudo systemctl enable mysql`①**, and then confirm its status with the **`sudo systemctl status mysql`②** command.

![13](img/13.png)

---

### Install PHP

- Install PHP along with required extensions by running the following script: **`sudo apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip`**.

![14](img/14.png)

**`sudo apt install php libapache2-mod-php php-mysql`**

![15](img/15.png)

- Confirm the downloaded PHP version by running **`php -v`**.

![16](img/16.png)

### Creating A Virtual Host For Your Website Using Apache

- Create the directory for Projectlamp using the 'mkdir' command as follows:
**`sudo mkdir /var/www/projectlamp`①** and assign ownership of the directory to our current system user using:
**`sudo chown -R $USER:$USER /var/www/projectlamp`②**.

![17](img/17.png)

- Create and open a new configuration file in Apache's sites-available directory using your preferred command-line editor:
**`sudo vi /etc/apache2/sites-available/projectlamp.conf`**.

- Creating this will produce a new blank file. Paste the configuration text provided below into it:

```
<VirtualHost *:80>

ServerName projectlamp

ServerAlias www.projectlamp

ServerAdmin webmaster@localhost

DocumentRoot /var/www/projectlamp

ErrorLog ${APACHE_LOG_DIR}/error.log

CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```

![18](img/18.png)

- Save your changes by pressing the **`Esc`** key, then type **`:wq`** and press **`Enter`**.

![19](img/19.png)

- Run the ls command **`sudo ls /etc/apache2/sites-available`①** to show the **new file②** in the sites-available directory.

![20](img/20.png)

- We can now enable the new virtual host using the a2ensite command: **`sudo a2ensite projectlamp`**.

![21](img/21.png)

- To disable Apache's default website, use the a2dissite command. Type: **`sudo a2dissite 000-default`**.

![22](img/22.png)

- To ensure your configuration file doesn’t contain syntax errors, run: **`sudo apache2ctl configtest`**. You should see **"Syntax OK"** in the output if your configuration is correct.

![23](img/23.png)

- Finally run: **`sudo systemctl reload apache2`**. This will reload Apache for the changes to take effect.

> [!NOTE]
Our new website is now active, but the web root **`/var/www/projectlamp`** is still empty. Let's create an **`index.html`** file in that location to test that the virtual host works as expected.

- To create the **index.html** file with the content **"Hello LAMP from Jay"** in the /var/www/projectlamp directory, use the following command: **`sudo echo 'Hello LAMP from Jay' > /var/www/projectlamp/index.html`**.

![24](img/24.png)

- Now, let's open our web browser and try to access our website using the IP address:

**`http://<EC2-Public-IP-Address>:80`**

> [!NOTE]
Replace **`<EC2-Public-IP-Address>`** with your actual EC2 instance's public IP address.

![25](img/25.png)

- Remove the index.html file by running the following command: **`sudo rm /var/www/projectlamp/index.html `**

### Enable PHP On The Website

With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file. To change the precedence of index files (such as index.php over index.html) in Apache, you'll need to edit the dir.conf file. Here’s how you can do it:

- Edit the dir.conf file using a text editor (such as nano or vi): **`sudo nano /etc/apache2/mods-enabled/dir.conf`**

- Look for the DirectoryIndex directive within this file. It typically looks like this:

```
<IfModule mod_dir.c>
    DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>
```

- To prioritize **index.php** over **index.html**, move **index.php** to the beginning of the list, like this:

```
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

![26](img/26.png)

- Press **`ctrl` + `x`①** on your keyboard to save and exit.

![27](img/27.png)

- Type **`y`②** to save the changes.

![28](img/28.png)

- When prompted to confirm the file name, simply press **`ENTER`③** to save the changes with the existing file name.

![29](img/29.png)

- Finally, reload Apache for the changes to take effect: **`sudo systemctl reload apache2`**.

![30](img/30.png)

Now, Apache will prioritize index.php over index.html when both files exist in the same directory.

- To create a new file named index.php inside your custom web root folder (/var/www/projectlamp), you can use the following command to open it in the nano text editor: **`nano /var/www/projectlamp/index.php`**.

- This will create a new file. Copy and paste the following PHP code into the new file:

```
<?php

phpinfo();
```

![31](img/31.png)

- Once you've saved and closed the file, go back to your web browser and refresh the page. You should see something like this:

![32](img/32.png)

> [!NOTE]
This page provides information about your server from the perspective of PHP. It is useful for debugging and to ensure that your settings are being applied correctly.

If you can see this page in your browser, then congratulations🎉 your PHP installation is working as expected.
After verifying the relevant information about your PHP server through that page, it's recommended to remove the file you created, as it contains sensitive information about your PHP environment and your Ubuntu server. You can use the rm command to do so:
**`sudo rm /var/www/projectlamp/index.php`**.

You can always recreate this page if you need to access the information again later.

---

### Install Wordpress

After setting up our LAMP environment, we can start installing WordPress. First, we'll download the WordPress installation files and place them in the default web server root directory: **/var/www/html**.

- Navigate to the directory using the cd command **`cd /var/www/html`**, and then download the WordPress installation files using the following command: **`sudo wget -c http://wordpress.org/latest.tar.gz`**

![33](img/33.png)

- Extract the files from the downloaded WordPress archive using the command: **`sudo tar -xzvf latest.tar.gz`**

![34](img/34.png)

- Run the command **`ls -l`** to confirm the existence of the **wordpress** directory in the current location (`/var/www/html`).

![35](img/35.png)

> [!NOTE]
The files must be owned by the user of your web server. Identify the web server's user and assign the appropriate permissions accordingly. The user **`www-data`** is widely adopted as the default user for web server processes, especially on Ubuntu and Debian systems. This user oversees the operation of web server software (like Apache or Nginx) and manages incoming web requests. However, for verification and for tasks involving services that may not have a predefined user, checking the user of the web server is advisable.

- Check the user running your web server with the command: **`ps aux | grep apache | grep -v grep`**.

![36](img/36.png)

*This command filters processes related to Apache (apache2 on Ubuntu) and displays information about the user running those processes.*

- Grant ownership of the WordPress directory and its files to the web server user **(www-data)** by running the command: **`sudo chown -R www-data:www-data /var/www/html/wordpress`**.

![37](img/37.png)

### Create a Database For Wordpress

- Access your MySQL root account with the following command: **`sudo mysql -u root -p`①**. Enter the **password②** you set earlier when prompted.

![38](img/38.png)

- To create a separate database named wp_db for WordPress to manage, execute the following command in the MySQL prompt: **`CREATE DATABASE wp_db;`**

![39](img/39.png)

> [!NOTE]
This command allows you to create a new database (**wp_db**) within your MySQL environment. Feel free to name it as you prefer.

- To access the new database, you can create a MySQL user account with a strong password using the following command:
**`CREATE USER jay@localhost IDENTIFIED BY 'wp-password';`**

![40](img/40.png)

*Replace 'wp-password' with your preferred strong password for the MySQL user account.*

- To grant your created user (jay@localhost) all privileges needed to work with the wp_db database in MySQL, use the following commands:

```
GRANT ALL PRIVILEGES ON wp_db.* TO jay@localhost;
FLUSH PRIVILEGES;
```

![41](img/41.png)

> [!NOTE]
This grants all privileges **(ALL PRIVILEGES)** on all tables within the wp_db database **(`wp_db.*`)** to the user jay when accessing from localhost. The FLUSH PRIVILEGES command ensures that MySQL implements the changes immediately. Adjust the database name **(wp_db)** and username **(jay)** as per your setup.

- Type **`exit`** to exit the MySQL shell.

![42](img/42.png)

- Grant executable permissions recursively (-R) to the wordpress folder using the following command: **`sudo chmod -R 777 wordpress/`**

![43](img/43.png)

> [!NOTE]
This command sets read (r), write (w), and execute (x) permissions for the owner, group, and others on all files and directories within the wordpress folder. Using 777 permissions is quite permissive and may not be necessary for all files and folders; consider adjusting permissions based on security requirements.

- Change into the WordPress directory by running the command: **`cd wordpress`**.

![44](img/44.png)

### Configure Wordpress

Once you've established a database for WordPress, the next crucial step is setting up and configuring WordPress itself. To begin, you'll need to create a configuration file tailored for WordPress.

- Rename the sample WordPress configuration file with the command: **`mv wp-config-sample.php wp-config.php`**.

- Edit the **`wp-config.php`** file using the command: **`sudo nano wp-config.php`**.

![45](img/45.png)

- Update the database settings in the **`wp-config.php`** file by replacing placeholders like **database_name_here**, **username_here**, and **password_here** with your actual database details.

![46](img/46.png)

- Modify the configuration file projectlamp.conf: **`sudo nano /etc/apache2/sites-available/projectlamp.conf`** to update the document root to the directory where your WordPress installation is located.

![47](img/47.png)

- After updating the document root to **`/var/www/html`** directory in your editor, save the changes and exit.

![48](img/48.png)

- Reload Apache for the changes to take effect: **`sudo systemctl reload apache2`**.

- Once you've completed these steps, you can access your WordPress page to complete the installation. Open your web browser and go to **`http://<EC2 IP>/wordpress/`**. This will lead you to the WordPress setup wizard where you can finalize the installation process.

> [!NOTE]
Replace **<EC2 IP>** with the IP address of your EC2 instance when accessing your WordPress page.

- Select your preferred language and then click on **Continue** to proceed.

![49](img/49.png)

- Enter the required information and click on **Install WordPress** once you have finished.

  - **Site Title①:** Enter the name of your WordPress website. It's recommended to use your domain name for better optimization.
  - **Username②:** Choose a username for logging into WordPress.
  - **Password③:** Set a secure password to protect your WordPress account.
  - **Your email④:** Provide your email address to receive updates and notifications.
  - **Search engine visibility⑤:** You can leave this box unchecked to prevent search engines from indexing your site until it's ready.

![50](img/50.png)

- WordPress has been successfully installed. You can now log in to your admin dashboard using the previously set up information by clicking the **Log In** button.

![51](img/51.png)

- Enter your username and password, then click **Log In** to access your WordPress admin dashboard.

![52](img/52.png)

- Once you successfully log in, you will be greeted by the WordPress dashboard page.

![53](img/53.png)

---

### Create An A Record

To make your website accessible via your domain name rather than the IP address, you'll need to set up a DNS record. I did this by buying my domain from Namecheap and then moving hosting to AWS Route 53, where I set up an A record.

> [!NOTE]
Visit [**Project1**](https://github.com/StrangeJay/devops-beginner-bootcamp/blob/main/project1/project1.md) for instructions on how to create a hosted zone.

- Point your domain's DNS records to the IP addresses of your Apache load balancer server.

- In route 53, click on **Create record**.

![54](img/54.png)

- Paste your **IP address➀** and then click on **Create records➁** to create the root domain.

![55](img/55.png)

- Click on **Create record** again, to create the record for your sub domain.

![56](img/56.png)

- Paste your IP address➀, input the Record name(**www➁**) and then click on **Create records**➂.

![57](img/57.png)

- To update your Apache configuration file in the sites-available directory to point to your domain name, use the command: **`sudo nano /etc/apache2/sites-available/projectlamp.conf`**.

> [!NOTE]
This command opens the **`projectlamp.conf`** file in the nano text editor with superuser privileges **(`sudo`)**. Within the editor, adjust the necessary details to reflect your domain name configuration.

- Ensure that the server settings in your Apache configuration point to your domain name, and that the document root accurately points to your WordPress directory. Once you've made these adjustments, save the changes and exit the editor.

```
<VirtualHost *:80>
    ServerName <Your root domain name>
    ServerAlias <Your sub domain name>
    ServerAdmin webmaster@<Your root domain name>

    DocumentRoot /var/www/html/wordpress

    <Directory /var/www/html/wordpress>
        Options Indexes FollowSymLinks
       # AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

```

![58](img/58.png)

> [!NOTE]
The new configuration defines how Apache should handle requests for your domain, and its subdomain. With this configuration: Apache will handle requests for **cloudghoul.online** and **www.cloudghoul.online**. Files will be served from the **`/var/www/html/wordpress`** directory. Directory listings and symbolic links are allowed. The directory can be accessed by any client.
Error logs will be written to **`/var/log/apache2/error.log`**. Access logs will be written to **`/var/log/apache2/access.log`** in the combined log format.

- To update your **`wp-config.php`** file with DNS settings, use the following command: **`sudo nano wp-config.php`** and add these lines to the file:

```
/** MY DNS SETTINGS */
define('WP_HOME', 'http://<domain name>');

define('WP_SITEURL', 'http://<domain name>');
```

Replace **`http://<domain name>`** with your actual domain name. Save the changes and exit the editor.

![59](img/59.png)

- Reload your Apache server to apply the changes with the command: **`sudo systemctl reload apache2`**, After reloading, visit your website at **`http://<domain name>`** to view your WordPress site. Replace **<domain name>** with your actual domain name.

![60](img/60.png)

- To log in to your WordPress admin portal, visit **`http://<domain name>/wp-admin`**, Enter your **username①** and **password②**, then click on **log In③**. *Replace **<domain name>** with your actual domain name.*

![61](img/61.png)

> [!NOTE]
My domain name is **cloudghoul.online**, so i'll visit **`http://cloudghoul.online/wp-admin`**.

- Now that your WordPress site is successfully configured to use your domain name, the next step is to secure it by requesting an SSL/TLS certificate.

![62](img/62.png)

---

### Install certbot and Request For an SSL/TLS Certificate

- Install certbot by executing the following commands:
`sudo apt update`
`sudo apt install certbot python3-certbot-apache`

- Run the command **`sudo certbot --apache`** to request your SSL/TLS certificate. Follow the instructions provided by Certbot to select the domain name for which you want to enable HTTPS.

![63](img/63.png)

- You should receive a message confirming that the certificate has been successfully obtained.

![64](img/64.png)

- Visit your website to confirm, and you'll notice that the **"not secure"** warning no longer appears, indicating that your site is now secure with HTTPS.

![65](img/65.png)

![66](img/66.png)

---
---

#### The End Of Project 4