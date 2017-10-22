# Linux-Server-Config
#### Project Deployment Link: http://saurabhjain.tk/sports/ , IP: http://139.59.77.21/ SSHPort:2200

## Project 6 of FSND
This project is linked to the Configuring Linux Web Servers, which teaches  to secure and set up a Linux server. 
In this Project we will deploy the Flask Postgres Apache server.

### 1. Let's Start with Ubuntu Linux server instance on Digital Ocean.
* Create a new Droplet on Digital Ocean with following config.
![Imgur](https://i.imgur.com/KNHH7Gh.png)
##### Distribution - Ubuntu 16.04 x64

![Imgur](https://i.imgur.com/kIOsgRr.png)
##### Size - $5/mo

![Imgur](https://i.imgur.com/T8JLBA6.png)
##### Datacenter region - Banglore

* Create a new ssh-key called sports_root .
save the key `/Users/YOUR_USER_NAME/.ssh/catalog_root` where YOUR_USER_NAME is your username.
* Set up a passphrase, I have set it to "hello". It can be of your choice.
![Imgur](https://i.imgur.com/i2r0cTW.png)
* Next, Add the key by copying the contents of sports_root.pub .
![Imgur](https://i.imgur.com/dAsXVNR.png)
* Finish on create button, you have set up your droplet !!.
![Imgur](https://i.imgur.com/QItROcm.png)

* ssh into the server with command.
```
ssh -i ~/.ssh/sports_root root@139.59.77.21
```
Here `139.59.77.21` is my IP.

Enter the passphrase for the ssh key that you created, for me it was "hello".
![Imgur](https://i.imgur.com/4XhuqRf.png)

##  Update the packages

* To update the packages run

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install unattended-upgrades
sudo unattended-upgrades
```
* Next, To change ssh port to 2200
```
sudo nano /etc/ssh/sshd_config
```
* Change the port no from 20 to 2200

* Restart ssh service.

```sudo service ssh restart```

* Next Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).
```
sudo ufw allow 2200
sudo ufw allow 80
sudo ufw allow 123
sudo ufw enable
```
* To create a user called grader
```
sudo adduser grader
```
Enter the user information for grader
![Imgur](https://i.imgur.com/Xjs2P2U.png)

* To give grader permission create grader file inside sudoers.d.
```
touch /etc/sudoers.d/grader
```
Next open 
```
sudo nano /etc/sudoers.d/grader
```
and add the following line
```
grader ALL=(ALL) NOPASSWD:ALL
```

## Create an SSH key pair for grader using the ssh-keygen tool.

Repeat steps in Step 1 (similar to root) to create a new ssh key at `~/.ssh/sports_grader`.
Run the following commands next 

```
su - grader
mkdir .ssh
chmod 700 .ssh
nano .ssh/authorized_keys
# paste the content of sports_grader.pub file and save the file
chmod 644 .ssh/authorized_keys
```
* Restart service and try logging in with grader
```
sudo service ssh restart
ssh -p 2200 -i ~/.ssh/sports_grader grader@139.59.77.21
```

![Imgur](https://i.imgur.com/ypcdH2f.png)

## Congfigure timezone 
```
sudo dpkg-reconfigure tzdata
```

![Imgur](https://i.imgur.com/iMta7iz.png)


* Install and configure Apache to serve a Python mod_wsgi application.
```
sudo apt-get install python3
sudo apt-get install python3-setuptools
sudo apt-get install apache2 libapache2-mod-wsgi-py3
```
* Restart the service 

```
sudo service apache2 restart
```
* Install the POSTGRES
```
sudo apt-get install postgresql
```

* Disable remote connections remove other IPs besides 127.0.0.1
```
sudo nano /etc/postgresql/VERSION_NUMBER/main/pg_hba.conf
```
VERSION_NUMBER here was 9.5

## Create a databse 

```
create user catalog with password 'hello';
create database catalog with owner catalog;
```
![Imgur](https://i.imgur.com/mGpgo2u.png)

press `\q` to exit and install git

```
sudo apt-get install git
```

 ## Clone and setup the Item Catalog project  Next move to directory using
```
cd /var/www
sudo mkdir catalog
cd catalog
sudo git clone https://github.com/saurabhjn76/ItemCatalog
```
Install pip and the project dependencies
```
sudo apt-get install python-pip
```
Project requirements 
```
sudo pip3 install Flask==0.12.2
sudo pip3 install SQLAlchemy==1.1.11
sudo pip3 install Flask-SQLAlchemy==2.2
sudo pip3 install Flask-Login==0.4.0
sudo pip3 install equests-oauthlib==0.8.0
sudo pip3 install psycopg2==2.7.1
sudo pip3 install oauth2client
```

## Create a configuration file for your project.
```
sudo nano /etc/apache2/sites-available/catalog.conf
```
Add the following code and save the file
```
<VirtualHost *:80>
    ServerName 139.59.77.21

    WSGIScriptAlias / /var/www/catalog/wsgi.py

    <Directory /var/www/catalog>
        Order allow,deny
        Allow from all
    </Directory>
</VirtualHost>
```

* Add a wsgi file in catalog folder
```
import sys

sys.path.insert(0, '/var/www/catalog/ItemCatalog')

from ItemCatalog import app as application

application.secret_key = 'New secret key.'
```

* Enable the site and restart apache service
```
sudo a2ensite catalog  # enable site
sudo service apache2 reload
```
This will make your app working on your ip check (http://139.59.77.21/) error in logs if any.
```
sudo cat /var/log/apache2/error.log
```

* Since google oauth dosen't works with only IP. Setup subdomain and link your ip to it.

## Change configs in google OAuth 2.0

configure your OAuth 2.0 credential and update authorized origin and redirect URIs with your subdomain

##  Disbale password based authentication and Root login
In config file
```
sudo nano /etc/ssh/sshd_config
```
Change
```
PermitRootLogin no 
PasswordAuthentication no
```
Save it and restart ssh service
```
sudo service ssh restart
```









 





