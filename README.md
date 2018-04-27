# tinc-ninuxto

Ninux-Torino TINC network configuration

Copyright (C) 2016-2017, [Gianpaolo Macario](https://gmacario.github.io/)

## Assigned IP addresses inside TINC network `ninuxto`

According to <http://wiki.ninux.org/GestioneIndirizzi>, IPv4 subnet `10.23.0.0/16` has been assigned to ninux-torino.

| VPN_IP_Address | Hostname      | Admin      | OS_Version                        | Notes                          |
|----------------|---------------|------------|-----------------------------------|--------------------------------|
| ...            |               |            |                                   |                                |
| 10.23.3.0/24   | -             | gmacario   | -                                 | Subnet reserved for gmacario   |
| 10.23.3.1      | tincgm01      | gmacario   | Ubuntu 16.04 64-bit               | Test VM on VirtualBox          |
| ...            |               |            |                                   |                                |
| 10.23.3.20     | kruk          | gmacario   | Ubuntu 16.04 64-bit               | gateway for gmoffice           |
| 10.23.3.21     | tincgw21      | gmacario   | Ubuntu 14.04 64-bit               | Instance on AWS                |
| 10.23.3.22     | mygmcont22    | gmacario   | Ubuntu 16.04 64-bit               | Inside a Docker container      |
| 10.23.3.23     | rpi3gm23      | gmacario   | Raspbian GNU/Linux 8 (jessie)     | Gateway for gmhome             |
| 10.23.3.24     | itmgmacariow7 | gmacario   | Ubuntu-desktop 16.04.2 LTS 64-bit | Laptop Dell Precision          |
| 10.23.3.25     | rpi2gm89      | gmacario   | Raspbian GNU/Linux 9 (stretch)    | Raspberry Pi 2                 |
| ...            |               |            |                                   |                                |
| 10.23.3.30     | udooneomv30   | gmacario   | UDOObuntu2.0rc2                   | UDOO NEO Full + lora-shield    |
| 10.23.3.31     | udooneogm01   | gmacario   | UDOObuntu 2.2.0 Minimal (14.04 LTS) | UDOO NEO Full                |
| 10.23.3.32     | chipgm32      | gmacario   | Debian GNU/Linux 8 (jessie)       | C.H.I.P.                       |
| 10.23.3.33     | ardyungm33    | gmacario   | OpenWrt-Yun 1.5.3                 | Gateway for solpev             |
| ...            |               |            |                                   |                                |

## See also

* [Configuring TINC `ninuxto` on a UDOO NEO](configuring-tinc-ninuxto-on-udoobuntu2.md)
* http://wiki.ninux.org/GestioneIndirizzi

<!-- EOF -->
