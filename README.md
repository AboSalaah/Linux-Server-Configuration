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
### Disable root login
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

