# Implementing Loadbalancers with Nginx.
## Setting up a Basic Load Balancer.
#### step 1: Provisioning two EC2 instance running on ubuntu. Opening port 8000 to allow traffic from anywhere.
![Screenshot from 2023-10-09 22-24-52](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/e86b9ab1-7cf0-4309-a346-2de48fb0b499)

![Screenshot from 2023-10-09 22-02-10](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/25c514ed-8d2a-4a74-97f8-78dca7cb668d)

#### Step 2: Installing apache webservers on them.
sudo apt update -y &&  sudo apt install apache2 -y
![Screenshot from 2023-10-09 22-34-16](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/a326475f-11e9-4d96-955a-0b506ff0e2c0)
#### Verify that apache is running:
sudo systemctl status apache2
![Screenshot from 2023-10-09 22-37-31](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/dde41d57-61e7-4276-ac91-7ea0a71475be)

#### Step 3: Configuring apache to serve a page showing it's IP address.
sudo vi /etc/apache2/ports.conf 
#### Adding a new Listen directive for port 8000.
![Screenshot from 2023-10-09 22-50-55](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/52ca9d28-081d-4c44-8fb9-f2a974ba6e66)

#### Step 4: Adding the listen directive and saving  file.
##### Open the file /etc/apache2/sites-available/000-default.conf and change port 80 on the virtualhost to 8000.
sudo vi /etc/apache2/sites-available/000-default.conf
![Screenshot from 2023-10-09 23-17-00](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/d77e3a93-8cb1-4fc9-9788-62649d564479)

#### Step 5: Restarting apache to load the new configuration.
sudo systemctl restart apache2
![Screenshot from 2023-10-09 23-22-20](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/4e22736d-f447-4e4d-913d-36bb547ad7bd)

### Creating a new Html file.
#### 1. Creating a new Html file and insert the html file below . Before pasting the html file replacing the placeholder text for IP address in the html file with the IP address of the Instances.
sudo vi index.html
![Screenshot from 2023-10-09 23-32-07](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/2b972b5d-162d-48eb-a279-9871ffe43103)

#### 2. Changing file ownership of the index.html.
sudo chown www-data:www-data ./index.html
![Screenshot from 2023-10-09 23-45-07](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/e184c345-e2ae-44fd-9970-564fe6ff0777)

### Overriding the Default html file of Apache Webserver.
#### 1. Replace the default html file with our new html file using the command below.
sudo cp -f ./index.html /var/www/html/index.html
![Screenshot from 2023-10-09 23-49-52](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/f8ca5402-262a-4be4-ab93-cebcdd3b27d6)

#### 2. Restart the webserver to load the new configuration using the command below.
sudo systemctl restart apache2
![Screenshot from 2023-10-09 23-52-40](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/033863e6-6505-484d-a59c-747084ccc065)

#### 3. Confirming status from the web.
![Screenshot from 2023-10-09 23-54-11](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/95192b63-9e6b-42ba-801f-a7683644769c)

![Screenshot from 2023-10-09 23-54-44](https://github.com/PromiseNwachukwu/Implementing-Loadbalancers-with-Nginx/assets/109115304/cf4ca0fa-5e68-4da0-9020-d8ecd70d0005)



