# sysrepo/netopeer2 layer for Yocto Project
# meta-sysrepo
sysrepo and netconf toolset
This README file contains information on the contents of the sysrepo/netopeer2 layer for Yocto Project.

Please see the corresponding sections below for details.

## Dependencies

This layer depends on:
* URI: git://git.openembedded.org/meta-openembedded 
  branch: master

It was tested with yocto project master branch:
* URI: git://git.yoctoproject.org/poky
  branch: master

Build host dependencies:
https://www.yoctoproject.org/docs/current/yocto-project-qs/yocto-project-qs.html#packages

## Adding the layer to your build

In order to use this layer, you need to make the build system aware of it.
First prepare yocto project and all required layers:
```
git clone git://git.yoctoproject.org/poky
cd poky
git clone git://git.openembedded.org/meta-openembedded
git clone https://github.com/sartura/meta-sysrepo
source oe-init-build-env
```
The last command initializes the build environment and your current working directory is set to the `build` directory.
Now add the layer to the build system by adding the location of the sysrepo layer to `conf/bblayers.conf`, along with any other layers needed, e.g.:
```
BBLAYERS ?= " \
  /home/build/poky/meta \
  /home/build/poky/meta-poky \
  /home/build/poky/meta-yocto-bsp \
  /home/build/poky/meta-sysrepo \
  /home/build/poky/meta-openembedded/meta-efl \
  /home/build/poky/meta-openembedded/meta-initramfs \
  /home/build/poky/meta-openembedded/meta-multimedia \
  /home/build/poky/meta-openembedded/meta-networking \
  /home/build/poky/meta-openembedded/meta-oe \
  /home/build/poky/meta-openembedded/meta-python \
  "
```

Update `conf/local.conf` file to include additional software in the final image, e.g. add to the end:
```
IMAGE_INSTALL_append = " sysrepo netopeer2-server netopeer2-cli \
  netopeer2-keystored openssh openssl "
```
Optionally, adapt `MACHINE` variable for target system.

## Build test image

Now a test image can be built, e.g.:
```
bitbake core-image-base
```

The image is located under `tmp/deploy/images/<target>` directory.

Default `MACHINE` target is `qemux86`, so it can be run with:
```
runqemu qemux86
```

## Starting sysrepo and netopeer2-server

There are init.d scripts (as part of the meta-sysrepo layer) which are automatically stored in the image:
* /etc/init.d/sysrepo
* /etc/init.d/netopeer2-server

It is enough to start only `/etc/init.d/netopeer2-server` script and it will make sure `sysrepo` processes are also started.
```
/etc/init.d/netopeer2-server start
```

## Demo
[![asciicast](https://asciinema.org/a/126605.png)](https://asciinema.org/a/126605)
