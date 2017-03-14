# Configuring TINC `ninuxto` on a UDOO NEO

### Introduction

This document explains how to configure a UDOO NEO to connect to TINC network `ninuxto`.

The following instructions should also work for a generic host running Ubuntu 14.04 or later.

### Prerequisites

* One [UDOO NEO Full](http://www.udoo.org/udoo-neo/) connected to Internet via Ethernet
* Booting from Micro SD Card written from file `udoobuntu-udoo-neo-desktop_2.0rc2.img`

### Step-by-step instructions

Login to udooneo-gm via Remote Terminal (i.e. browse http://100.84.248.76:8000/) - or via the [serial console](http://gmacario.github.io/howto/udoo/neo/embedded/software/development/2015/11/08/connecting-to-udoo-neo-serial-console.html)

Logged as udooer@udooneo-gm, change password for user "udooer", then configure hostname (let us choose `udooneo1234` in this example)

```
$ passwd udooer
$ echo "udooneo1234" | sudo tee /etc/hostname
$ sudo vi /etc/hosts      (Replace occurrencies of old hostname with new)
```

Update installed Ubuntu packages, then reboot to activate the changes

```
$ sudo apt-get update && sudo apt-get -y dist-upgrade
$ sudo reboot
```

As soon as the host is up and running, login as `udooer@udooneo1234`, then install TINC and other required pacakges

```
$ sudo apt-get -y install git rsync tinc
```

(Optional) Verify the correct installation of tinc

```
$ dpkg -l tinc
$ dpkg -L tinc
```

Clone the "gmacario/tinc-ninuxto" repository from GitHub

```
$ mkdir -p ~/MYGIT
$ cd ~/MYGIT && [ ! -e tinc-ninuxto ] && git clone https://github.com/gmacario/tinc-ninuxto
```

Create the local TINC configuration

```
$ cd ~/MYGIT/tinc-ninuxto && git pull --all --prune && \
  sudo mkdir -p /etc/tinc/ninuxto/hosts/ && \
  sudo rsync -avz hosts/ /etc/tinc/ninuxto/hosts/
```

Customize TINC configuration files starting from some templates

```
$ sudo cp ~/MYGIT/tinc-ninuxto/sample-tinc.conf /etc/tinc/ninuxto/tinc.conf
$ sudo vi /etc/tinc/ninuxto/tinc.conf      (Adjust Name="udooneo1234")
```

```
$ sudo cp ~/MYGIT/tinc-ninuxto/sample-tinc-up /etc/tinc/ninuxto/tinc-up
$ sudo vi /etc/tinc/ninuxto/tinc-up        (Choose an available IP Address according to the table at README.md)
```

```
$ sudo cp ~/MYGIT/tinc-ninuxto/sample-tinc-down /etc/tinc/ninuxto/tinc-down
$ sudo vi /etc/tinc/ninuxto/tinc-down      (Everything should be OK, but double check)
```

Create a public/private key pair if they do not exist

```
$ [ ! -e /etc/tinc/ninuxto/rsa_key.priv ] && tincd -n ninuxto -K4096
```

then submit a Pull Request to https://github.com/gmacario/tinc-ninuxto with

* A new line in the table at `README.md` to mark the IP address you chose
* Your **public** key `/etc/tinc/ninuxto/hosts/udooneo1234` under `hosts/`

### Enabling VPN at boot

To have TINC network `ninuxto` active at boot, do the following

```
$ echo "ninuxto" | sudo tee -a /etc/tinc/nets.boot
$ sudo service tinc restart
```

<!-- EOF -->
