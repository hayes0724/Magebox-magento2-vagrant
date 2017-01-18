## Magebox - Magento2 Vagrant
Simple vagrant setup for Magento2 that provisions all necessary software then installs Magento2 via git or composer. Works on Windows 10 and Linux, can be used without vagrant directly on Ubuntu.

### Main Features
+ Install your choice of software automatically with one vagrant box
+ Easily switch between Apache, Nginx, PHP 5/7, etc.... with one line of code
+ Installs Magento2 via GIT or Composer automatically with `vagrant up`
+ All settings are in one file "env" to allow you to quickly customize any settings.
+ Restore most recent database and media files automatically

### Software
+ NGINX or Apache
+ MySQL, Percona or MariaDB
+ PHP-FPM 5.6 or 7

### Installation
1. `git clone https://github.com/hayes0724/mage2-vagrant-windows.git project-name`
2. Add your Magento Connect authentication credentials to the global enviroment file "env"
	1. If using git instead of Composer to install Magento2 then add your Github access token also
	2. Instructions for getting credentials: [Magento Authentication](http://devdocs.magento.com/guides/v2.0/install-gde/prereq/dev_install.html#generate-authentication-tokens)
3. `Vagrant up`
4. Add `192.168.33.10 mage2.dev www.mage2.dev` in
	1. Windows: `C:\Windows\System32\drivers\etc\hosts`
	2. Linux: `/etc/hosts`
5. Your site is now installed and ready to use, go to `www.mage2.dev`

### Usage
+ Store URL: www.mage2.dev
+ Admin URL: www.mage2.dev/admin_123
+ Username: admin
+ Password: mage1234

### SSL
If you wish to use SSL you can create a self signed cert automatically and have magebox configure it with your installed web server
```shell
magebox ssl:makecert
```
To enable SSL
```shell
magebox ssl:enable
```

### Restore database and media files
Restore Database: Copy database file (.sql) to share/restore/db, This will update your URL's on import to match vagrant box
```shell
magebox restore:database
```
Restore Media: copy your media (.tgz) file to share/restore/media
```shell
magebox restore:media
```

### Customization
All environment variables such software used, database name/password, etc... are all editable from one simple file "env". This will be in a shared folder ~/share/scripts/env on the guest machine.

##### Web Server
```shell
WEB_SERVER="nginx" # options: nginx or apache
HOST_NAME='mage2.dev'
```

##### Database
```shell
DATABASE_SOFTWARE="mysql" # Options: mysql, percona & mariadb
DATABASE_NAME='magento'
DATABASE_USER='magento'
DATABASE_PASS='mage1234'
```
##### PHP
```shell
PHP_VERSION=7 # Options: 5, 7
```

##### Auth.json
```shell
GITHUB_TOKEN=""
AUTH_USER=""
AUTH_PASS=""
```

##### Magento2 Install variables
```shell
AUTO_INSTALL="composer" # Choose Magento2 auto installer method: composer or git
INSTALL_DIR="/home/vagrant/mage2"
MAGENTO_HOME="$INSTALL_DIR/public_html" # Do not edit
base_url='http://www.mage2.dev'
db_password=$DATABASE_PASS # Do not edit
db_host='localhost'
db_name=$DATABASE_NAME # Do not edit
db_user=$DATABASE_USER # Do not edit
admin_firstname='Name'
admin_lastname='Lastname'
admin_email='your@email.com'
admin_url='admin_123'
admin_user='admin'
admin_password='mage1234'
language='en_US'
currency='USD'
timezone='America/Chicago'
```

##### Magebox script
Magebox script is located in ~/share/scripts folder in guest machine. It is automatically run with vagrant up but can be used to install outside of vagrant or make changes after first provision.

```shell
Usage: magebox [command] [-d]

 GENERAL
magebox bootstrap       Install all required software (PHP, Apache/Nginx, Mysql, etc..)
magebox help            Shows commands available
magebox enviroment      Shows current environment variables

 MAGENTO2 INSTALL
magebox install              Install Magento2 based on environment settings
magebox install:git          Install Magento2 with git
magebox install:composer     Install Magento2 with Composer
magebox install:sample       Install magento sample data with composer

 SSL
magebox ssl:disable          Disables SSL in the database
magebox ssl:enable           Enables SSL in database
magebox ssl:makecert         Create a self signed SSL cert and add to web server

 RESTORE
magebox restore:database     Restore database from the restore/db folder will choose latest if multiples
magebox restore:media        Restore media from the restore/media folder will choose latest if multiples

 MODE
magebox mode:developer 		 	 set magento to developer mode
magebox mode:production 	 	 set magento to production mode

 DATABASE
magebox database:install     Reinstall database server from env settings

 PHP SERVER
magebox php:install          Reinstall PHP server from env settings

 WEB SERVER
magebox web:install          Reinstall web server from env settings
```
