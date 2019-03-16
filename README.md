# README

#install apache2
```bash
# install apache2
$ sudo apt-get install apache2 apache2-doc apache2-utils
$ sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/passenger-apache-cap.conf
$ sudo a2dissite 000-default.conf
```

```bash
$ sudo apt-get install -y libapache2-mod-passenger
$ sudo apt-get install -y nodejs

$ sudo passenger-config about ruby-command
$ sudo vim /etc/apache2/sites-available/passenger-apache-cap.conf
```

```xml
<VirtualHost *:80>
    ServerName ec2-13-211-167-226.ap-southeast-2.compute.amazonaws.com

    ServerAdmin ubuntu@ec2-13-211-167-226.ap-southeast-2.compute.amazonaws.com
    DocumentRoot /var/www/passenger-apache-cap/current/public
    PassengerRuby /home/ubuntu/.rvm/gems/ruby-2.5.0/wrappers/ruby

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    <Directory "/var/www/passenger-apache-cap/current/public">
        Options FollowSymLinks
        Require all granted
    </Directory>
</VirtualHost>
```

```bash
$ sudo a2ensite /etc/apache2/sites-available/passenger-apache-cap.conf
$ sudo a2enmod passenger

$ sudo systemctl reload apache2
$ sudo systemctl restart apache2
$ passenger-config restart-app
```

###two ways for fix error passenger-config restart-app 
1. add PASSENGER_INSTANCE_REGISTRY_DIR
```bash
$ mkdir /home/ubuntu/passenger_temp
$ sudo vim /etc/apache2/mods-enabled/passenger.conf
PassengerInstanceRegistryDir /home/ubuntu/passenger_temp # added into passenger.conf


#config/deploy/production.rb in the Rails app
set :default_env, { 'PASSENGER_INSTANCE_REGISTRY_DIR' => '/home/MYUSER/passenger_temp' }
```


2. disable PrivateTmp
```bash
$ sudo vim /lib/systemd/system/apache2.service

PrivateTmp=false # in apache2.service

$ sudo systemctl daemon-reload
```

https://www.linode.com/docs/development/ror/ruby-on-rails-apache-debian/

https://stackoverflow.com/questions/46058728/passenger-doesnt-seem-to-be-running-capistrano-rails-apache-ubuntu/46224099
https://www.pistolfly.com/weblog/en/2016/01/passenger-config-and-passenger-status-result-in-an-error-on-centos7.html