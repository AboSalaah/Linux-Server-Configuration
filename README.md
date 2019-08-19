# Linux Server Configuration
In this project we will take a baseline installation of a Linux server and prepare it to host our web applications. We will secure our server from a number of attack vectors, install and configure a database server, and deploy one of our existing web applications onto it.

## Getting Started

### Update all currently installed packages
```
$ sudo apt-get update
$ sudo apt-get upgrade
```
### Change the SSH port from 22 to 2200
```
$ sudo nano /etc/ssh/sshd_config
```
* Then change port 20 to 2200, save and exit.

* After that we need to reload SSH
```
$ sudo service ssh restart
```

### Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).
* Block all the incoming requests

```
$ sudo ufw default deny incoming 
```
* Allow all the outgoing requests
```
$ sudo ufw default allow outgoing
```
* Allow incoming requests to specified ports
```
$ sudo ufw allow 2200/tcp
$ sudo ufw allow www
$ sudo ufw allow 123/udp 
```
* Activate the ufw firewall
```
$ sudo ufw enable
```
* Now, if we run  ```  $ sudo ufw status ```
```
To                         Action      From
--                         ------      ----
2200/tcp                   ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
123/udp                    ALLOW       Anywhere
2200/tcp (v6)              ALLOW       Anywhere (v6)
80/tcp (v6)                ALLOW       Anywhere (v6)
123/udp (v6)               ALLOW       Anywhere (v6)

```
### Disable root remote login
```
$ sudo nano /etc/ssh/sshd_config
```
* Change ```PermitRootLogin prohibit-password```  to  ```PermitRootLogin no```

### Create grader user
```
$ sudo adduser grader
```
### Give sudo access to grader user
```
$ sudo cp /etc/sudoers.d/90-cloud-init-users /etc/sudoers.d/grader

$ sudo nano /etc/sudoers.d/grader
```
* Replace ```ubuntu ALL=(ALL) NOPASSWD:ALL``` with ```grader ALL=(ALL) NOPASSWD:ALL```

### Create an SSH key pair for grader using the ssh-keygen tool

* On the local machine type this command
```
$ ssh-keygen
```
* It will ask for a password to add another layer of security

* On the server switch to the grader user and make sure you're in the home directory ```cd ~ ```
* Make ```.ssh``` directory which will store all your key related files
```
$ mkdir .ssh
```
* Create new file within ```.ssh``` directory called ```authorized_keys``` that will store all the public keys that this user is allowed to use for authentication
```
$ touch .ssh/authorized_keys
```

* Switch again to your local machine and copy the content of the public key file which has the extension ```.pub``` and paste it in ```authorized_keys``` file in your server

### Change the permissions of ```.ssh``` directory and ```authorized_keys``` file
```
$ chmod 700 .ssh
$ chmod 644 .ssh/authorized_keys
```

### Login to grader user
```
$ ssh grader@instanceIP -p SSH-port -i ~/.ssh/keys-file
```

### Configure the local timezone to UTC
```
$ sudo dpkg-reconfigure tzdata
```
* Choose ```none of the above from the first list```
* Choose ```UTC``` from the second list

### Install and configure Apache to serve a Python mod_wsgi application
```
$ sudo apt-get install apache2
$ sudo apt-get install libapache2-mod-wsgi python-dev
$ sudo service apache2 restart
```

### Install and configure PostgreSQL
* Install PostgreSQL
```
$ sudo apt-get install postgresql
```
* To make sure no remote connections are allowed we can check this file 
```
$ sudo nano /etc/postgresql/9.5/main/pg_hba.conf
```

* During the Postgres installation, an operating system user named postgres was created to correspond to the postgres PostgreSQL administrative user. We need to change to this user to perform administrative tasks
```
$ sudo su - postgres
```
* Get into postgreSQL shell 
```
$ psql
```
* Create a new database named ```itemcatalogpsql```
```
$ postgres=# CREATE DATABASE itemcatalogpsql;
```
* Create a new user name ```catalog```
```
$ postgres=# CREATE USER catalog;
```
* Set a password for the user ```catalog```
```
$ postgres=# ALTER ROLE catalog WITH PASSWORD '1234';
```
* Give our database user access rights to the database we created

```
$ postgres=# GRANT ALL PRIVILEGES ON DATABASE itemcatalogpsql TO catalog;
```
* To exit postgreSQL shell 
```
$ postgres=# \q
```
* To logout from ```postgres``` user
```
$ exit
```

### Install git
```
$ sudo apt-get install git
```

### Install packages needed for the project to work
* Install Flask
```
$ sudo apt-get install python-flask

```
* Install SQLalchemy
```
$ sudo apt-get install python-sqlalchemy
```

* Install requests module
```
$ sudo apt-get install python-requests
```
* Install PostgreSQL database adapter for the Python 
```
$ sudo apt-get install python-psycopg2
```
* Install http client library
```
$ sudo apt-get install python-httplib2
```
* Install oauth2 client library
```
$ sudo apt-get install python-oauth2client
```