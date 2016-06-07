# udacity-linux
AWS server setup


Basic Info for server:

IP: http://52.39.255.30/

URL: http://drtysnow.com/

AWS: ec2-52-39-255-30.us-west-2.compute.amazonaws.com



NOTE: If testing CRUD operations, after login edit your user profile to grant admin priveleges to the site. Only admins can delete specific content. 

# Configuration steps:

# update all packages:

apt-get update

apt-get upgrade

apt-get dist-upgrade


reboot # (if required)

# Add user, set passwd, grant sudo:

adduser grader

passwd grader  

addgroup grader sudo

# Lock Down, set ports for fw:
ufw status

ufw disable

ufw allow 2200/tcp

ufw allow 80/tcp

ufw allow 123/udp

ufw deny 22/tcp

Edit /etc/ssh/sshd_config, change port setting, disable root login, verify no passwd login (key only)

ssh restart

# Verify SSH port:
netstat -l


# Turn on firewall:
ufw enable

# Modify time zone to GMT:
dpkg-reconfigure tzdata

# install packages:
apt-get install python-pip git postgresql libapache2-mod-wsgi apache2dr libpq-dev python-dev

# install python modules:
pip install flask httplib2 flaskwtf oauth2client

#### setup postgres: ####

#become the postgres user:

sudo su postgres

# create db:
psql
create database catalog;
create user catalog with password 'catalog';

# Check/Disable for remote db access:
cat /etc/postgresql/9.3/main/postgresql.conf | grep listen
cat /etc/postgresql/9.3/main/postgresql.conf | grep port
netstat -l
ufw deny 5432/tcp

# turn on the wsgi apache module: 

a2enmod wsgi

# Create WSGI file that defines the app at:
  mkdir /var/www/drtysnow/drtysnow.wsgi
  
# Create apache site defination at:
 /etc/apache2/sites-available/appname.conf

# Download site code:

git pull app structure, copy the application directory to /var/www/appname/appname

# Set the ownership and permissions of the upload folder:

chmod 755 path to upload folder

chown -R www-data:www-data path to folder

restart apache
