# meta-agent-openwrt

OpenWRT package feed meta data for macchina.io REMOTE device agent (WebTunnelAgent) and gateway.

This repository contains OpenWRT package sources for building OpenWRT packages
for the macchina.io REMOTE device agent (rmagent) and gateway (rmgateway).

# Building

The recommended way for building the packages is by using the [OpenWRT SDK docker](https://hub.docker.com/r/openwrtorg/sdk)
images. It's of course also possible to build using a standard OpenWRT SDK.

To launch an OpenWRT SDK build environment in a Docker container, use one of the
official OpenWRT SDK Docker images, suitable for your target device.

```
$ docker run --rm -v "$(pwd)"/openwrt/bin:/home/build/openwrt/bin -v "$(pwd)"/openwrt/src:/home/build/openwrt/src -it openwrtorg/sdk:ar71xx-generic-19.07.8
```

This repository can be used as a package feed, so the steps to build the packages
are as follows:

1. Add this repository to the OpenWRT `feeds.conf.default` or `feeds.conf` file.

```
$ echo 'src-git macchina_remote https://github.com/my-devices/meta-agent-openwrt.git' >>feeds.conf.default
```

2. Update the feeds

```
$ ./scripts/feeds update base
$ ./scripts/feeds update macchina_remote
```

3. Install the feeds

```
$ ./scripts/feeds install macchina_remote
```

4. Configure the SDK (default)

```
$ make defconfig
```

5. Build the packages

```
$ make package/rmagent/compile
$ make package/rmgateway/compile
```
