#/bin/bash
apt-get update && apt-get upgrade
apt-get install mysql-server mysql-client libmysqlclient-dev
echo "CREATE DATABASE cit DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci; GRANT ALL ON cit.* TO 'cit'@'localhost' IDENTIFIED BY 'cit'; FLUSH PRIVILEGES;" | mysql -u root -p
apt-get install graphicsmagick-libmagick-dev-compat imagemagick ruby1.8 ruby1.8-dev rubygems ri rdoc rake librmagick-ruby libcurl4-openssl-dev apache2-threaded-dev libapr1-dev libaprutil1-dev
#this didn't seem to be available for 12.04.4
apt-get install ruby-switch
ruby-switch --set ruby1.8

gem install gchartrb RedCloth json tzinfo echoe fastercsv test-spec ferret hoe mongrel eventmachine -r
gem install ZenTest -v 4.1.4 -r
gem install icalendar -v 1.1.0 -r
gem install mysql -v 2.7 -r
gem install passenger
gem install rdoc-data
rdoc --install

wget https://github.com/cmoman/clockingit/archive/master.tar.gz
tar -zxvf master.tar.gz
mv clockingit-master/cit /opt/.
cd /opt/cit
ruby setup.rb
chmod 777 /opt/cit -Rf
apt-get install apache2
a2enmod rewrite
echo "<VirtualHost *:80>
ServerName cit.tesla.local
DocumentRoot /opt/cit/public/
ErrorLog /opt/cit/log/http.log
<Directory /opt/cit/public/>
Options ExecCGI FollowSymLinks
AllowOverride all
Allow from all
Order allow,deny
</Directory>
</VirtualHost>" > /etc/apache2/sites-available/default
/etc/init.d/apache2 restart
cp cit /etc/init.d/cit
chmod +x /etc/init.d/cit
sed -i 's/CHANGE_CIT_PATH/\/opt\/cit/g' -i /etc/init.d/cit
/etc/init.d/cit start

passenger-install-apache2-module 

#we now add the passenger module to the apache2 configuration.

echo "<VirtualHost *:80>
LoadModule passenger_module /var/lib/gems/1.8/gems/passenger-4.0.57/buildout/apache2/mod_passenger.so
<IfModule mod_passenger.c>
PassengerRoot /var/lib/gems/1.8/gems/passenger-4.0.57
PassengerDefaultRuby /usr/bin/ruby1.8
</IfModule>

ServerName cit.blahdeblah.co.nz
#RailsBaseURI /rails

DocumentRoot /opt/cit/public/
ErrorLog /opt/cit/log/http.log
<Directory /opt/cit/public/>
Options ExecCGI FollowSymLinks
Options -MultiViews
AllowOverride all
Allow from all
Order allow,deny
</Directory>
</VirtualHost>" > /etc/apache2/sites-available/default
