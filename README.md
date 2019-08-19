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