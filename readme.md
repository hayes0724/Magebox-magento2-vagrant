## Magebox - Magento2 Vagrant 
Simple vagrant setup for Magento2 that provsions all necessary software then installs Magento2 via git or composer.

### Main Features
+ Install your choice of software automatically with one vagrant box
+ Installs Magento2 via GIT or Composer automatically with `vagrant up`
+ All settings are in one file "env" to allow you to quickly customize any settings.

### Software
+ NGINX or Apache
+ MySQL, Percona or MariaDB
+ PHP-FPM 5.6

### Installation
1. `git clone https://github.com/hayes0724/mage2-vagrant-windows.git`
2. Add your Magento Connect authentication credentials to the global enviroment file "env"
	1. If using git instead of Composer to install Magento2 then add your Github access token also
	2. Instructions for getting credentials: [Magento Authentication](http://devdocs.magento.com/guides/v2.0/install-gde/prereq/dev_install.html#generate-authentication-tokens)
3. `Vagrant up`
4. Add `192.168.33.10 mage2.dev www.mage2.dev` in 
	1. ![Windows](docs/images/windows.png) `C:\Windows\System32\drivers\etc\hosts`
	2. ![Linux](docs/images/linux.png) `/etc/hosts`
5. Your site is now installed and ready to use, go to `www.mage2.dev`

### Usage
+ Store URL: www.mage2.dev
+ Admin URL: www.mage2.dev/admin_123
+ Username: admin
+ Password: mage1234

### Customization
All enviroment variables such software used, database name/password, etc... are all editable from one simple file "env". This will be in a shared folder ~/env on the guest machine.

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
PHP_VERSION=5 # Only 5.6 is supported currently
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
magebox script is located in ~/scripts folder in guest machine.

```shell
GENRAL
magebox bootstrap			Install all required software (PHP, Apache/Nginx, Mysql, etc..)
magebox help				Shows commands avaliable
magebox enviroment  		Shows current enviroment variables

MAGENTO2 INSTALL
magebox install				Install Magento2 based on enviroment settings
magebox install:git			Install Magento2 with git
magebox install:composer	Install Magento2 with Composer

DATABASE
magebox database:install 	Install database from config settings

PHP SERVER
magebox php:install			Install PHP server from config settings

WEB SERVER
magebox web:install 		Install web server from config settings
```
### Roadmap

#### Software

- [x] apache
- [x] percona
- [x] mariadb
- [ ] php7
- [ ] php-fpm 5.5.9
- [ ] memcache
- [ ] redis
- [ ] varnish
- [ ] opcache gui
- [ ] xdebug

#### Support

- [ ] Windows phpstorm configuration files and instructions

#### Features

- [ ] reinstall script to reset magento install
- [ ] hostmanager to setup hosts file
