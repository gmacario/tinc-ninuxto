# tinc-ninuxto

Ninux-Torino TINC network configuration

Copyright (C) 2016-2019, [Gianpaolo Macario](https://gmacario.github.io/)

## Assigned IP addresses inside TINC network `ninuxto`

According to <https://ipam.ninux.org/>, IPv4 subnet `10.23.0.0/16` has been assigned to ninux-torino.

| VPN_IP_Address | Hostname      | Admin      | OS_Version                        | Notes                          |
|----------------|---------------|------------|-----------------------------------|--------------------------------|
| ...            |               |            |                                   |                                |
| 10.23.3.0/24   | -             | [gmacario](https://github.com/gmacario) | -    | Subnet reserved to Gianpaolo Macario |
| 10.23.3.1      | tincgm01      | gmacario   | Ubuntu 16.04 64-bit               | Test VM on VirtualBox          |
| ...            |               |            |                                   |                                |
| 10.23.3.20     | kruk          | gmacario   | Ubuntu 16.04 64-bit               | gateway for gmoffice           |
| 10.23.3.21     | tincgw21      | gmacario   | Ubuntu 14.04 64-bit               | Instance on AWS                |
| 10.23.3.22     | mygmcont22    | gmacario   | Ubuntu 16.04 64-bit               | Inside a Docker container      |
| 10.23.3.23     | rpi3gm23      | gmacario   | Raspbian GNU/Linux 8 (jessie)     | Gateway for gmhome             |
| 10.23.3.24     | itmgmacariow7 | gmacario   | Ubuntu-desktop 16.04.4 LTS 64-bit (Xenial Xerus) | Laptop Dell Precision |
| 10.23.3.25     | rpi2gm89      | gmacario   | Raspbian GNU/Linux 9 (stretch)    | Raspberry Pi 2                 |
| 10.23.3.26     | rpi3gm26      | gmacario   | Raspbian Stretch Lite April 2018  | Raspberry Pi 3B                |
| 10.23.3.27     | iongmacario   | gmacario   | Ubuntu 18.04.1 64-bit             | Inside a Docker container      |
| 10.23.3.28     | rpi3pgm28     | gmacario   | Raspbian Stretch Lite June 2018   | Raspberry Pi 3B Plus           |
| 10.23.3.29     | rpi3pgm29     | gmacario   | Raspbian Stretch Desktop June 2018 | Raspberry Pi 3B Plus in RasPad |
| 10.23.3.30     | udooneomv30   | gmacario   | UDOObuntu2.0rc2                   | UDOO NEO Full + lora-shield    |
| 10.23.3.31     | udooneogm01   | gmacario   | UDOObuntu 2.2.0 Minimal (14.04 LTS) | UDOO NEO Full                |
| 10.23.3.32     | udooneogm02   | gmacario   | UDOObuntu 2.2.0 Desktop (14.04 LTS) | UDOO NEO Extended            |
| 10.23.3.33     | ardyungm33    | gmacario   | OpenWrt-Yun 1.5.3                 | Gateway for solpev             |
| 10.23.3.34     | udoox86gm1    | gmacario   | Ubuntu 18.04.2 LTS 64-bit         | New gateway for solpev         |
| ...            |               |            |                                   |                                |
| 10.23.4.0/24   | -             | MarcoTo    | -                                 | Subnet reserved to Marco Toscano |
| ...            |               |            |                                   |                                |
| 10.23.5.0/24   | -             | 61615m1    | -                                 | Subnet reserved to Luigi Smiraglio |
| ...            |               |            |                                   |                                |
| 10.23.6.0/24   | -             | [Muwattalli](https://github.com/muwattalli) | - | Subnet reserved to Gianfranco Poncini |
| ...            |               |            |                                   |                                |
| 10.23.25.0/24  |               | [GTeti](https://github.com/gteti) | -          |  Subnet reserved to Gianluca Teti |
| 10.23.25.2     |rpi2gt64       | GTeti      | Raspbian 9.9                      | Raspberry Pi 2                 |
| ...            |               |            |                                   |                                |

## See also

* [Configuring TINC `ninuxto` on a UDOO NEO](docs/configure-tinc-ninuxto-on-udoobuntu.md)
* [Running TINC `ninuxto` inside a Docker container](docs/configure-tinc-ninuxto-docker.md)
* Gestione Indirizzi di Ninux.org: <https://ipam.ninux.org/>
  * LAN Subnet Ninux.org Torino: 10.23.0.0/16
  * Ninux.org addresses previously managed with http://wiki.ninux.org/GestioneIndirizzi

<!-- EOF -->
