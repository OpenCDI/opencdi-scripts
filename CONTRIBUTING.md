# Make bug reports, thanks.

There are no format for the repository, but please tell us

* which OS (distros, libc versions, etc.) you used.
* which shell you used. (plan to do `ls -l /bin/sh`)
* which versions of requirements you installed. (docker, shelib, cosh, opencdi)
* how to reproduce the probrem in your environment.
* how the shelib debug message alike (plan to insert -x flag after `cook` invokation at the end of `opencdi` executable)

# PR

If you would like to improve opencdi tools, following developer's environment recommended :

* debian 10.5+
* dash 0.5.2+
* busybox 1.30+
* bash-5.0+
* install docker.io with apt
* or install rootless docker following to [this instruction](https://docs.docker.com/engine/security/rootless/).
