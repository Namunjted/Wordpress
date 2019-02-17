# **Install WordPress On Ubuntu 18.04 LTS With Apache2, MariaDB And PHP 7.1**

---
To get started with installing WordPress, follow the steps below:

### **Step 1: Install Apache2 HTTP Server**

```
sudo apt update
sudo apt install apache2
```
After installing Apache2, the commands below can be used to stop, start and enable Apache2 service to always start up with the server boots.

```
sudo systemctl stop apache2.service
sudo systemctl start apache2.service
sudo systemctl enable apache2.service
```
### **Step 2: Install MariaDB Database Server**
WordPress also need a database server… and MariaDB database server is a great place to start. To install it run the commands below.
WordPress needs a web server and Apache2 is a lightweight and powerful HTTP Server. To install Apache2 on Ubuntu, run the commands below:

```
sudo apt-get install mariadb-server mariadb-client
```
After installing, the commands below can be used to stop, start and enable MariaDB service to always start up when the server boots.

```
sudo systemctl stop mariadb.service
sudo systemctl start mariadb.service
sudo systemctl enable mariadb.service
```
After that, run the commands below to secure MariaDB server by creating a root password and disallowing remote root access.
```
sudo mysql_secure_installation
```
When prompted, answer the questions below by following the guide.

-  Enter current password for root (enter for none): Just press the **Enter**
- Set root password? [Y/n]: **Y**
- New password: **Enter password**
- Re-enter new password: **Repeat password**
- Remove anonymous users? [Y/n]:  **Y**
- Disallow root login remotely? [Y/n]: **Y**
- Remove test database and access to it? [Y/n]:   **Y**
- Reload privilege tables now? [Y/n]:  **Y**

Restart MariaDB server

```
sudo systemctl restart mariadb.service
```

### **Step 3: Install PHP 7.1 and Related Modules**

PHP 7.1 may not be available in Ubuntu default repositories… in order to install it, you will have to get it from third-party repositories.

Run the commands below to add the below third party repository to upgrade to PHP 7.1

```
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:ondrej/php
```

Then update and upgrade to PHP 7.1

```
sudo apt update
```
Finally, run the commands below to install PHP 7.1 and related modules.

```
sudo apt install php7.1 libapache2-mod-php7.1 php7.1-common php7.1-mbstring php7.1-xmlrpc php7.1-gd php7.1-xml php7.1-mysql php7.1-cli php7.1-mcrypt php7.1-zip php7.1-curl
```
### **Step 4: Create WordPress Database**

Now that you’ve install all the packages that are required, continue below to start configuring the servers. First run the commands below to create a blank WordPress database.

To logon to MariaDB database server, run the commands below.

```
sudo mysql -u root -p
```
Then create a database called wpdb

```
CREATE DATABASE wpdb ; 
```
Create a database user called wpdbuser with new password

```
CREATE USER `wpdbuser`@'localhost' IDENTIFIED BY 'password';
```
Then grant the user full access to the database.
```
GRANT ALL ON wpdb.* TO 'wpdbuser'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
```
Finally, save your changes and exit.

```
FLUSH PRIVILEGES;
EXIT;
```
### **Step 5: Download WordPress Latest Release**
Next, visit WordPress site and download the latest version….

After downloading, run the commands below to extract the downloaded file and move it into a new WordPress root directory.

```
cd /tmp && wget https://wordpress.org/latest.tar.gz
tar -zxvf latest.tar.gz
sudo mv wordpress /var/www/html/wordpress
```
Then run the commands below to set the correct permissions for WordPress to function.

```
sudo chown -R www-data:www-data /var/www/html/wordpress/
sudo chmod -R 755 /var/www/html/wordpress/
```
### **Step 6: Configure Apache2 HTTP Server**

Finally, configure Apache2 site configuration file for WordPress. This file will control how users access WordPress content. Run the commands below to create a new configuration file called wordpress.conf

```
sudo nano /etc/apache2/sites-available/wordpress.conf
```

Then copy and paste the content below into the file and save it. Replace the highlighted line with your own domain name and directory root location.

