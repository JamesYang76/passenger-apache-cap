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
$ sudo vim /lib/systemd/system/apache2.service #for fix error passenger-config restart-app
```
apache2.service
```bash
PrivateTmp=false
```
```bash
$ sudo systemctl daemon-reload
$ sudo systemctl restart apache2

$ passenger-config validate-install
$ passenger-memory-stats
```

https://www.pistolfly.com/weblog/en/2016/01/passenger-config-and-passenger-status-result-in-an-error-on-centos7.html
https://stackoverflow.com/questions/31761542/phusion-passenger-status-what-value-for-passenger-instance-registry-dir