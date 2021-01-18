OpenCDI tools for containerized VDI developpers (alpha release)

OpenCDI is a light-weight VDI implementation using Linux containers.
This project aims to be a simple, loose coupled, open and vendor neutral CLI interfaces for CDI users and Linux container (docker, podman, etc.) users.

# Features

* cdictl and @opencdi shelib module for cdi control interface
* cosh API integration
* Xsession scripts (cdi-launch\*) for xinit and login sessions

At now, outbound connection not supported but you can integrate the feature with some of VNC (or Web-based VNC) implementations. 
(XForwarding not recommended)

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
export FROM_XINIT=1
export SUDOING=1
cdictl up 
```

Execute `startx` as an user in wheel/sudo group.

## (2) xinit with rootless docker

``` ~/.xinitrc
export FROM_XINIT=1
export CDI_ROOTLESS=1
cdictl up 
```

## (3) LightDM desktop manager example

1. add xsession desktop file:

``` /usr/share/xsession/cdi-mate.desktop
[Desktop Entry]
Name[en_GB]=CDI Mate
Name=CDI Mate
Comment[en_GB]=This session logs you into CDI Mate 
Comment=This session logs you into CDI
Exec=/usr/local/bin/cdi-launch
TryExec=/usr/local/bin/cdi-launch
Type=Application
DesktopNames=CDI-Mate;CDI;
```

2. Restart lightdm or reboot

3. Select `CDI-Mate` session for the login session, input password and login!


## Bugs

This package is alpha release. 
If you are interesting to report bugs and make any improvement, see CONTRIBUTING.md


## Support

opencdi[at]uillos.org
