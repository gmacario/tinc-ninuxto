# Configuring TINC `ninuxto` on a UDOO NEO

### Introduction

This document explains how to configure a [UDOO NEO](http://www.udoo.org/udoo-neo/) board to connect to TINC network `ninuxto`.

The following instructions should also work for a generic host running Ubuntu 14.04 or later.

### Prerequisites

* One [UDOO NEO Full](http://www.udoo.org/udoo-neo/) connected to Internet via Ethernet
* A blank MicroSD Card  (>= 8 GB)

### Step-by-step instructions

#### Prepare your UDOO NEO

* Browse <http://www.udoo.org/downloads/>
  - Click on tab "UDOO NEO"
  - Download "UDOObuntu 2.2.0 Minimal (14.04 LTS) from "MIRROR EU"
* Download and install [Etcher](https://etcher.io/) for your host OS
* Run Etcher and write the MicroSD Card with file `udoobuntu-udoo-neo-minimal_2.2.0.zip`
* Insert the MicroSD Card into your UDOO NEO
* Connect the UDOO NEO to your router through an Ethernet cable
* Power up the UDOO NEO
* Use the [Fing App](https://www.fing.io/) or similar tools to identify the IP address assigned to your UDOO NEO

Login to your UDOO Neo via Remote Terminal (i.e. browse <http://assigned_ip_address:8000/>) - or via a [serial console](http://gmacario.github.io/howto/udoo/neo/embedded/software/development/2015/11/08/connecting-to-udoo-neo-serial-console.html)

#### Change default password

Logged as user `udooer`, change default password:

```
passwd udooer
```

Logout and login to apply the changes

#### Change default hostname 

Configure hostname (let us choose `udooneo1234` in this example)

```
echo "udooneo1234" | sudo tee /etc/hostname
sudo vi /etc/hosts      # Replace all occurrences of "udooneo" with the new hostname
```

#### Install required packages

Update installed Ubuntu packages, then reboot your UDOO NEO to activate the changes

```
sudo apt-get update && sudo apt-get -y dist-upgrade
sudo reboot
```

As soon as the host is up and running, login as `udooer@udooneogm01`, then install TINC and other required pacakges

```
sudo apt-get -y install git rsync tinc
```

(Optional) Verify the correct installation of tinc

```
dpkg -l tinc
dpkg -L tinc
```

#### Configure TINC for network `ninuxto`

Clone the "gmacario/tinc-ninuxto" repository from GitHub

```
mkdir -p ~/github/gmacario
cd ~/github/gmacario && [ ! -e tinc-ninuxto ] && git clone https://github.com/gmacario/tinc-ninuxto
```

Create the local TINC configuration

```
cd ~/github/gmacario/tinc-ninuxto && git pull --all --prune && \
  sudo mkdir -p /etc/tinc/ninuxto/hosts/ && \
  sudo rsync -avz hosts/ /etc/tinc/ninuxto/hosts/
```

Customize TINC configuration files starting from some templates

```
sudo cp ~/github/gmacario/tinc-ninuxto/sample-tinc.conf /etc/tinc/ninuxto/tinc.conf
sudo vi /etc/tinc/ninuxto/tinc.conf      # Adjust Name="udooneo1234"
```

**NOTE**: Make sure the name as spelled in `tinc.conf` does only contain letters and digits (no spaces, hashes, etc.)

```
sudo cp ~/github/gmacario/tinc-ninuxto/sample-tinc-up /etc/tinc/ninuxto/tinc-up
sudo vi /etc/tinc/ninuxto/tinc-up        # Choose an available IPv4 address according to the table at README.md
sudo chmod 755 /etc/tinc/ninuxto/tinc-up
```

```
sudo cp ~/github/gmacario/tinc-ninuxto/sample-tinc-down /etc/tinc/ninuxto/tinc-down
sudo vi /etc/tinc/ninuxto/tinc-down      # Everything should be OK, but double check
sudo chmod 755 /etc/tinc/ninuxto/tinc-down
```

Create a public/private key pair if they do not exist

```
sudo sh -c "[ ! -e /etc/tinc/ninuxto/rsa_key.priv ] && tincd -n ninuxto -K4096"
```

then submit a Pull Request to <https://github.com/gmacario/tinc-ninuxto> with

* A new line in the table at `README.md` to mark the IP address you chose
* Your **public** key `/etc/tinc/ninuxto/hosts/udooneo1234` under `hosts/`

After the PR is merged, update the gmacario/tinc-ninuxto repository in all your peer nodes (i.e. tincgw21, rpi3gm23) to make sure the new node is recognized.

#### Test connectivity to network `ninuxto`

Try connecting to TINC network ninuxto

```
sudo tincd -n ninuxto --no-detach -d7
```

#### Automatically connect at boot

To have TINC network `ninuxto` active at boot, do the following

```
echo "ninuxto" | sudo tee -a /etc/tinc/nets.boot
sudo service tinc restart
```

<!-- EOF -->
