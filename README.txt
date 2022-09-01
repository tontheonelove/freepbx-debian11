# freepbx-debian11 installation

sudo apt -y update

sudo apt -y upgrade

sudo reboot

sudo apt -y install git vim curl wget libnewt-dev libssl-dev libncurses5-dev subversion libsqlite3-dev build-essential libjansson-dev libxml2-dev uuid-dev

cd /usr/src/

sudo wget https://downloads.asterisk.org/pub/telephony/asterisk/asterisk-18-current.tar.gz

sudo tar xvf asterisk-18-current.tar.gz

cd asterisk-18*/

sudo contrib/scripts/get_mp3_source.sh

sudo contrib/scripts/install_prereq install

sudo ./configure

make menuselect
======================================
Under Add-ons, select chan_ooh323 and format_mp3
========================================
On Core Sound Packages, pick audio packets formats as shown
-WAV
-ULAW
-ALAW
-GSM
-G729
-G722
-SLN16
-SILEN7
-SILEN14
=============================================
Music On Hold and select the below modules
-WAV
-ULAW
-ALAW
-GSM
=============================================
Also select on Extra Sound Packages as shown
-WAV
-ULAW
-ALAW
-GSM
============================================
Under Applications, enable app_macro
==============================================
sudo make

sudo make install

sudo make progdocs

sudo make samples

sudo make config

sudo ldconfig

sudo groupadd asterisk

sudo useradd -r -d /var/lib/asterisk -g asterisk asterisk

sudo usermod -aG audio,dialout asterisk

sudo chown -R asterisk.asterisk /etc/asterisk

sudo chown -R asterisk.asterisk /var/{lib,log,spool}/asterisk

sudo chown -R asterisk.asterisk /usr/lib/asterisk


sudo vim /etc/default/asterisk       
AST_USER="asterisk"
AST_GROUP="asterisk"


sudo vim /etc/asterisk/asterisk.conf
runuser = asterisk ; The user to run as.
rungroup = asterisk ; The group to run as


AST_USER="asterisk"
AST_GROUP="asterisk"

sudo systemctl restart asterisk
sudo systemctl enable asterisk

sed -i 's";\[radius\]"\[radius\]"g' /etc/asterisk/cdr.conf
sed -i 's";radiuscfg => /usr/local/etc/radiusclient-ng/radiusclient.conf"radiuscfg => /etc/radcli/radiusclient.conf"g' /etc/asterisk/cdr.conf
sed -i 's";radiuscfg => /usr/local/etc/radiusclient-ng/radiusclient.conf"radiuscfg => /etc/radcli/radiusclient.conf"g' /etc/asterisk/cel.conf


sudo apt -y install mariadb-server mariadb-client
sudo systemctl start mariadb
sudo systemctl enable mariadb


mysql_secure_installation 


sudo apt install curl dirmngr apt-transport-https lsb-release ca-certificates
sudo curl -sL https://deb.nodesource.com/setup_14.x | sudo bash
sudo apt update
sudo apt -y install gcc g++ make
sudo apt -y install nodejs



sudo apt -y install apache2
sudo cp /etc/apache2/apache2.conf /etc/apache2/apache2.conf_orig
sudo sed -i 's/^\(User\|Group\).*/\1 asterisk/' /etc/apache2/apache2.conf
sudo sed -i 's/AllowOverride None/AllowOverride All/' /etc/apache2/apache2.conf

# Remove default index.html page

sudo rm -f /var/www/html/index.html
sudo unlink  /etc/apache2/sites-enabled/000-default.conf



sudo apt install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2

echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/sury-php.list

wget -qO - https://packages.sury.org/php/apt.gpg | sudo apt-key add -

sudo apt remove php*

sudo apt install php7.4-{mysql,cli,common,imap,ldap,xml,fpm,curl,mbstring,zip,gd,gettext,xml,json}

sudo apt install libapache2-mod-php7.4

sudo sed -i 's/\(^upload_max_filesize = \).*/\120M/' /etc/php/7.4/apache2/php.ini
sudo sed -i 's/\(^upload_max_filesize = \).*/\120M/' /etc/php/7.4/cli/php.ini
sudo sed -i 's/\(^memory_limit = \).*/\1256M/' /etc/php/7.4/apache2/php.ini

sudo apt install sox mpg123 lame ffmpeg sqlite3 git unixodbc dirmngr postfix  odbc-mariadb pkg-config libicu-dev



sudo tee /etc/odbcinst.ini<<EOF
[MySQL]
Description = ODBC for MySQL (MariaDB)
Driver = /usr/lib/x86_64-linux-gnu/odbc/libmaodbc.so
FileUsage = 1
EOF

sudo tee /etc/odbc.ini<<EOF
[MySQL-asteriskcdrdb]
Description = MySQL connection to 'asteriskcdrdb' database
Driver = MySQL
Server = localhost
Database = asteriskcdrdb
Port = 3306
Socket = /var/run/mysqld/mysqld.sock
Option = 3
EOF


cd /usr/src
sudo wget http://mirror.freepbx.org/modules/packages/freepbx/7.4/freepbx-16.0-latest.tgz
sudo tar xfz freepbx-16.0-latest.tgz
sudo rm -f freepbx-16.0-latest.tgz


cd freepbx
sudo systemctl stop asterisk
sudo ./start_asterisk start

sudo ./install -n --dbuser root --dbpass "yourpassword"

sudo fwconsole ma disablerepo commercial
sudo fwconsole ma installall
sudo fwconsole ma delete firewall
sudo fwconsole reload
sudo fwconsole restart


sudo a2enmod rewrite
sudo systemctl restart apache2


Visit your browser http://<your-server-ip> to load FreePBX.









