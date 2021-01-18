OpenCDI tools for containerized VDI operators/developers (alpha release)

OpenCDI is a light-weight VDI implementation using Linux containers.
This project aims to be a simple, loose coupled, open and vendor neutral CLI interfaces for CDI users and Linux container (docker, podman, etc.) users.

# Features

* cdictl command and @opencdi shelib module for cdi control interface
* cosh API integration
* Xsession scripts (cdi-launch\*) for xinit and login sessions

At now, outbound connection not supported but you can integrate the feature with some of VNC (or Web-based VNC) implementations. 
(XForwarding not recommended)

# Supported platforms

* amd64 archtecture (at now)
* GNU/Linux or its derivetives (Linux Namespaces and Docker support needed)
* posix mode of bash, dash, /bin/sh of busybox and some of other posix compatible shells
* Docker (podman, lxc/lxd or [orchestrator](https://github.com/tanban-oss/opencdi/WHY_NOT_ORCHESTRATOR_NOW.md) not supported now.)
* At now, shell scripting runtime based on shelib 0.5+ (because of PoC convinience)

# Requirements

* bash, dash, busybox sh or other POSIX compatible /bin/sh
* Linux kernel >= 4.9 (namespace support required for rootless modes)
* shelib >= v0.5.0
* cosh >= v0.1.0
* docker >= 19.01.0 (for non-rootless mode)
* docker-rootless (for rootless mode, required in $HOME/bin)

# Install

```
shef install https://github.com/tanban-oss/opencdi
```

For the reduce of the time of CDI startup, pull coshapp image before the first invokation of cdictl.

```
for i in admin rootless firefox thunderbird libreoffice; do
  docker pull coshapp/$i:debian-10.7
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

sudo install -v -m 755 misc/Scripts/debian/cdi-launch\* /usr/local/bin/
sudo install -v -m 644 misc/xsession/debian/\* /usr/share/xsessions/

```

3. make symbolic links for OpenCDI related executables (if you don't install shelib globally)

```

for i in opencdi cdictl cook; do sudo ln -sv $(readlink $i) /usr/local/bin/; done
sudo ln -sv /absolute_path_to_repo/misc/Scripts/debian/cdi-\* /usr/local/bin/

```

4. Restart lightdm or reboot

```

sudo systemctl restart lightdm

```

5. Select `CDI-Mate` session for the login session, input password and login!

OpenCDI has several deployment patterns (plain dom0, rind, plain rootless, and so on.). 
Choose most beneficial mode for your use cases.
You can also write a new launcher and corresponding desktop entry to do that.


## Bugs

* This package is alpha release. (buggy)
* If you want to use rootless mode for OpenCDI, check your DOCKER_HOST for valid (or ideal) action inside the administrative container (dom0). 
* If you are interesting to report bugs and make any improvement, see CONTRIBUTING.md

## Support

opencdi[at]tanban.org
