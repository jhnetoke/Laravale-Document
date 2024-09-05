
#################### Ubuntu Server 22.04 ##################################

###################### PHP laravale Project Configer #######################

 
 ################## প্রথমে  Server Update  করতে হবে নিচের  Command দিয়ে ।
     
   apt update -y

################ Apache Install করতে হবে নিচের  Command দিয়ে , এবং এনাবেল করতে হবে যাতে  Server restart নেওয়ার পরে  Auto Apache Run হয়।

   apt install apache2 -y
   systemctl enable apache2

################## দেখার জন্য  Status Command
  systemctl status apache2


################## PHP Extension Install  করতে হবে  নিছে দেওয়া হল।

  apt install php php-common php-opcache php-mcrypt php-cli php-gd php-curl php-mysql php-xml php-pdo php-xmlrpc php-imap php-devel php-mbstring php-litespeed -y

  apt-get install ca-certificates apt-transport-https software-properties-common

  add-apt-repository ppa:ondrej/php

################## আবার সিষ্টমে আপডেট করতে হবে নিচের  Command  দিয়ে ।
   apt-get update

################## Install PHP8.3 নিচের  COmmand দিয়ে

  apt-get install php8.3
  php8.3 --version
  systemctl restart apache2

################## PHP-fpm  Install  করতে হবে

 apt install php8.3-{cli,fpm,curl,mysqlnd,gd,opcache,zip,intl,common,bcmath,imagick,xmlrpc,readline,memcached,redis,mbstring,apcu,xml,dom,memcache} -y
 apt install php8.3-fpm
 systemctl status php8.3-fpm
 apt install php8.3-mysql php8.3-imap php8.3-ldap php8.3-xml php8.3-curl php8.3-mbstring php8.3-zip
 update-alternatives --config php

systemctl restart php8.3-fpm.service

####################################################################################################

Datavase Server Install  করতে হবে এখনে আমরা   Latest Version Install  করতে ছি 

   apt install mariadb-server


   mysql_secure_installation

   mysql -u root -p
   systemctl enable mariadb
   systemctl restart mariadb
   systemctl start mariadb
   systemctl status mariadb
   mariadb -v

###################################################################### Compuser Install ############################

এখন আমরা composer composer  Install  করব প্রথমে Temp folder  এ প্রবেশ করব তার পরে নিচের  Command  দিতে   হবে।

root@hamko-ict-vm-01:~# cd /tmp/
root@hamko-ict-vm-01:/tmp# curl -sS https://getcomposer.org/installer | php
root@hamko-ict-vm-01:/tmp# ls
root@hamko-ict-vm-01:/tmp# mv composer.phar /usr/local/bin/composer
root@hamko-ict-vm-01:/usr/local/bin# ll
root@hamko-ict-vm-01:/usr/local/bin# composer

##########################################################################

root@hamko-ict-vm-01:~# cd /var/www/html/
root@hamko-ict-vm-01:/var/www# ll
   
root@hamko-ict-vm-01:/var/www# php -v

root@hamko-ict-vm-01:/var/www# composer create-project laravel/laravel example-app
root@hamko-ict-vm-01:/var/www# composer global require laravel/installer
root@hamko-ict-vm-01:/var/www# cd example-app
root@hamko-ict-vm-01:/var/www/example-app# ll

root@hamko-ict-vm-01:/var/www/example-app# php artisan serve
root@hamko-ict-vm-01:/var/www/example-app#mysql -u root -p
root@hamko-ict-vm-01:/var/www/example-app# ll
root@hamko-ict-vm-01:/var/www/example-app# vim .env
root@hamko-ict-vm-01:/var/www/example-app# php artisan migrate

================Database Create  করতে হবে ====================

root@hamko-ict-vm-01:/var/www/example-app# mysql -u root -p 

==================================== File Permission ========================

root@hamko-ict-vm-01:/var/www/example-app# ll
root@hamko-ict-vm-01:/var/www/example-app# chown -R www-data:www-data /var/www/example-app/storage/
root@hamko-ict-vm-01:/var/www/example-app# ll
root@hamko-ict-vm-01:/var/www/example-app# chown -R www-data:www-data /var/www/example-app/bootstrap/cache/
root@hamko-ict-vm-01:/var/www/example-app# ll
root@hamko-ict-vm-01:/var/www/example-app# systemctl restart apache2

root@hamko-ict-vm-01:# cd /var/www/
root@hamko-ict-vm-01:/var/www# sudo find example-app/ -type f -exec chmod 644 {} \;
root@hamko-ict-vm-01:/var/www# sudo find example-app/ -type d -exec chmod 755 {} \;

