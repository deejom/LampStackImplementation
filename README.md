# LampStackImplementation
## Building and deploying web applications using the LAMP stack

### STEP 0: THE PREREQUISITES

- [Registering and setting up an AWS free-tier account with Ubuntu Server OS via EC2 (Elastic Compute Cloud)](https://www.youtube.com/watch?v=xxKuB9kJoYM&list=PLtPuNR8I4TvkwU7Zu0l0G_uwtSUXLckvh&index=7)

- [Connecting to your EC2 Instance](https://www.youtube.com/watch?v=TxT6PNJts-s&list=PLtPuNR8I4TvkwU7Zu0l0G_uwtSUXLckvh&index=8)

- Connecting to EC2 
  ![image](https://github.com/deejom/LampStackImplementation/assets/141459374/9f913280-5bef-45d6-ab1d-6eaf101ae81c)

  a. Use of PEM key to connect to EC2 instance via ssh
  
  b. Installation of Windows Terminal tool

  c. Run "cd Downloads"

  d. Connect to the instance by running "ssh -i file_name.pem ubuntu@ip_address" e.g. ssh -i ec2-key.pem ubuntu@18.130.7.33
  ![image](https://github.com/deejom/LampStackImplementation/assets/141459374/6a523d3b-5d9e-4c13-a7cc-c68baf8b88f7)


### STEP 1: INSTALLING APACHE AND UPDATING THE FIREWALL

- Updating the list of packages in the package manager in Ubuntu - "sudo apt update"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/e3738ef7-a242-4e17-bbfe-2427d7e25a1b)

- Running apache2 package installation - "sudo apt install apache2"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/0390358d-0681-4c5f-a8ab-4898aa489724)

- Confirming if apache2 is running on the OS - "sudo systemctl status apache2"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/73895ae0-69d7-4cb4-b193-58a7f4303a34)

- Adding rule to EC2 configuration to open inbound connection via port 80
![image](https://github.com/yemikareem/LampStackImplementation/assets/141459374/8146c912-0f19-4114-9851-0262d13a2457)
![image](https://github.com/yemikareem/LampStackImplementation/assets/141459374/f686a84e-35dd-43f7-9cb0-6f9205e5f90c)

- Checking how to access it locally in our Ubuntu shell - "curl http://localhost:80" or "curl http://127.0.0.1:80"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/5bde721a-70eb-4314-ae28-9f7ec7f6717b)

- Testing how Apache HTTP server can respond to requests from the internet, open any web browser and run "http://public-IP-Address:80" e.g. "http://13.40.132.198/" (with or without :80 works)
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/4640b5cb-e113-43cf-90bb-68de5249f9a2)


### STEP 2: INSTALLING MYSQL 

- Inquire and install MySql - "sudo apt install mysql-server"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/2ff148b5-3a0a-4094-b398-89d46a8ca44a)

- Log in to the MySQL console - "sudo mysql"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/ea32daab-c3ec-4357-983a-c39b19130db5)

- Setting password for the root user - "ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/d9c83b5b-f8ae-4248-8731-d881ec3b0276)

- Exiting MySQL shell - after "mysql>", just type "exit" and hit ENTER
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/599419fb-4557-4384-a382-18d039409c58)

- Starting interactive script - "sudo mysql_secure_installation"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/6764ea81-8f87-4374-b2fd-6467b638749f)
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/201ffbc8-f729-470f-8f96-feec9e9fe3d7)

- Testing if you are able to access the MySQL console - "sudo mysql -p"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/62fea4e1-96eb-4e17-9f09-2344b115410f)

NB: For security reasons, the best is to have dedicated user accounts with less priviledges, especially if there are multiple databases hosted on the server. 


### STEP 3: INSTALLING PHP

Quick Tip: Linux is installed to serve as work environment, Apache is installed to serve the content, MySQL is installed to store and manage data, and PHP processes code to display dynamic content to end user

In addition to the PHP package, we'll need: 
1. php-mysql: a PHP module that helps PHP to communicate with MySQL-based databases
2. libapache2-mod-php: enables Apache to handle PHP files 

- To install these 3 packages at once - "sudo apt install php libapache2-mod-php php-mysql"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/ed232712-f42a-4327-8323-0912210043ff)

- To confirm the PHP version - "php -v"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/0afc0f8d-15eb-40d1-996a-d1bb2c675041)

