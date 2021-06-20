### OpenCDI tools for containerized VDI operators/developers (alpha release)

# SUGGESTION for watchers

We are sorry that this repo has been `private` until June 20 2021.
This is public now, but archive for several reasons.

* shelib runtime is not well-maintained at now.
* We will rewrite cdictl and cosh (maybe it will be written in the other language)

# About

**OpenCDI** is a light-weight and open VDI toolset using Linux containers.
This project aims to be a simple, loose coupled, open and vendor neutral CLI interfaces for CDI users and Linux container users(docker, podman, etc.).
This tool (PoC) is built on top of [shelib](https://github.com/shimmortal/shelib) runtime (version 5+) and userspaces can be containerized over almost all of your Linux desktop experience lifetime. 

You can be logged in with an user inside the administrative container or non-administrative container.

There are several features supporting userspace transition of the graphical environment, from outside to inside container:

* cdictl CLI and @opencdi shelib module for cdi control interface
* cosh API integration and public (and easly modifiable) coshapp docker images 
* Xsession scripts (cdi-launch\*) and desktop entry files (\*.desktop) for xinit and display managers

At now, inbound network connection not supported. But you can integrate some of VNC (or Web-based VNC) implementations for that.

# Supported platforms

* amd64 archtecture (currently testing)
* GNU/Linux or its derivatives (Linux Namespaces and Docker support needed)
* posix mode of bash, dash, /bin/sh of busybox and some of other posix compatible shells
* Docker (podman, lxc/lxd or orchestrators not supported now.)
* shell scripting runtime based on shelib 0.5+ (because of the auther's PoC convinience)

# Requirements

* bash, dash, busybox sh or other POSIX compatible /bin/sh
* Linux kernel >= 4.9 (namespace support required for rootless modes)
* [shelib](https://github.com/shimmortal/shelib) >= v0.5.0
* [cosh](https://github.com/shimmortal/cosh) >= v0.1.0
* docker >= 19.01.0  (for rootless mode. Read to met the [prerequisity](https://docs.docker.com/engine/security/rootless/#prerequisites).)

# Install

```
shef install https://github.com/opencdi/opencdi-scripts
```

For the reduce of the time of CDI startup, pull coshapp image before the first invokation of cdictl.

```
for i in core admin firefox thunderbird libreoffice; do
  docker pull coshapp/$i:debian-10.9
done
```

# Quick start

## (1) xinit

``` ~/.xinitrc
xhost +local:0
export DISPLAY=:0
export FROM_XINIT=1
export SUDOING=1
cdictl up 
```

Execute `startx` as an user in wheel/sudo group.

Because of docker's design on [permission control](https://docs.docker.com/engine/security/#docker-daemon-attack-surface), we recommend to use docker with sudo and [userns-remap](https://docs.docker.com/engine/security/userns-remap/), or use [rootless](https://docs.docker.com/engine/security/rootless/).

## (2) xinit with rootless docker

``` ~/.xinitrc
xhost +local:0
export DISPLAY=:0
export FROM_XINIT=1
export CDI_ROOTLESS=1
cdictl up 
```

## (3) LightDM desktop manager example

1. check shelib and opencdi installation

```
$ cook -V
shelib-0.5.0 (@core/cook)
$ opencdi help --short
opencdi-0.0.1 (@opencdi/opencdi)
```

2. install the OpenCDI launcher and xsession desktop entry

```
$ sudo install -v -m 755 misc/Scripts/debian/cdi-launch\* /usr/local/bin/
$ sudo install -v -m 644 misc/xsession/debian/\* /usr/share/xsessions/
```

3. make symbolic links for OpenCDI related executables (if you don't install shelib globally)

```
$ for i in opencdi cdictl cook; do sudo ln -sv $(readlink $i) /usr/local/bin/; done
$ sudo ln -sv /absolute_path_to_repo/misc/Scripts/debian/cdi-\* /usr/local/bin/
```

4. Restart lightdm or reboot

```
$ sudo systemctl restart lightdm
```

5. Select `CDI-Mate` session for the login session, input password and login!

OpenCDI has several deployment patterns (plain dom0 (dangerous), rind, plain rootless, and so on.). 
Choose most beneficial mode for your use cases.
You can also write a new launcher and corresponding desktop entry to do that.


## Bugs

* This package is alpha release. (buggy)
* If you want to use rootless mode for OpenCDI, check your DOCKER_HOST for valid (or ideal) action inside the administrative container (dom0). 

## Support

opencdi[at]tanban.org
