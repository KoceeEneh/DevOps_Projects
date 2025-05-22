# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

## Project Overview
a LAMP stack is an open source technological stack used for building and hosting web applications.AWS provides a versatile and scalable environment for running web applications hence why it was used to deploy the project. this is a part of the DevOps practice.

## Diagram

- Linux server deployed in the AWS cloud

![lamp](https://github.com/user-attachments/assets/59a71426-84c1-44e9-82c5-4b021cdd1d0c)

- Apache, MySQL, PHP running inside the instance

![main](https://github.com/user-attachments/assets/a3a6a528-10e5-4884-ad2e-cd9f020e2e81)



## Technologies
- AWS EC2
- ubuntu server
- Apache
- MySQL
- PHP
- SSH

## Project Objective

- Understand how to set up a full web stack manually.

- Learn basic Linux administration and package installation.

- Practice secure remote access using SSH.

- Serve dynamic content with PHP and connect to a MySQL DB.

## Prerequisites
- AWS Account

- Basic knowledge of Linux commands

- SSH key pair

## Implementation

### 1. Creating an AWS account 

For this project you will need an aws account, to make this shorter I’ll cover a separate documentation on how to set up an aws account successfully.
<br>
Also im using a Mac OS so I just need to ssh into the server after creating. For windows you need a tool called putty to connect to your instance.

### 2. Launching an EC2 instance 

- use a suitable name for the instance
for this instance i used 

` PROJECT_1`

- Create a key pair but if you already have one just use it.

<img width="944" alt="Screenshot 2025-05-19 at 17 13 20" src="https://github.com/user-attachments/assets/4aca884b-8ce0-4ae5-8c56-b3c0bd8f6456" />

- Make sure ssh is enabled

 <img width="907" alt="Screenshot 2025-05-19 at 17 18 16" src="https://github.com/user-attachments/assets/e6afc63d-3c8c-4944-99a8-6685507bbe12" />


- Since this is a simple ec2 instance leave everything in their default state.

- Time to ssh into the server (note that the server in this context is the ec2 instance we created)

- In your terminal cd into the location of the PEM file you created

 ```bash

cd Downloads
```

- in your aws console click on connect

<img width="711" alt="Screenshot 2025-05-19 at 17 26 24" src="https://github.com/user-attachments/assets/c5141610-97e4-4f6c-9f44-fea58c1cb95a" />

- There you will see ssh client click on it to reveal the command you will use to ssh

<img width="663" alt="Screenshot 2025-05-19 at 17 30 24" src="https://github.com/user-attachments/assets/7900bed8-9ca8-4899-b98e-24729c9333d3" />

- Copy and paste into your terminal

- You might receive an “unprotected key file warning”, if you do just change the ownership of the pem file

- Proceed to ssh again
  
<img width="682" alt="Screenshot 2025-05-19 at 17 34 49" src="https://github.com/user-attachments/assets/ac5f7d26-4b18-4d33-8e49-b71c1d494b0e" />

- Congratulations you have just created your first linuxserver in the cloud!

### 3. Installing and setting up apache web server 

We will be using the ubuntu package manager apt to install apache

- First we update
```bash
sudo apt update
```

- Run apache2 package installation

```bash
sudo apt install apache2
```

- Then verify that the service is running

 ```bash

sudo systemctl status apache2
```

- Congratulations you have ran your first web server!

### 4. Opening port 80

We need to open port 80 which is used  by web browsers to access web pages on the internet

- We head over to the ec2 instance to add a rule to open inbound connection through port 80

- After that we can check if the port is working by running

```bash
curl http://127.0.0.1 :80
```
 in the terminal. if you got a lines of codes that doesnt really make much sense. then, congratulations it works.

- lets check if its accessible over the internet by using

```bash
http://<Public-IP-Address>:80
```

- To retrieve your public ip directly from the terminal use

```bash
 curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```
 
- If you see the “it works” interface then it actually works

<img width="1130" alt="Screenshot 2025-05-19 at 18 16 39" src="https://github.com/user-attachments/assets/e619c3b8-942f-4850-bd56-7e09fce0d8c6" />


### 5. Installing SQL

- Installing sql

```bash
sudo apt install mysql-server
```
  
When prompted confirm installation by typing Y and ENTER

- After installing log in to your  mysql

- This connects you to the mysql server as the admin user root

- set a password for root
  
We will use musql_native_password as default authentication 

```bash
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PASSWORD';
```

replace 'PASSWORD' with your actual password

- Exit after

- Start the script by running

```bash
$ sudo mysql_secure_installation
```

<img width="565" alt="Screenshot 2025-05-19 at 20 32 37" src="https://github.com/user-attachments/assets/ea10fc2a-39fb-4d15-bdd8-7755c00d9a8b" />


- Once it runs it requests your root password

  `PASSWORD`

- Next it asks you to. Validate password plugins, enter Y for yes

 <img width="572" alt="Screenshot 2025-05-19 at 20 35 22" src="https://github.com/user-attachments/assets/ed37348f-97e2-4198-b305-8e9013892fb0" />

<img width="572" alt="Screenshot 2025-05-19 at 20 35 22" src="https://github.com/user-attachments/assets/c0eec25d-08b2-4a58-8ba1-75895ab42b40" />

- For the rest of the question press Y and hit ENTER key at each prompt

- When you are done, test if you are able to login to the mysql console using

```bash
sudo mysql -p
```

### 6. Installing PHP

- For this we will be needing php-mysql, a PHP module that allows it to communicate with mysql-based databases. We will also need libapache2-mod-php to enable apache to handle PHP files.

- Installing the 3 packages

```bash
 sudo apt install php libapache2-mod-php php-mysql
```

- Confirm the version

```bash
php -v
```

- Congratulations you just set up php now the LAMP stack is complete!

### 7. Creating a virtual host for your website using apache 

- Setting up a domain eg; 'project1lamp'

- Create a directory for project1lamp

```bash
sudo mkdir var/www/project1lamp
```

- Assign ownership of the direciory to the current user

```bash
sudo chown -R $USER:$USER /var/www/projectlamp
```

- Create and open a new config file in apache’s site-available directory.

Paste this inside 

```bash 

<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
 </VirtualHost> }
```

- You can use the ls command to show the new file we just created

```bash
sudo ls /etc/apache2/sites-available
```

- In the course of this project there is no domain name for project1lamp yet so we can use ‘#’ to comment the server name and server alias. This tells the program to skip those lines

- to enable the new virtual host use

```bash

sudo a2ensite project1lamp

```

- To disable the default website that comes with apache use

```bash
 sudo a2dissite 000-default
```

- Next we test to make sure it doesn’t contain errors
  
```bash
sudo apache2ctl configtest
```

- If we get a syntax ok, the next step is to reload to apply the changes

 ```bash
sudo systemctl reload apache2
```

- our new website is now active, but the web root `/var/www/projectlamp` is still empty. Create an index.html file in that location so that we can test that the virtual host works as expected:

```bash

sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
```

- Now go to your browser and try to open your website URL using IP address:

` http://<Public-IP-Address>:80`

Screenshot 2025-05-19 at 22.47.05

### 8. Enabling PHP on the website

- First we change the order the index.php file is listed within the directory index

```bash

sudo vim /etc/apache2/mods-enabled/dir.conf
```
change this:

`DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm`

To this:

`DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm`

- After saving and closing the file, you will need to reload Apache so the changes take effect:

- Next we create a php script to test that php is correctly installed and configured on the server

 ```bash
nano /var/www/projectlamp/index.php
```
created inside our custom web root folder

- Inside the blank file add the following php code:

  ```bash
   <?php phpinfo();
  ```

- This page provides information about your server from the perspective of PHP. It is useful for debugging and to ensure that your settings are being applied correctly.

  <img width="1440" alt="Screenshot 2025-05-20 at 21 20 39" src="https://github.com/user-attachments/assets/959fe6f1-bf86-4a62-8db4-7792a706b402" />

If you can see this page in your browser, then your PHP installation is working as expected.

- After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment -and your Ubuntu server. You can use rm to do so:

```bash

sudo rm /var/www/projectlamp/index.php

```

- You can always recreate this page if you need to access the information again later.

##  Troubleshooting

- Apache not starting?

- Permissions issues with /var/www/html?

- Security group not allowing HTTP (port 80)?

- MySQL connection denied?

## Security Considerations

- Don't expose SSH to 0.0.0.0/0

- Always use key-based SSH auth

- Secure MySQL with strong root password

- Use least privilege principle with users







