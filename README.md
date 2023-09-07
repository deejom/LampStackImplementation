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
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/23fd3df8-aad3-4fa1-876a-4a1658cabde3)
- Running apache2 package installation - "sudo apt install apache2"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/b49ea368-b4eb-4ca6-aa8f-6301b6d29fc8)
- Confirming if apache2 is running on the OS - "sudo systemctl status apache2"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/5a112de6-8bac-4776-b1c8-fa4ab324a58d)
- Adding rule to EC2 configuration to open inbound connection via port 80
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/37caa3ea-d013-40a8-bb0a-8cf8fd4b314f)
- Checking how to access it locally in our Ubuntu shell - "curl http://localhost:80" or "curl http://127.0.0.1:80"
![image](https://github.com/deejom/LampStackImplementation/assets/141459374/0503e283-1b70-40b8-bb33-386784055503)
- Testing how Apache HTTP server can respond to requests from the internet, open any web browser and run "http://public-IP-Address:80" e.g. 


  
