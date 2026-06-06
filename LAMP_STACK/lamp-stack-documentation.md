# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

This project demonstrates the deployment of a LAMP (Linux, Apache, MySQL, PHP) stack on an AWS EC2 instance. The LAMP stack is a foundational open-source web platform used to run dynamic websites and web applications.

---

## Project Overview & Architecture
The LAMP Stack consists of four distinct layers working in harmony:
1. **Linux**: The operating system layer (Ubuntu LTS running on AWS EC2).
2. **Apache**: The web server layer that processes requests and serves assets via HTTP.
3. **MySQL**: The relational database management system (RDBMS) layer used for data storage.
4. **PHP**: The server-side scripting language responsible for processing dynamic content.

---

## Infrastructure Environment Details

The infrastructure for this deployment is provisioned in the **AWS US East (Ohio) `us-east-2`** region with the following system specifications:

| Resource Property | Value / Configuration |
|:---|:---|
| **Instance ID** | `i-06814c959a88a36ca` |
| **Instance Type** | `t2.micro` (1 vCPU, 1 GiB Memory) |
| **Amazon Resource Name (ARN)** | `arn:aws:ec2:us-east-2:330714580032:instance/i-06814c959a88a36ca` |
| **Public IPv4 Address** | `18.225.224.255` |
| **Public DNS (IPv4)** | `ec2-18-225-224-255.us-east-2.compute.amazonaws.com` |
| **Private IPv4 Address** | `172.31.46.215` |
| **Private IP DNS Name** | `ip-172-31-46-215.us-east-2.compute.internal` |
| **Virtual Private Cloud (VPC)** | `vpc-0da29922b6fef1666` |
| **Subnet ID** | `subnet-0e404b8d730363b9a` |
| **IMDSv2** | Required |

---

## Step-by-Step Implementation Guide

### Step 0: Connect to the EC2 Instance via SSH

Open your local terminal and navigate to the directory where your private key (`.pem` file) is saved. Update permissions and establish an SSH connection to your instance:

```bash
# Set appropriate read-only permissions for your private key
chmod 400 your-aws-key.pem

# Securely log into your Ubuntu EC2 instance using its public IP address
ssh -i "your-aws-key.pem" ubuntu@18.225.224.255


Step 1: Installing Apache and Updating the Firewall
First, update the server's local package index to ensure access to the latest software packages, then install the Apache HTTP web server.

Bash
# Update repository package metadata
sudo apt update

# Install Apache Web Server
sudo apt install apache2 -y
Verifying Apache Installation
To ensure that apache2 is up, running, and active within systemd, run the following command:

Bash
sudo systemctl status apache2
<img width="975" height="745" alt="image" src="https://github.com/user-attachments/assets/e02923c7-f382-4f85-b230-75b64c747e38" />


Validating Web Server Access
Verify that the server is reachable over the Internet by hitting its Public IP address or Public DNS from your web browser:

URL: http://18.225.224.255

Alternative URL: http://ec2-18-225-224-255.us-east-2.compute.amazonaws.com

(Place Screenshot here showing the default Apache Ubuntu webpage)
Step 2: Installing MySQL
With the web server active, install MySQL Server to handle structured relational database services.

Bash
# Install MySQL Database Server package
sudo apt install mysql-server -y
Once the installation completes, run the built-in interactive security script to lock down access and remove vulnerable default configurations:

Bash
sudo mysql_secure_installation
Note: Follow the prompts to configure the VALIDATE PASSWORD COMPONENT, set a strong custom root password, remove anonymous users, disallow root login remotely, and remove the test database.

Log into the interactive MySQL database console to verify operations:

Bash
sudo mysql
Exit the database environment using:

SQL
exit
Step 3: Installing PHP
To process dynamic server-side scripts, install the PHP runtime core engine along with modules that allow seamless integration with Apache and MySQL.

Bash
# Install PHP core runtime engine, the Apache module wrapper, and native MySQL utilities
sudo apt install php libapache2-mod-php php-mysql -y
Confirm that the runtime environment is accurately set up and running by verifying the active version build:

Bash
php -v
Step 4: Configuring a Virtual Host for your Project
To support hosting custom target domains or isolated web directories under the same web server, configure an Apache Virtual Host. This ensures our files run outside the default /var/www/html path.

Create a dedicated web directory structure for your project site named projectlamp:

Bash
sudo mkdir /var/www/projectlamp
Re-assign system ownership of the custom directory to your active user account (ubuntu):

Bash
sudo chown -R $USER:$USER /var/www/projectlamp
Generate a clean configurations file for your virtual host inside Apache's sites-available system space:

Bash
sudo nano /etc/apache2/sites-available/projectlamp.conf
Add the following directives exactly into the text editor, save, and close (Ctrl+O, Enter, Ctrl+X):

Apache
<VirtualHost *:80>
    ServerName 18.225.224.255
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
Enable the newly declared Virtual Host file:

Bash
sudo a2ensite projectlamp
Disable the generic, out-of-the-box Apache configuration page layout to prevent structural route conflicts:

Bash
sudo a2dissite 000-default
Validate that the configurations scripts do not contain critical structural errors:

Bash
sudo apache2ctl configtest
(Expected output should include: Syntax OK)

Reload the active web server processes to implement changes:

Bash
sudo systemctl reload apache2
Add a dummy test layout script to the new root directory before testing the connection:

Bash
echo 'Hello LAMP from Ubuntu EC2 Instance ID i-06814c959a88a36ca!' > /var/www/projectlamp/index.html
Now, navigate your web browser back to the public IP endpoint: http://18.225.224.255. It will now serve the customized asset file instead of the boilerplate page layout.

Step 5: Testing PHP Integration with the Web Server
Now, create a dedicated PHP script utility to test that Apache safely interprets incoming backend scripts and processes information down to your users.

Construct an info.php file inside your deployment path root:

Bash
nano /var/www/projectlamp/info.php
Inject the following programmatic block inside the text editor, then save and exit:

PHP
<?php
phpinfo();
?>
Open a browser tab and request your dedicated testing endpoint URL:

Plaintext
[http://18.225.224.255/info.php](http://18.225.224.255/info.php)<img width="1006" height="572" alt="image" src="https://github.com/user-attachments/assets/383f19e0-c775-44d5-a59b-f2372b0beac6" />


⚠️ CRITICAL SECURITY NOTE: After confirming that your backend application components communicate properly, delete the generated info.php testing script file. Leaving it open publicly exposes comprehensive architecture specifications about your AWS virtual container to the web.

Bash
sudo rm /var/www/projectlamp/info.php

# Securely log into your Ubuntu EC2 instance using its public IP address
ssh -i "your-aws-key.pem" ubuntu@18.225.224.255

Conclusion
You have successfully deployed a functional, modular LAMP Stack on your Amazon Web Services EC2 infrastructure! The web server environment is now prepared to run dynamic content pipelines, connect securely to local structural databases, and support custom web applications.
