OpenCDI tools for developpers

# About

OpenCDI is a light-weight VDI implementation using Linux containers.
This package is composed of three parts:

* cdictl and @opencdi shelib module for cdi control interface
* Xsession scripts (cdi-launch\*) for xinit and login sessions
* Dockerfiles for admin/rootless images for CDI users/operators

# Requirements

bash, ash, dash, busybox sh or other POSIX compatible /bin/sh
Linux kernel >= 4.9 (namespace support required for rootless modes)
shelib >= v0.5.0
docker >= 19.01.0
docker-rootless (for rootless mode, required in $HOME/bin)

# Install

```
shef install https://github.com/opencdi
```

For the reduce of the time of CDI startup, pull coshapp image before the first invokation of cdictl.

```
for i in admin firefox thunderbird libreoffice; do
  docker pull coshapp/$i:debian-10.5
done
```

# Quick start

## (1) for xinit

``` ~/.xinitrc
export FROM_XINIT=1
export SUDOING=1
cdictl up 
```

Execute `startx` in multi-user mode.

## (2) LightDM desktop manager example

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

If some misterious messages occur, please make issue or fix it. See CONTRIBUTING.md


## Support

Reports and questions are welcomed in Github issues or from contact below:

opencdi@lab.sysnk.net
