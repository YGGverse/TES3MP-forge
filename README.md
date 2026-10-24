# TES3MP-forge

A container to simplify the packaging of TES3MP for GNU/Linux with fixes for newer Linux systems with IPv6 support (single-stack)

This Docker image creates a Debian Buster build environment, compiles a bunch of dependencies with parameters to make the resulting TES3MP GNU/Linux package work on most distros, and finally summons [TES3MP-deploy](https://github.com/oooo-ps/TES3MP-deploy) to build and package TES3MP automatically.

Available for `linux/amd64` and `linux/arm/v7`.

## Install

``` bash
git clone https://github.com/oooo-ps/TES3MP-forge.git
cd TES3MP-forge
docker build --tag oooo-ps/tes3mp-forge --build-arg BUILD_THREADS=4 .
docker run --rm -it -v /tmp/TES3MP:/build oooo-ps/tes3mp-forge
```
* unpack `/tmp/TES3MP/*.tar.gz` to destination

Setup game asset:

``` bash
./openmw-wizard
```

Play:

``` bash
./tes3mp
```
* use `tes3mp-client-default.cfg` as the `--connect` argument is yet not supported