```
<VirtualHost *:80>
     ServerAdmin admin@example.com
     DocumentRoot /var/www/html/wordpress/
     ServerName example.com
     ServerAlias www.example.com

     <Directory /var/www/html/wordpress/>
        Options +FollowSymlinks
        AllowOverride All
        Require all granted
     </Directory>

     ErrorLog ${APACHE_LOG_DIR}/error.log
     CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```
Save the file and exit.

### **Step 7: Enable the WordPress and Rewrite Module**

After configuring the VirtualHost above, enable it by running the commands below… the commands also disable PHP7.0 and enable PHP 7.1 for Apache2.

```
sudo a2ensite wordpress.conf
sudo a2enmod rewrite
```

### **Step 8 : Restart Apache2**
To load all the settings above, restart Apache2 by running the commands below.

```
sudo systemctl restart apache2.service
```

### **STEP 9: CONFIGURE WORDPRESS**
Now that Apache2 is configured, run the commands below to create WordPress wp-config.php file.

```
sudo mv /var/www/html/wordpress/wp-config-sample.php /var/www/html/wordpress/wp-config.php
```
Then run the commands below to open WordPress configuration file.

```
sudo nano /var/www/html/wordpress/wp-config.php
```
Enter the highlighted text below that you created for your database and save.
```
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', 'wpdb');

/** MySQL database username */
define('DB_USER', 'wpdbuser');

/** MySQL database password */
define('DB_PASSWORD', 'password');

/** MySQL hostname */
define('DB_HOST', 'localhost');

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');
```
After that, open your browser and browse to your domain name to launch WordPress configuration wizard.

You should see WordPress setup wizard to complete. Please follow the wizard carefully.

[http://localhost/wordpress/](http://localhost/wordpress/)

![setup1](https://i1.wp.com/websiteforstudents.com/wp-content/uploads/2017/11/wordpress_ubuntu_wizard.png?w=816&ssl=1)

### **Step 10: Install phpMyAdmin**
Requirements for Installing phpMyAdmin:
* php 

```
 php -v
```
*MySQL or Mariadb

```
 mysql -v
```
Install phpMyAdmin from Ubuntu Packages.

```
sudo apt-get update
sudo apt-get install -y phpmyadmin
```
Configure phpMyAdmin Package

After installing phpMyAdmin, you will be presented with the package configuration screen.

Press the SPACE bar to place an “*” beside “apache2.”

Press TAB to highlight “OK,” then hit ENTER.

![setup1](https://www.hostingadvice.com/wp-content/uploads/2015/03/phpmyadmin-package-configuration-select-apache2.png)

The installation process will continue until you’re back at another package configuration screen.

Select “Yes” and then hit ENTER at the dbconfig-common screen:

![setup1](https://www.hostingadvice.com/wp-content/uploads/2015/03/phpmyadmin-package-configuration-dbconfig-common-select-yes.png)

You will be prompted for your database administrator’s password 

Type it in, hit TAB to highlight “OK,” and then press ENTER.

![setup1](https://www.hostingadvice.com/wp-content/uploads/2015/03/phpmyadmin-package-configuration-enter-db-admin-pass.png)

Next, enter a password for the phpMyAdmin application itself.

![setup1](https://www.hostingadvice.com/wp-content/uploads/2015/03/phpmyadmin-package-configuration-enter-phpmyadmin-pass.png)

Confirm the phpMyAdmin application password.

![setup1](https://www.hostingadvice.com/wp-content/uploads/2015/03/phpmyadmin-package-configuration-confirm-phpmyadmin-pass.png)

After the installation process completes, it adds the phpMyAdin configuration file here:

**/etc/apache2/conf-enabled/phpmyadmin.conf**

- Enable PHP mcrypt Module:

Check if the PHP mcrypt module is already in use:

```
php -m | grep mcrypt
```
If you don’t get any results, install the PHP mcrypt module with:

```
sudo php5enmod mcrypt
```
Now when we check, you should see mcrypt enabled:

```
$ php -m | grep mcrypt
mcrypt
```
- Restart Apache:

Now we should restart the Apache web server for changes to take affect:

```
sudo service apache2 restart
```
* Access phpMyAdmin 

You can assest [http://localhost/phpmyadmin/](http://localhost/phpmyadmin/)

![setup1](https://www.hostingadvice.com/wp-content/uploads/2015/03/phpmyadmin-login-as-root-user.png)

