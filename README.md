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

Add missed libs from the container, e.g. on Fedora 44:

``` deps.sh
docker run --rm --entrypoint bash oooo-ps/tes3mp-forge -c '
    files=(
        libavformat.so.58 libavutil.so.56 libswresample.so.3 libvpx.so.5
        libwebp.so.6 libcrystalhd.so.3 libaom.so.0 libcodec2.so.0.8.1
        libshine.so.3 libx264.so.155 libx265.so.165 libpcre.so.3 libsndio.so.7.0
        libavcodec.so.58 libswscale.so.5 libbluray.so.2 libssh-gcrypt.so.4
    )
    paths=()
    for f in "${files[@]}"; do
        if [ -f "/usr/lib/$f" ]; then paths+=("/usr/lib/$f")
        elif [ -f "/usr/lib/x86_64-linux-gnu/$f" ]; then paths+=("/usr/lib/x86_64-linux-gnu/$f")
        elif [ -f "/lib/x86_64-linux-gnu/$f" ]; then paths+=("/lib/x86_64-linux-gnu/$f")
        fi
    done
    tar -chf - "${paths[@]}" 2>/dev/null
' | tar -xf - -C ./lib --strip-components=3 --no-same-permissions
```

Setup game asset:

``` bash
./tes3mp-wizard
```

Play:

``` bash
./tes3mp
```
* use `tes3mp-client-default.cfg` as the `--connect` argument is yet not supported
