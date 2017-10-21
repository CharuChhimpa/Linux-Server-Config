# Linux-Server-Config
Project Deployment Link: http://saurabhjain.tk/sports/ , IP: http://139.59.77.21/

Project 6 of FSND
This project is linked to the Configuring Linux Web Servers, which teaches  to secure and set up a Linux server. 
In this Project we will deploy the Flask Postgres Apache server.

* Let's Start with Ubuntu Linux server instance on Digital Ocean.
Create a new Droplet on Digital Ocean with following config.
![Imgur](https://i.imgur.com/KNHH7Gh.png)
Distribution - Ubuntu 16.04 x64
![Imgur](https://i.imgur.com/kIOsgRr.png)
Size - $5/mo
![Imgur](https://i.imgur.com/T8JLBA6.png)
Datacenter region - Banglore

Create a new ssh-key called sports_root .
save the key `/Users/YOUR_USER_NAME/.ssh/catalog_root` where YOUR_USER_NAME is your username.
Set up a passphrase, I have set it to "hello". It can be of your choice.
![Imgur](https://i.imgur.com/i2r0cTW.png)
Next, Add the key by copying the contents of sports_root.pub .
![Imgur](https://i.imgur.com/dAsXVNR.png)
Finish on create button, you have set up your droplet !!.
![Imgur](https://i.imgur.com/QItROcm.png)

* ssh into the server with command.
```
ssh -i ~/.ssh/sports_root root@139.59.77.21
```
Here `139.59.77.21` is my IP.

Enter the passphrase for the ssh key that you created, for me it was "hello".
![Imgur](https://i.imgur.com/4XhuqRf.png)

To update the packages run

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install unattended-upgrades
sudo unattended-upgrades
```





