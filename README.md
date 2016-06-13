# nano-fs-05

```
ssh -i ~/.ssh/udacity_key.rsa -p 2200 root@52.39.44.26 
```

_The following steps were run after having logged into the virtual machine._

1. Create a new user named grader

  ```
  sudo adduser grader
  ```

2. Give the grader the permission to sudo

  ```
  sudo nano /etc/sudoers.d/grader
  ```
	
  Add the following:
	
  ```
  grader ALL=(ALL) NOPASSWD:ALL
  ```
	
  Save and exit.


3. Update all currently installed packages.

  ```
  sudo apt-get update
  sudo apt-get upgrade
  ```

4. Configure the local timezone to UTC.

  ```
  sudo dpkg-reconfigure tzdata
  ```
  
  Then follow the menu options to select UTC.

5. Change the SSH port from 22 to 2200
  
  ```
  nano /etc/ssh/sshd_config
  ```
  
  Then change the port number from 22 to 2200 in the file.
  
  Save the file and exit nano.
  
  ```
  service ssh restart
  ```
  
6. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)
  
  ```
  sudo ufw default deny incoming
  sudo ufw default allow outgoing
  sudo ufw allow 2200/tcp
  sudo ufw allow ntp
  sudo ufw allow www
  sudo ufw enable
  ```
  
7. Install and configure Apache to serve a Python mod_wsgi application
  
  ```
  sudo apt-get install apache2
  sudo apt-get install python-dev python-setuptools
  sudo apt-get install libapache2-mod-wsgi
  sudo service apache2 restart
  ```

8. Install and configure PostgreSQL
  
  ```
  sudo apt-get install postgresql
  sudo locale-gen en_GB.UTF-8
  sudo a2enmod wsgi
  sudo service apache2 restart
  
  ```

9. Install git, clone and set up your Catalog App project (from your GitHub repository from earlier in the Nanodegree program) so that it functions correctly when visiting your server’s IP address in a browser. Remember to set this up appropriately so that your .git directory is not publicly accessible via a browser!
  ```
  sudo apt-get install git
  cd /var/www
  git clone https://github.com/maweeks/nano-fs-03.git
  mkdir /var/www/catalog
  cp -r catalog /var/www/catalog
  mv nano-fs-03/ /var
  cd /var/www/catalog/catalog
  sudo apt-get install python-pip
  sudo pip install virtualenv
  sudo virtualenv venv
  source venv/bin/activate
  sudo pip install Flask
  
  sudo easy_install SQLAlchemy
  sudo pip install google-api-python-client
  sudo nano /etc/apache2/sites-available/FlaskApp
  ```
  
  Add the following to the new file.
  
  ```
  <VirtualHost *:80>
                ServerName 52.39.44.26
                ServerAdmin m_weeks@hotmail.co.uk
                WSGIScriptAlias / /var/www/catalog/catalog.wsgi
                <Directory /var/www/catalog/catalog/>
                        Order allow,deny
                        Allow from all
                </Directory>
                Alias /static /var/www/catalog/catalog/static
                <Directory /var/www/catalog/catalog/static/>
                        Order allow,deny
                        Allow from all
                </Directory>
                ErrorLog ${APACHE_LOG_DIR}/error.log
                LogLevel warn
                CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
  ```
  
  Save and exit nano.
  
  cd /var/www/catalog
  sudo nano flaskapp.wsgi
  
  ```
  #!/usr/bin/python
  import sys
  import logging
  logging.basicConfig(stream=sys.stderr)
  sys.path.insert(0,"/var/www/catalog/")

  from catalog import app as application
  application.secret_key = 'Add your secret key'
  ```
  
10. 
  a. Do not allow remote connections
    
    
    
  b. Create a new user named catalog that has limited permissions to your catalog application database
    
  
