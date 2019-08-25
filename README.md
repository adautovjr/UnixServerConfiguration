# UnixServerConfiguration
Udacity Project

##
This project's purpose was to completely setup and secure a linux-based server on the internet. Basically, I went through Firewall configurations and used SSH keys to make sure this server will be acessed only by authorized personnel. Also, I left one of my projects running on http, which you can check it out by clicking [here](http://adautovjr.codes).

## Important info

* IP: 167.71.176.162
* SSH Port: 2200
* URL: [http://adautovjr.codes](http://adautovjr.codes)

## Configuration

### Check update
1. Update package list
    ```
	sudo apt-get update
    ```
2. Perform an upgrade
    ```
    sudo apt-get upgrade
	```
3. Remove old packages
    ```
    sudo apt-get autoremove
    ```

### Create new user and give ```sudo```
* Create the new user
    1. Install finger
        ```
        sudo apt-get install finger
        ```
    2. Add new user "grader"
        ```
        sudo adduser grader
        ```
    3. check user info
        ```
        cat /etc/passwd
        ```

* Give ```sudo``` access to the new user (e.g. grader)
    1. Open sudoers options
        ```
        sudo cat /etc/sudoers
        ```
    2. Get list of sudoers
        ```
        sudo ls /etc/sudoers.d
        ```
    3. Create user profile for the new user (e.g. grader)
        ```
        sudo touch /etc/sudoers.d/grader
        ```
    4. Edit profile
        ```
        sudo nano /etc/sudoers.d/grader
        ```
    5. Add new line
        ```
        grader ALL=(ALL) NOPASSWD:ALL
        ```

### SSH
#### Generate key pairs
* build key pairs in local (in CMD or git bash)
	```
    ssh-keygen
    ```
* name key pairs (e.g. GraderPrivateKey)
* enter passphrase (Sent on Instructor notes)

#### Publish key pairs
1. Login linux us a user (e.g. grader)
2. Create a folder and a file by
    ```
    mkdir .ssh
    touch .ssh/authorized_keys
    ```
4. Go to local key folder
    ```
    cat id_rsa.pub
    ```
4. copy all
5. back to linux
    ```
    nano .ssh/authorized_keys
    ```
6. paste all
7. save
7. change folder permission
    ```
    chmod 700 .ssh
    chmod 644 .ssh/authorized_keys
    ```

#### Login with key pairs
1. ssh grader@167.71.176.162 -p 2200 -i GraderPrivateKey
2. enter passphrase (Sent on Instructor notes)

#### Disable password-based login & root login, and change port to 2200
1. Type
    ```
    sudo nano /etc/ssh/sshd_config
    ```
2. scroll down and find
    ```
    PasswordAuthentication Yes
    ```
3. change Yes to No
4. Find
    ```
    PermitRootLogin
    ```
5. Change to
    ```
    PermitRootLogin no
    ```
6. Find
    ```
    #Port 22
    ```
7. Uncomment and change to 2200
8. restart service
    ```
    sudo service ssh restart
    ```

### Configuration of Firewall
1. Block all incoming connections on all ports:
    ```
    sudo ufw default deny incoming
    ```
2. Deny incoming connections for SSH on port 22:
    ```
    sudo ufw deny 22
    ```
3. Allow outgoing connection on all ports:
    ```
    sudo ufw default allow outgoing
    ```
4. Allow incoming connection for SSH on port 2200:
    ```
    sudo ufw allow 2200/tcp
    ```
5. Allow incoming connections for HTTP on port 80:
    ```
    sudo ufw allow www
    ```
6. Allow incoming connection for NTP on port 123:
    ```
    sudo ufw allow ntp
    ```
7. To check the rules that have been added before enabling the firewall use:
    ```
    sudo ufw show added
    ```
8. To enable the firewall, use:
    ```
    sudo ufw enable
    ```
9. To check the status of the firewall, use:
    ```
    sudo ufw status
    ```

### Install required packages

1. Install Apache:
	```
    sudo apt-get install apache2
    ```
    1. Install the libapache2-mod-wsgi package and setting:
        ```
        sudo apt-get install libapache2-mod-wsgi
        sudo apt-get install libapache2-mod-wsgi-py3 (if python3, this case used 2)
        ```
    2. Enable the mod_wsgi using the command:
        ```
        sudo a2enmod wsgi
        ```
    3. Install some libraries of python development:
        ```
        sudo apt-get install libpq-dev python-dev	
        ```

3. Install packages on requirements.txt
    ```
    pip install -r requirements.txt
    ```

4. Install git and clone project
    ```
    sudo apt-get install git
    ```
    1. Navigate to
        ```
        /var/www/html/ItemsCatalog/
        ```
    2. Clone to ItemsCatalog folder
        ```
        sudo git clone projectURL ItemsCatalog
        ```

5. Install Virtual Environment

    * From /var/www/html/ItemsCatalog/ directory install pip:
        ```
        sudo apt-get install python3-pip
        ```

    * Install the virtual environment:
        ```
        sudo apt-get install python-virtualenv
        ```
    * Create the virtual environment:
        ```
        sudo virtualenv -p python3 venv3
        ```
    * Change the ownership to grader with:
        ```
        sudo chown -R grader:grader venv3/
        ```
        
### Create and update catalog.wsgi file for the installation
* From /var/www/html/ItemsCatalog/
    ```
    touch /var/www/html/ItemsCatalog/ItemsCatalog.wsgi
    ```
* Add
    ```
    #!/usr/bin/python
    import sys
    import logging
    logging.basicConfig(stream=sys.stderr)
    sys.path.insert(0, "/var/www/html/ItemsCatalog/")
    
    from ItemsCatalog import app as application
    ```
* Save

### Configure and Enabling New Virtual Host
1. Create and edit virtual host
    ```
    sudo nano /etc/apache2/sites-available/ItemsCatalog.conf
    ```
2. Add IP address
    ```
    <VirtualHost *:80>
        ServerName adautovjr.codes
        ServerAdmin adautovjr@yahoo.com

        WSGIScriptAlias / /var/www/html/ItemsCatalog/ItemsCatalog.wsgi
        WSGIDaemonProcess ItemsCatalog python-path=/home/adminfm5/ItemsCatalog/venv/lib/python3.7/site-packages:/var/www/html/ItemsCatalog/venv/lib/python3.7/site-packages
        WSGIProcessGroup ItemsCatalog

        <Directory /var/www/html/ItemsCatalog/>
                Order allow,deny
                Allow from all
                <Files ItemsCatalog.wsgi>
                        Require all granted
                </Files>
        </Directory>
        Alias /static /var/www/html/ItemsCatalog/static
        <Directory /var/www/html/ItemsCatalog/static/>
                Order allow,deny
                Allow from all
        </Directory>
        ErrorLog /var/www/html/ItemsCatalog/error.log
        LogLevel warn
        CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>
    ```
3. Enable the virtual host by using the command:
    ```
    sudo a2ensite ItemsCatalog.conf
    ```
4. Type the following command for restarting the apache:
    ```
    service apache2 reload
    service apache2 restart
    ```
## Reference 

* [Python Flask Fazendo deploy com Apache](http://www.devfuria.com.br/python/flask-apache/)
