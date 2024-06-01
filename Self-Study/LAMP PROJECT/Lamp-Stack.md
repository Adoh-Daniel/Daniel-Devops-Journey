## DEPLOYING A LAMP STACK WEBSITE IN AWS CLOUD (My Journey)

**THE LAMP STACK** is a popular Open-source web development stack that consists of **Linux**(Operating system), **Apache**(Web server), **MySQL**(Data Management System), **PHP**(Server-side scripting Language), they usually represents an alternative to expensive commercial packages. This documentation outlines the steps i took in the set-up, configuration, and implementation of Lamp stack in AWS Cloud.

**Step 0: Preparing Prerequisites** An Ec2 Instance of t3.micro type and ubuntu 24.04 LTS (HVM) was launched in the us-east-1 region using the AWS console.

**Step 1 Configure Security Group** My Security group was configured with the following inbound rules. SSH (port 22) was created by default. Then i changed the source from custom to anywhere. Then I added and allow traffic on port 80 (HTTP) with source from anywhere on the internet. Also, added and allow traffic on port 443 (HTTPS) with source from anywhere on the internet.

**Step 2 Connecting to my AWS Ec2 instance via ssh**. To achieve this i made sure i downloaded and name my-private-key.pem file correctly, which was in my windows download folder. Then, I change my directory using `cd /mnt/c/users/hp/Downloads`, I used the following commannds to ssh into my Ec2 instance. `ssh chmod 400 "Danec2.pem"` Note that this will ask for password, then `ssh -i "Danec2.pem" ubuntu@ec2-16-170-212-214.eu-north-1.compute.amazonaws.com` which i was successfully connected to my ec2 instance.

**Step 3 Installing Apache and updating firewall**. I updated all list of packages in my package manager, then i run apache package installation using 

                    sudo apt update

                    sudo apt install apache2

To verify apache is running as a service, it was green and running which indicated I have installed apache successfully

                    sudo systemctl status apache2

![IMG_20240526_022324](https://github.com/Adoh-Daniel/Daniel-Devops-Journey/assets/169608648/37392619-cc9a-4258-9397-c889f1f1ad23)

I accessed my apache HTTP server on port 80 locally and test if it responded to request from the internet using
curl http://localhost:80

http://my-public-ip-address:80

![IMG_20240527_215715](https://github.com/Adoh-Daniel/Daniel-Devops-Journey/assets/169608648/1263cd2b-d36f-4a39-93fc-d864f994fc75)

**Step 4 Installing MySQl:** I run the following commands to install MySQl

                    sudo apt install mysql-server


I log in to the mysql console using this command

                    sudo mysql

Then I set a password for root user using mysql_native_password as default authentication method. And my user's password was defined as "DeeDan000$"

                    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY "DeeDan000$";

I exited mysql shell with

                    exit

Now, regardless whether the VALIDATION PASSWORD PLUGIN is set up, the server will ask to select and confirm a password for mysql root user.

A command propmt for password was noticed after running this command below

                    sudo mysql -p

**Step 5 Installing PHP:** 3 package was install at once using this command

                    sudo apt install php libapache2-mod-php php-mysql

![IMG_20240526_093816](https://github.com/Adoh-Daniel/Daniel-Devops-Journey/assets/169608648/0306c757-917e-44f5-87f4-ba4ba6af1038)


Once installation was complete, i use this command to confirm my PHP version

                    php -v

**Step 6 Creating a virtual host for the website using apache:** 

The default directory serving the apache default page is /var/www/html. I created my document directory next to the default one using "mkdir" command

                    sudo mkdir /var/www/projectlamp

Assigned the directory ownership with $USER environment variable which references the current system user.

                    sudo chow -R $USER:$USER /var/www/projectlamp

Then, I created a new configuration file in apache "sites-available" directory using vim.

                    sudo vim /etc/apache2/sites-available/projectlamp.conf


This will create a blank file, then I pasted the following bare-bone configuration and then save the file by pressing `ctrl + ] and then :wq`

                    <VirtualHost *:80>
                        ServerName projectlamp
                        ServerAlias www.projectlamp
                        ServerAdmin webmaster@localhost
                        DocumentRoot /var/www/projectlamp
                        ErrorLog ${APACHE_LOG_DIR}/error.log
                        CustomLog ${APACHE_LOG_DIR}/access.log combined
                    </VirtualHost>

Now, I use this `a2dissite` command to enable new virtual host and to disable apache's default website 

                    sudo a2dissite projectlamp
                    sudo a2dissite 000-default

Then I ensure the configuration does not contain syntax error and also reload so that changes takes effect using this commands

                    sudo apache2ctl configtest
                    sudo systemctl reload apache2

Open the website on a browser using public IP address. Note that this file can be left in place as a temporary landing page for the application until an index.php file is set to replace it. Once tis is done, the index.html file should be renamed or removed from the document root as it willl take precedence over index.php file by default. 

**Enable PHP on website**

1. Open a `dir.conf` file with `vim` to change the behaviour using this command `sudo vim /etc/apache2/mods-enabled/dir.conf`

2. Reload apache using `sudo systemctl reload apache2`

3. Create a PHP test script to confirm that apache is able to handle and process requests for PHP files. A new index.php file was created inside the custom web root folder.

4. Then, refresh the page. The page provides information about the server from the perspective of PHP. It is also useful in debugging and to ensure the settings are applied correctly. Note that it can also be recreated if the information is needed in the future and it is advisable to remove the file after going throuh necessary information. To remove file I use this command

                          sudo rm /var/www/projectlamp/index.php

**Conclusion**

The LAMP stack provides a wide and flexible platform for developing and deploying Web Applications. By following the detailed guidelines in this documentation, it shows it was possible to set-up, configure, and maintain a LAMP environment successfully, which enables the creation of an immersive web application. THANK YOU.


   
   


            



