# Mer devel

Various experimental tools and script for using developing Mer (merproject.org) components.

## PIM testing docker image

```
docker build --tag mer-pim pim-docker
docker run --rm=true --privileged -it mer-pim /bin/bash
```

