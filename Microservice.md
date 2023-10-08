# Creating Instances for Wordpress As well AS MySQL
# Choose an AMI in the classic instance wizard: I chose the Amazon ubuntu AMI
![image](https://github.com/JayDeep-Giri/infotrixs/assets/131878379/6d6ccb0a-13f5-4931-8abb-24a7c8f2c4a6)

# Instance details: Select the Instance Type you want to use. Here I am using t2.micro.
![image](https://github.com/JayDeep-Giri/infotrixs/assets/131878379/6be23025-92ef-4176-a375-4e8d00b0885f)

# Create A security group allowing all traffic for now.
![image](https://github.com/JayDeep-Giri/infotrixs/assets/131878379/7fc3b79f-da01-400b-8708-d3fe4aa4d499)


# Create a new key or choose already existing key(If any).
# Launch your instance.
# Note: This setup is common for both the instances. Now, for setting up wordpress and mysql we need to ssh login to the system for that find note the Public IP and key.

# Setting Up MySQL first:
Step-1: ssh login:
ssh -l ec2-user -i ubuntu-key.pem your-public-ip
where ec2-user is the by default user already created in the Amzon AMI. And key assigned to the instance and the public Ip of your instance. Do check your own before doing this.

Step-2: Switch to root user:
As while setting up it needs some root power. Use command:

sudo su - root
Step-3: Install mysql-server
Your WordPress installation needs to store information, such as blog posts and user comments, in a database. This procedure helps you create your blog's database and a user that is authorized to read and save information to it. To install it..

yum install mysql57-server -y
Step-4: Start mysqld service
service mysqld start
This command will start mysql daemon or can say mysql server.

Step-5: Secure Database server

## Run mysql_secure_installation to secure the database using following command:
mysql_secure_installation

When prompted, type a password for the root account.

Type Y to set a password, and type a secure password twice.
Type Y to remove the anonymous user accounts.
Type N to enable the remote root login.
Type Y to remove the test database.
Type Y to reload the privilege tables and save your changes.
Step-6: Create an User and Database for Wordpress
#login to mysql first
>> mysql -u root -p
>> CREATE USER 'wordpress-user'@'localhost' IDENTIFIED BY 'abc@123';
Note: "abc@123" is a password I set for wordpress-user. You can set your own.

>> CREATE DATABASE `wordpress-db`;
>> GRANT ALL PRIVILEGES ON *.* TO 'wordpress-user'@'%' IDENTIFIED BY 'abc@123' WITH GRANT OPTION;
>> FLUSH PRIVILEGES;
The above three commands will create a database with name wordpress-db and will grant the privileges for database to wordpress-user even if we login remotely.

Step-7: Enable mysqld service
Use the chkconfig command to ensure that the database services start at every system boot.

chkconfig mysqld on

Setting Up Wordpress:
Step-1: SSH login to Wordpress's Instance:
ssh -l ec2-user -i key3.pem 13.232.69.68
Step-2: Switch user to root:
sudo su - root
Step-3: Download and Extract wordpress package:
## Download the latest WordPress installation package with the wget command and Unzip and unarchive the installation package. The installation folder is unzipped to a folder called wordpress.

wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
Step-4: Install Apache server and Php:
yum install -y httpd24 php72 php72-mysqlnd
Step-5: Copy website files to /var/www/html:
cp -r wordpress/* /var/www/html/
Step-6: Edit wp-config.php file:
## Copy the wp-config-sample.php file to a file called wp-config.php. This creates a new configuration file and keeps the original sample file intact as a backup. And edit the same using vim command;

cd /var/www/html
cp wp-config-sample.php wp-config.php
vim wp-config.php
Edit the host, database name, user and password as shown in the following:
![image](https://github.com/JayDeep-Giri/infotrixs/assets/131878379/006a3d0a-94b4-460b-86cc-f8e5b86d49bb)


In the same file find the section called Authentication Unique Keys and Salts. Visit https://api.wordpress.org/secret-key/1.1/salt/ to randomly generate a set of key values that you can copy and paste into your wp-config.php file as follows:
![image](https://github.com/JayDeep-Giri/infotrixs/assets/131878379/9219635e-a8da-47fa-81eb-06076f48ade6)

Step-7: Start apache server:
service httpd start
Step-8: Enable httpd service:
Use the chkconfig command to ensure that the httpd services start at every system boot.

chkconfig httpd on
- - - - - - - -

Complete setup is now done, browse with your public IP. And Wordpress will automatically connect to the MySQL server on another EC2.
