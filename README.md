# nano-fs-05

```
ssh -i ~/.ssh/udacity_key.rsa -p 2200 root@52.39.44.26 
```

_The following steps were run after having logged into the virtual machine._

1. Update all currently installed packages.

  ```
  sudo apt-get update
  sudo apt-get upgrade
  ```

2. Configure the local timezone to UTC.

  ```
  sudo dpkg-reconfigure tzdata
  ```
  
  Then follow the menu options to select UTC.

3. Change the SSH port from 22 to 2200
  
  ```
  nano /etc/ssh/sshd_config
  ```
  
  Then change the port number from 22 to 2200 in the file.
  
  Save the file and exit nano.
  
  ```
  service ssh restart
  ```
  
4. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)
  
  ```
  sudo ufw default deny incoming
  sudo ufw default allow outgoing
  sudo ufw allow 2200/tcp
  sudo ufw allow ntp
  sudo ufw allow www
  sudo ufw enable
  ```
  
5. Install and configure Apache to serve a Python mod_wsgi application
  
  
  
6. Install and configure PostgreSQL
  
  a. Do not allow remote connections
    
    
    
  b. Create a new user named catalog that has limited permissions to your catalog application database
    
    
    

7. Install git, clone and set up your Catalog App project (from your GitHub repository from earlier in the Nanodegree program) so that it functions correctly when visiting your server’s IP address in a browser. Remember to set this up appropriately so that your .git directory is not publicly accessible via a browser!

  
