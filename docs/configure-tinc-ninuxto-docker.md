# Running TINC 1.1pre15 inside a Docker container

<!-- 2018-08-16 19:30 CEST -->

### References 

* Docker image sources: <https://github.com/JensErat/docker-tinc>
* tinc v1.1 manual: <https://www.tinc-vpn.org/documentation-1.1/>
* tinc VPN homepage: <https://www.tinc-vpn.org/>

### Prerequisites

* A host running Docker CE 18.03
  - Tested on iongmacario (Ubuntu 18.04.1 LTS 64-bit)
  
### Install tinc in a Docker container

#### Verify prerequisites

Login as gmacario@iongmacario and try running a Docker container

```
gmacario@iongmacario:~$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
9db2ca6ccae0: Pull complete
Digest: sha256:4b8ff392a12ed9ea17784bd3c9a8b1fa3299cac44aca35a85c90c5e3c7afacdc
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/

gmacario@iongmacario:~$
```

#### Print tinc command line help

Login as gmacario@iongmacario

```shell
docker run --rm jenserat/tinc --help
```

#### Prepare the configuration

Login as gmacario@iongmacario

Create configuration file

```shell
mkdir -p $HOME/tinc-config
docker run --rm --volume $HOME/tinc-config:/etc/tinc jenserat/tinc \
  -n ninuxto init iongmacario
```

Add known hosts

```shell
cd $HOME/tinc-config/ninuxto/hosts
sudo chown $USER .
curl -O -L https://github.com/gmacario/tinc-ninuxto/raw/master/hosts/rpi3gm23
curl -O -L https://github.com/gmacario/tinc-ninuxto/raw/master/hosts/tincgw21
curl -O -L https://github.com/gmacario/tinc-ninuxto/raw/master/hosts/udooneogm01
sudo chmod 644 *
```

Modify the initial configuration

```shell
docker run --rm --volume $HOME/tinc-config:/etc/tinc jenserat/tinc \
  -n ninuxto add subnet 10.23.3.27/32
docker run --rm --volume $HOME/tinc-config:/etc/tinc jenserat/tinc \
  -n ninuxto add connectto rpi3gm23
docker run --rm --volume $HOME/tinc-config:/etc/tinc jenserat/tinc \
  -n ninuxto add connectto udooneogm01
```

Configure script `$HOME/tinc-config/ninuxto/tinc-up`

```diff
gmacario@iongmacario:~/tinc-config/ninuxto$ diff -uw tinc-up.ORIG tinc-up
--- tinc-up.ORIG        2018-08-17 08:06:06.966615850 +0200
+++ tinc-up     2018-08-17 08:06:48.965671907 +0200
@@ -1,5 +1,5 @@
 #!/bin/sh

-echo 'Unconfigured tinc-up script, please edit '$0'!'
+ifconfig $INTERFACE 10.23.3.27 netmask 255.255.0.0

-#ifconfig $INTERFACE <your vpn IP address> netmask <netmask of whole VPN>
+# EOF
gmacario@iongmacario:~/tinc-config/ninuxto$
```

#### Start tinc daemon

```shell
docker run -d --rm \
  --name tinc \
  --net=host \
  --device=/dev/net/tun \
  --cap-add NET_ADMIN \
  --volume $HOME/tinc-config:/etc/tinc \
  jenserat/tinc \
  start -D -n ninuxto -U nobody -d0
```

### Runtime commands

When the tinc daemon is running in a container you can issue runtime commands as arguments to `docker exec tinc ...`

#### Dump reachable nodes

<!-- 2018-08-17 07:48 CEST -->

```shell
docker exec tinc \
  tinc -n ninuxto dump reachable nodes
```

#### Dump network as a digraph

<!-- 2018-08-17 08:12 CEST -->

```shell
docker exec tinc \
  tinc -n ninuxto dump digraph
```

Result:

```
digraph {
 ardyungm33 [label = "ardyungm33", color = "red"];
 chipgm32 [label = "chipgm32", color = "red"];
 iongmacario [label = "iongmacario", color = "green", style = "filled"];
 itmgmacariow7 [label = "itmgmacariow7", color = "red"];
 rpi2gm89 [label = "rpi2gm89", color = "black"];
 rpi3gm23 [label = "rpi3gm23", color = "green"];
 tincgw21 [label = "tincgw21", color = "black"];
 udooneogm01 [label = "udooneogm01", color = "black"];
 udooneogm02 [label = "udooneogm02", color = "black"];
 iongmacario -> rpi3gm23 [w = 297.542999, weight = 297.542999];
 rpi2gm89 -> rpi3gm23 [w = 79.864021, weight = 79.864021];
 rpi2gm89 -> tincgw21 [w = 157.410507, weight = 157.410507];
 rpi3gm23 -> iongmacario [w = 297.542999, weight = 297.542999];
 rpi3gm23 -> rpi2gm89 [w = 79.864021, weight = 79.864021];
 rpi3gm23 -> tincgw21 [w = 207.738174, weight = 207.738174];
 rpi3gm23 -> udooneogm01 [w = 211.726685, weight = 211.726685];
 rpi3gm23 -> udooneogm02 [w = 105.025398, weight = 105.025398];
 tincgw21 -> rpi2gm89 [w = 157.410507, weight = 157.410507];
 tincgw21 -> rpi3gm23 [w = 207.738174, weight = 207.738174];
 tincgw21 -> udooneogm01 [w = 188.782242, weight = 188.782242];
 tincgw21 -> udooneogm02 [w = 67.264915, weight = 67.264915];
 udooneogm01 -> rpi3gm23 [w = 211.726685, weight = 211.726685];
 udooneogm01 -> tincgw21 [w = 188.782242, weight = 188.782242];
 udooneogm02 -> rpi3gm23 [w = 105.025398, weight = 105.025398];
 udooneogm02 -> tincgw21 [w = 67.264915, weight = 67.264915];
}
```

You can then convert the digraph to a PDF using [Graphviz](http://www.graphviz.org/)

```shell
docker exec tinc \
  tinc -n ninuxto dump digraph \
  | circo -Tpdf >tinc-network.pdf
```

Alternatively view it online with <http://sandbox.kidstrythisathome.com/erdos/>

#### Display information about a node

Example:

```
gmacario@iongmacario:~/tinc-config/ninuxto$ docker exec tinc  tinc -n ninuxto info tincgw21
Node:         tincgw21
Node ID:      d33bffc03796
Address:      52.58.214.157 port 655
Online since: 2018-08-17 06:07:42
Status:       visited reachable
Options:      pmtu_discovery clamp_mss
Protocol:     17.0
Reachability: unknown
Edges:        rpi2gm89 rpi3gm23 udooneogm01 udooneogm02
Subnets:      10.23.3.21
gmacario@iongmacario:~/tinc-config/ninuxto$
```

#### Display runtime network statistics

```shell
docker exec -ti tinc \
  tinc -n ninuxto top
```

<!-- EOF -->