At this point, LAMP stack is completely installed and fully operational:   
  - Linux (Ubuntu) 
  - Apache HTTP Server 
  - MySQL
  - PHP


### STEP 4: CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE 

- To create a directory for ProjectLAMP - "sudo mkdir /var/www/projectlamp"
- Assign ownership of the directory - "sudo chown -R $USER:$USER /var/www/projectlamp"
- Open and create a new configuration in Apache's site-available - "sudo vi /etc/apache2/sites-available/projectlamp.conf"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/4639d1a4-bc38-4d44-8d98-9adf4152523e)

This creates a new blank file. Paste the following by hitting on "i" on the keyboard: 
<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/007b98ca-8a79-4a00-b139-2468f27faecf)

- To save and close the file: 
    1. Hit the "esc" button on the keyboard
    2. Type ":"
    3. Type "wq", w for write, q for quit
    4. Hit "ENTER" to save the file

- To check the new file in the site-available directory - "sudo ls /etc/apache2/sites-available"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/49fbd5ce-9f61-4b65-a9a0-be4aab032497)

- To enable the new virtual host - "sudo a2ensite projectlamp"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/57794402-b3e2-4f98-b722-4f2b6490e4a7)

- To disable the default website that comes installed with Apache - "sudo a2dissite 000-default"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/177435ed-5a25-44e1-bd8b-0383c5d471c2)

- To ensure the configuration file does not contain syntax error - "sudo apache2ctl configtest"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/53d00b97-c083-410a-b443-52705c3573ea)

- To reload Apache such that these changes may take effect - "sudo systemctl reload apache2"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/e4a29040-74e0-40d0-921a-d3b6deda0566)

At this point, the website is active, but the web root/var/www/projectlamp is still empty

- To create an index html file in that location to test if the virtual host works - "sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/52e91eb2-3987-44a7-bdf3-13facdf3ecf9)

- Test the website by opening any web browser and run "http://public-IP-Address:80" e.g. "http://13.40.132.198/".
In the output, the public hostname (DNS name) and pubic IP address can be seen
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/821d1e02-b59b-419b-b543-f64a0d15446d)

- The public DNS name can also be used to access the website in the browser
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/bd17fef2-a3fb-47c4-b8f2-3435720cc80a)
  NB: This can be left in place temporarily as a landing page until index.php is set up for replacement


### STEP 5: ENABLING PHP ON THE WEBSITE 

Because of the default DirectoryIndex on Apache, any file named index.html will always take precedence over the index.php file. We can use this to set up maintenance pages on PHP applications by creating a temporary index.html file containing informative messages to visitors. And because this page will take precedence over the index.php page, it will become the application's landing page. Once the maintenance is over, the index.html is renamed or removed from the document root, bringing back the regular application page.

- To change this behaviour, edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive - "sudo vim /etc/apache2/mods-enabled/dir.conf"
![image](https://github.com/yemikareem/LampStackImplementation/assets/141459374/f2fe9ce7-8235-4fdc-88d2-bc8bc85c6e07)
<IfModule mod_dir.c>

  #Change this:

  #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm

  #To this:

  DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm

</IfModule>

- After saving and closing the file, reload Apache so that the changes can take effect - "sudo systemctl reload apache2"
![image](https://github.com/yemikareem/LampStackImplementation/assets/141459374/d981a8cf-4b5e-4359-86c7-a0d0f16548a7)

- Creating a PHP script to test PHP correct installation and configuration on the server, and to confirm that Apache is able to handle process requests for PHP files. 
1. Create a new file named index.php inside your customer web root folder - "vim /var/www/projectlamp/index.php"
![image](https://github.com/yemikareem/LampStackImplementation/assets/141459374/a9d0ebc4-af91-4964-8ac9-ff85d26cc273)

2. Add the following text, which is a valid PHP code inside the file -

"<?PHP"

"phpinfo();"
![image](https://github.com/yemikareem/LampStackImplementation/assets/141459374/36dd67d5-daf0-4a76-89c4-b9ca268ca397)

3. When done, save and close the file. Refresh the page, and the below will load up:  
![image](https://github.com/yemikareem/LampStackImplementation/assets/141459374/6d0e827a-f7e6-4e9c-b7cf-993a622502ae)



LampStackImplementation Project Completed! (c) Yemi Kareem, 2023. 
