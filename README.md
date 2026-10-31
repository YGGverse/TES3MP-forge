# TES3MP-forge

A container to simplify the packaging of TES3MP for GNU/Linux with fixes for newer Linux systems with IPv6 support (single-stack)

This Docker image creates a Debian Buster build environment, compiles a bunch of dependencies with parameters to make the resulting TES3MP GNU/Linux package work on most distros, and finally summons [TES3MP-deploy](https://github.com/YGGverse/TES3MP-deploy) to build and package TES3MP automatically.

Available for `linux/amd64` and `linux/arm/v7`.

## Install

``` bash
git clone https://github.com/YGGverse/TES3MP-forge.git
cd TES3MP-forge
docker build --tag yggverse/tes3mp-forge --build-arg BUILD_THREADS=4 .
docker run --rm -it -v /tmp/TES3MP:/build yggverse/tes3mp-forge
```
* unpack `/tmp/TES3MP/*.tar.gz` to destination

## Setup game asset

``` bash
./openmw-wizard
```

## Setup connection

`tes3mp-client-default.cfg`:

``` tes3mp-client-default.cfg
[General]
destinationAddress = 202:68d0:f0d5:b88d:1d1a:555e:2f6b:3148
..
[Master]
address = localhost
```

`tes3mp-server-default.cfg`:

``` tes3mp-server-default.cfg
[General]
localAddress = 202:68d0:f0d5:b88d:1d1a:555e:2f6b:3148
..
[MasterServer]
enabled = false
```

## Play

``` bash
./tes3mp
```
* use `tes3mp-client-default.cfg` as the `--connect` argument is yet not supported
