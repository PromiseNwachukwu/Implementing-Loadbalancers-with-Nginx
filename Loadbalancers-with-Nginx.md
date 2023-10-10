# Implementing Load Balancer with Nginx.
## Setting up a Basic Load Balancer.
### step 1: Provisioning two EC2 instance running on ubuntu.
![Screenshot from 2023-10-09 22-24-52](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/e86b9ab1-7cf0-4309-a346-2de48fb0b499)

### step 2: Opening port 8000 to allow traffic from anywhere.
![Screenshot from 2023-10-09 22-02-10](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/25c514ed-8d2a-4a74-97f8-78dca7cb668d)

### Step 3: Installing apache webservers on the two instances.
sudo apt update -y &&  sudo apt install apache2 -y
![Screenshot from 2023-10-09 22-34-16](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/a326475f-11e9-4d96-955a-0b506ff0e2c0)
#### Verify that apache is running:
sudo systemctl status apache2
![Screenshot from 2023-10-09 22-37-31](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/dde41d57-61e7-4276-ac91-7ea0a71475be)

### Step 4: Configuring apache to serve a page showing it's IP address.
sudo vi /etc/apache2/ports.conf 
#### Configuring Apache to serve content from port 8000.
![Screenshot from 2023-10-09 22-50-55](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/52ca9d28-081d-4c44-8fb9-f2a974ba6e66)

#### Adding new listen directive for port 8000 and saving  file.
##### With tet editor, open the file /etc/apache2/sites-available/000-default.conf and change port 80 on the virtualhost to 8000.
sudo vi /etc/apache2/sites-available/000-default.conf
![Screenshot from 2023-10-09 23-17-00](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/d77e3a93-8cb1-4fc9-9788-62649d564479)

##### Restarting apache to load the new configuration.
sudo systemctl restart apache2
![Screenshot from 2023-10-09 23-22-20](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/4e22736d-f447-4e4d-913d-36bb547ad7bd)

#### Creating a new Html file.
##### 1. Creating a new Html file and insert the html file below and with a text editor, pasting the html file replacing the placeholder text for IP address in the html file with the IP address of the Instances.
sudo vi index.html
![Screenshot from 2023-10-09 23-32-07](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/2b972b5d-162d-48eb-a279-9871ffe43103)

##### 2. Changing file ownership of the index.html.
sudo chown www-data:www-data ./index.html
![Screenshot from 2023-10-09 23-45-07](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/e184c345-e2ae-44fd-9970-564fe6ff0777)

#### Overriding the Default html file of Apache Webserver.
##### 1. Replace the default html file with our new html file using the command below.
sudo cp -f ./index.html /var/www/html/index.html
![Screenshot from 2023-10-09 23-49-52](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/f8ca5402-262a-4be4-ab93-cebcdd3b27d6)

##### 2. Restart the webserver to load the new configuration using the command below.
sudo systemctl restart apache2
![Screenshot from 2023-10-09 23-52-40](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/033863e6-6505-484d-a59c-747084ccc065)

##### 3. Confirming status from the web on the two servers.
![Screenshot from 2023-10-09 23-54-11](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/95192b63-9e6b-42ba-801f-a7683644769c)

![Screenshot from 2023-10-09 23-54-44](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/cf4ca0fa-5e68-4da0-9020-d8ecd70d0005)


### Step 5: Configuring Nginx as a Load Balancer
##### Provisioning a new EC2 instance running ubuntu 22.04.
##### Open port 80 to accept traffic from anywhere.
![Screenshot from 2023-10-10 08-40-32](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/ed0ac3ed-6cbb-4c0c-8761-6726204f80f1)
![Screenshot from 2023-10-10 09-10-08](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/b08bd36a-a0a0-4f17-b483-881b04dffa44)


##### SSH into the instance.
##### Install Nginx into the instance using the command.
sudo apt update -y && sudo apt install nginx -y
![Screenshot from 2023-10-10 00-44-31](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/47a332dc-3668-4747-b4da-d42b0423eee4)
   
##### Verify that Nginx is installed with the command below:
sudo systemctl status nginx
![Screenshot from 2023-10-10 00-47-02](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/8f87a23f-06c5-41c7-9959-17a5bac030c5)

##### Open Nginx configuration file with the command below:
sudo vi /etc/nginx/conf.d/loadbalancer.conf
![Screenshot from 2023-10-10 08-35-44](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/5f918431-886c-4cba-8c2b-da0d503dd67b)

##### Test configuration with the command below:
sudo nginx -t
![Screenshot from 2023-10-10 00-54-13](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/f9eb0a17-af19-4925-8c7d-aae5145d8bb4)

##### Restart Nginx to laod the new configuration with the command below:
sudo systemctl restart nginx
![Screenshot from 2023-10-10 01-00-46](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/278d0081-efbd-42d1-ace4-14815eb3c552)

##### Accessing IP address of Nginx load balancer over the web. This serves the same webpages served by the webservers.
![Screenshot from 2023-10-10 08-35-14](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/7b2dbb39-ea23-40db-a306-a072e6152ae0)

##### Complete project.







