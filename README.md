# macchina.io REMOTE Device Agent and Gateway OpenWRT Packages

OpenWRT package feed meta data for macchina.io REMOTE device agent
([WebTunnelAgent](https://github.com/my-devices/sdk/blob/master/WebTunnel/WebTunnelAgent/README.md))
and [gateway](https://github.com/my-devices/gateway).

## About macchina.io REMOTE

[macchina.io REMOTE](https://macchina.io/remote) provides secure remote access to connected devices
via HTTP or other TCP-based protocols and applications such as secure shell (SSH) or
Virtual Network Computing (VNC). With macchina.io REMOTE, any network-connected device
running the macchina.io REMOTE Device Agent or this Gateway program can be securely accessed remotely over the
internet from browsers, mobile apps, desktop, server or cloud applications.

This even works if the device is behind a NAT router, firewall or proxy server.
The device becomes just another host on the internet, addressable via its own URL and
protected by the macchina.io REMOTE server against unauthorized or malicious access.
macchina.io REMOTE is a great solution for secure remote support and maintenance,
as well as for providing secure remote access to devices for end-users via web or
mobile apps.

## About this Repository

This repository contains package sources for building OpenWRT packages
for the macchina.io REMOTE device agent (`rmagent`) and gateway (`rmgateway`).

## Building the Packages

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
$ ./scripts/feeds install -a
```

4. Configure the SDK (default)

```
$ make defconfig
```

5. Compile the packages

```
$ make package/rmagent/compile
$ make package/rmgateway/compile
```
