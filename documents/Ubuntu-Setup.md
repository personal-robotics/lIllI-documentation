# Initial Ubuntu Setup


The following steps are required to set up a new Ubuntu 22.04.2 LTS system. 
The steps are the same both on SBC as well as on a desktop machine.
The following procedure has been tested with Ubuntu 22.04.2 LTS. 
Later versions of Ubuntu should also work in the same way.

## Install Ubuntu Linux for your platform

Install Ubuntu 22.04.2 LTS for your platform.

## Ubuntu Software Updates

Ensure package lists and all installed packages are up-to-date before continuing:
```
# update package lists
sudo apt-get update

# update installed packages, add/remove packages if necessary
sudo apt-get dist-upgrade
```

## Headless Web Server

```
sudo apt install screen wget curl net-tools 
sudo apt install software-properties-common git python3 python3-pip python3-virtualenv 
sudo apt install cockpit jupyter-lab shellinabox tmux gnuscreen python3-serial vim emacs npm yarn webmin
```

## Desktop Software (optional)

Install desktop environment for GUI apps if needed:
```
# install minimal kde desktop incl. Qt libraries for GUI apps (e.g. rviz, gazebo)
# TODO use smaller set of packages?
sudo apt-get install kde-plasma-desktop
```

## Other Useful Software

```
sudo apt-get install aptitude screen 
```

### SSH Server

Esp. useful for headless web server.

```
sudo apt install task-ssh-server
# start server
service ssh start
```

## Configure Network (Static IP, DHCP)

### For Ubuntu Desktop Installations

TODO: configure network using configuration files, e.g. for Raspberry Pi.

