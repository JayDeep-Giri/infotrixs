1. Setting Up an AWS EC2 Instance
To begin, follow these steps to set up an AWS EC2 instance:

Log in to your AWS Management Console.
Navigate to the EC2 service and click on “Launch Instance.”
Choose an appropriate Ubuntu AMI for your EC2 instance.
![image](https://github.com/JayDeep-Giri/infotrixs/assets/131878379/15105080-008f-4a3a-b4a9-d88800b06a97)

Select the instance type and configure the desired specifications.
Configure storage and add any additional tags if required.
Configure security groups to allow the necessary inbound traffic for SSH (port 22) and HTTP (port 80).
2. Connecting to the EC2 Instance via SSH
To connect to your EC2 instance, you will need to use SSH. Follow these steps:

Retrieve the key pair you created during the instance setup.
Open your preferred terminal or command prompt.
Change the key pair’s permissions using the following command:
chmod 400 /path/to/your-key-pair.pem
4. Connect to the instance using the SSH command and your key pair:

ssh -i /path/to/your-key-pair.pem ubuntu@ec2-instance-public-ip
3. Installing LAMP Stack on Ubuntu
To install the LAMP stack (Linux, Apache, MySQL, PHP) on Ubuntu, execute the following commands on your EC2 instance:

Update the package lists:
sudo apt update
2. Install the Apache web server:

sudo apt install apache2
3. Install the MySQL database server and secure it with a root password:

sudo apt install mysql-server
sudo mysql_secure_installation
4. Install PHP and the necessary models:

sudo apt install php libapache2-mod-php php-mysql
5. Restart Apache for the changes to take effect:

sudo systemctl restart apache2
4. Configuring the Database for WordPress
WordPress requires a database to store its content. Follow these steps to set up a MySQL database for WordPress:

Access the MySQL server as the root user:
sudo mysql -u root -p
2. Create a new database and user for WordPress:

CREATE DATABASE wordpress;
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress_user'@'localhost' IDENTIFIED BY 'your_password';
FLUSH PRIVILEGES;
EXIT;
5. Downloading and Installing WordPress
To download and install WordPress on your EC2 instance, execute the following commands:

Download the latest version of WordPress:
wget https://wordpress.org/latest.tar.gz
2. Extract the downloaded archive:

tar -xvf latest.tar.gz
3. Move the extracted files to the Apache document root:

sudo mv wordpress/* /var/www/html/
4. Set appropriate ownership and permissions:

sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/
6. Configuring WordPress
Configure WordPress with the database details and other settings:

Access your WordPress site through a web browser using your EC2 instance’s public IP address.
Select your preferred language and click “Let’s go.”
Enter the database details:
Database Name: wordpress
Username: wordpress_user
Password: (your chosen password)
Database Host: localhost
Table Prefix: (optional)
4. Click “Submit” and then “Run the installation.”

5. Provide the necessary information for your WordPress site, including the site title, username, password, and email address.

6. Click “Install WordPress” to complete the installation.

7. Log in to your WordPress admin dashboard using the created credentials.

7. Securing the WordPress Installation
To enhance the security of your WordPress installation, consider implementing the following measures:

Keep WordPress, themes, and plugins up to date.
Use strong and unique passwords for all user accounts.
Install security plugins, such as Wordfence or Sucuri, to add an extra layer of protection.
Limit login attempts and enable two-factor authentication.
Regularly back up your WordPress site and database.
Consider implementing a web application firewall (WAF) to protect against malicious traffic.
8. Conclusion
Congratulations! You have successfully deployed a WordPress website on an AWS Ubuntu VM. By following this step-by-step guide, you have set up an EC2 instance, installed a LAMP stack, configured the database, and deployed WordPress. Remember to implement security measures to safeguard your website and keep it up to date for optimal performance.
