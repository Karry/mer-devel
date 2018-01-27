# Mer devel

Various experimental tools and script for developing and testing 
Mer (merproject.org) components.

## PIM testing docker image

This is docker image based on `x86_64` Ubuntu Artful with installed
few Mer PIM components for easy development and testing without 
complicated, Stcratchbox based Mer SDK.

Why this image is based on Ubuntu docker? I just have good 
experience with developing inside docker, and I believe 
that it may be usefull to identify and minimise troubles 
caused by using different platform (with different version 
of compiler and build tools).

```
docker build --tag mer-pim pim-docker
docker run --rm=true --privileged -it mer-pim /bin/bash
```

