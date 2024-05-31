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

I accessed my apache HTTP server on port 80 locally and test if it responded to request from the internet using
curl http://localhost:80
http://my-public-ip-address:80

**Step 4 Installing MySQl** I run the following commands to install MySQl

                    sudo apt install mysql-server


I log in to the mysql console using this command

                    sudo mysql

                    