root@hamko-ict-vm-01:# cd /var/www/html/example-app
root@hamko-ict-vm-01:/var/www/example-app# chmod 777 storage/ -R
 
########################### PHP artisan cache clear Command ##################################

root@hamko-ict-vm-01:/var/www/example-app# php artisan cache:clear
root@hamko-ict-vm-01:/var/www/example-app# php artisan view:clear
root@hamko-ict-vm-01:/var/www/example-app# php artisan route:clear
root@hamko-ict-vm-01:/var/www/example-app# php artisan clear-compiled
root@hamko-ict-vm-01:/var/www/example-app# systemctl restart apache2
 composer clear-cache

=====================================================================================================

########################### PHP artisan cache & key:gen & composer update Command ##################################

root@hamko-ict-vm-01:/var/www/example-app# composer update --optimize-autoloader --no-dev
root@hamko-ict-vm-01:/var/www/example-app# php artisan key:gen
root@hamko-ict-vm-01:/var/www/example-app# php artisan migrate

root@hamko-ict-vm-01:/var/www/example-app# php artisan optimize
root@hamko-ict-vm-01:/var/www/example-app# php artisan event:cache
root@hamko-ict-vm-01:/var/www/example-app# php artisan route:cache
root@hamko-ict-vm-01:/var/www/example-app# php artisan view:cache

############################################### Apache Configer File #################################

root@hamko-ict-vm-01:~# cd /etc/apache2/sites-available/
root@hamko-ict-vm-01:/etc/apache2/sites-available# vim 000-default.conf

000-default.conf এই টা  Apache configer file  এখনে  ServerName  আমাদের  Server IP  হবে । 

================================================================================
<VirtualHost *:80>

ServerName  103.209.41.32
    DocumentRoot /var/www

	Alias /hee /var/www/hee/public
        Alias /glogo /var/www/glogo/public
        Alias /hajaribug /var/www/hajaribug/public

    <Directory /var/www/hee/public>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    <Directory /var/www/glogo/public>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    <Directory /var/www/hajaribug/public>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

	ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>

================================================================================

Page Redirect করার জন্য .htaccess file এ গিয়ে নতুন একটা লাই  Add করতে হবে ।

root@hamko-ict-vm-01:~# cd /var/www/
root@hamko-ict-vm-01:/var/www# cd hajaribug/public/

============================= New Line =======================

RewriteBase /hajaribug

==============================================================

================================== Example ==============================

<IfModule mod_rewrite.c>
    <IfModule mod_negotiation.c>
        Options -MultiViews -Indexes
    </IfModule>

    RewriteEngine On
    RewriteBase /hajaribug

    # Handle Authorization Header
    RewriteCond %{HTTP:Authorization} .
    RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]

    # Redirect Trailing Slashes If Not A Folder...
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_URI} (.+)/$
    RewriteRule ^ %1 [L,R=301]

    # Send Requests To Front Controller...
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ index.php [L]
</IfModule>
 
==============================================================================

#######################  Database Configer  করতে হবে  .env file #######################

root@hamko-ict-vm-01:/var/www/hajaribug# vim .env

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=acc_hajaribug_24
DB_USERNAME=hamko
DB_PASSWORD=123456789

#######################  Database Configer  করতে হবে  database.php file #######################

root@hamko-ict-vm-01:/var/www/hajaribug# cd config/
root@hamko-ict-vm-01:/var/www/hajaribug/config# vim database.php

 'mysql' => [
            'driver' => 'mysql',
            'url' => env('DATABASE_URL'),
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'acc_hajaribug_24'),
            'username' => env('DB_USERNAME', 'hamko'),
            'password' => env('DB_PASSWORD', '123456789'),


============================================================================================

======================== Floder Copy Command ============================

root@hamko-ict-vm-01:/opt/Acc-Hee# cp -r acc_hee/ /opt/Acc-Hee

======================================================================

######################  Ubuntu 22.04 Install Mariadb Web Login ######################

root@hamko-ict-vm-01:~# sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl


================================================ Reference Link ============================

https://laravel.com/docs/11.x/deployment
https://www.linuxcapable.com/how-to-install-php-8-3-on-ubuntu-linux/
https://dev.to/kenfai/laravel-artisan-cache-commands-explained-41e1
https://www.linuxcapable.com/how-to-configure-php-fpm-on-ubuntu-linux/
https://phoenixnap.com/kb/install-phpmyadmin-on-centos-8    


















