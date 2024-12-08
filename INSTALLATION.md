# MRBS Installation Guide

This guide provides step-by-step instructions to set up and configure the Meeting Room Booking System (MRBS).

---
### **Step 1: Set Up the Server Environment**

#### 1. **Install PHP**

- Update the package manager:
     ```  
    sudo apt update sudo apt upgrade
``
- Install PHP and required extensions:

 ```  
 sudo apt install php php-mysql php-pgsql php-gd php-curl php-xml php-mbstring unzip -y 
```
- Verify PHP installation:

`php -v`

#### 2. **Install MySQL 

- For **MySQL**:
```
sudo apt install mysql-server -y sudo mysql_secure_installation
```

- Log in to MySQL and create a database for MRBS:

```
mysql -u root -p CREATE DATABASE mrbs; 
CREATE USER 'mrbs_user'@'localhost' IDENTIFIED BY 'password'; 
GRANT ALL PRIVILEGES ON mrbs.* TO 'mrbs_user'@'localhost'; 
FLUSH PRIVILEGES;
EXIT;
```

#### 3.**Install a Web Server**
```
sudo apt install apache2 libapache2-mod-php -y 
sudo systemctl enable apache2 
sudo systemctl start apache2
```

### **Step 2: Download and Configure MRBS**

#### 1. **Download MRBS**

- Navigate to the web root directory:
```
cd /var/www/html
```
Download MRBS from its official source (GitHub or similar):

```
https://sourceforge.net/projects/mrbs/
```
Extract the files:
```
tar -xvzf mrbs-1.11.6.tar.gz
```
#### 2. **Set Permissions**

- Assign appropriate ownership and permissions:
```
sudo chown -R www-data:www-data /var/www/html/mrbs sudo chmod -R 755 /var/www/html/mrbs
```
#### 3. **Configure MRBS**

- Navigate to the MRBS directory:
```
cd /var/www/html/mrbs/web 
sudo cp config.inc.php-sample config.inc.php
```
Edit the configuration file:
```
sudo nano config.inc.php
```
Update database settings:
```
$dbsys = "mysql"; // or "pgsql" for PostgreSQL 
$db_host = "localhost"; 
$db_database = "mrbs"; 
$db_login = "mrbs_user"; 
$db_password = "password";
```

### 4.**Set the `$timezone` Variable**

- Set the `$timezone` variable to a valid PHP timezone string. For example:
```
$timezone = "Asia/Singapore"; // Adjust based on your location
```

Import the SQL file into the database:

- For **MySQL**:
```
mysql -u mrbs_user -p mrbs < /var/www/html/mrbs/tables.my.sql

```
### **Step 3: Configure the Web Server**

#### 1. **Set Up Apache (Example)**

- Create a configuration file:
```
sudo nano /etc/apache2/sites-available/mrbs.conf
```
Add the following content:
```
<VirtualHost *:80>
    ServerAdmin admin@example.com
    DocumentRoot /var/www/html/mrbs/web
    ServerName yourdomain.com

    <Directory /var/www/html/mrbs/web>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

```
Enable the site and restart Apache:
```
sudo a2ensite mrbs.conf sudo systemctl reload apache2
```

### **Step 4: Complete the Installation**

1. Open a browser and navigate to your server’s IP or domain:
```
http://yourdomain.com
```
2. Follow any on-screen instructions for final setup.
3. Log in and configure meeting rooms and settings.