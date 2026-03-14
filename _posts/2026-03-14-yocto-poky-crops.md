---
layout: post
title: "Testing Poky Yocto image with QEMU under CROPS"
date: 2026-03-14
author: pokyuser
tags: yocto, qemu, crops, poky, bitbake, docker, modprobe, tun
---

In my first steps in Yocto, I followed the official tutorial and ran into a few issues.
Here's a helper/reminder.

## Step 1 : Setting up the host machine 

My machine :
* Intel i5 1.60GHz 8 cores 
* 8 GB RAM
* 1 GB Swap
* SSD 256GB  
* Debian 13 testing

Yocto requirements :
* At least 140 GB of free disk space
* At least 32 GB of RAM

I cannot add physical RAM so I'm going to extend the swap :

```bash
fallocate -l 32G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
```
To get the docker image :

```bash
docker pull crops/poky
```
Finally we clone bitbake repository :

```bash
git clone https://git.openembedded.org/bitbake
```

Building and running images will be in the container so we don't need to install any packages.

## Step 2 : Building core-image-minimal 

Time to create the container with all necessary tools and mount your workspace.
I chose to create a separate build directory so I mounted my entire workspace during the setup :

```bash
docker run -it --rm -v /home/$USER/workspace:/workdir crops/poky
```

Once the build directory is created only this directory will be mounted :

```bash
docker run -it --rm -v /home/$USER/workspace/yocto-build:/workdir/yocto-build crops/poky
```

You can then create your image. During my first step I created the smallest image : 
```bash
source /workdir/yocto-build/poky-master/build/init-build-env
bitbake core-image-minimal
```

The build lasted approximately 4 hours. Logs of each recipes can be found in 
```bash
build/tmp/work/*
```

Example : 

```bash
build/tmp/work/x86-64-v3-poky-linux/gcc/15.2.0/temp/log.do_compile
```

Where log.do_compile is a symlink to the last compilation log ASCII text file

## Step 3 : Listing artifacts

In the deploy folder you can list all the build products :

```bash
ls build/tmp/deploy/images/*/
total 320M
bzImage -> bzImage--6.18.13+git0+9b173d3a50_c9dde4c9d3-r0-qemux86-64-20260312190725.bin
bzImage--6.18.13+git0+9b173d3a50_c9dde4c9d3-r0-qemux86-64-20260312190725.bin
bzImage-qemux86-64.bin -> bzImage--6.18.13+git0+9b173d3a50_c9dde4c9d3-r0-qemux86-64-20260312190725.bin
core-image-minimal-qemux86-64.rootfs-20260312190725.ext4
core-image-minimal-qemux86-64.rootfs-20260312190725.ext4.zst
core-image-minimal-qemux86-64.rootfs-20260312190725.manifest
core-image-minimal-qemux86-64.rootfs-20260312190725.qemuboot.conf
core-image-minimal-qemux86-64.rootfs-20260312190725.spdx.json
core-image-minimal-qemux86-64.rootfs-20260312190725.tar.zst
core-image-minimal-qemux86-64.rootfs-20260312190725.testdata.json
core-image-minimal-qemux86-64.rootfs.ext4.zst -> core-image-minimal-qemux86-64.rootfs-20260312190725.ext4.zst
core-image-minimal-qemux86-64.rootfs.manifest -> core-image-minimal-qemux86-64.rootfs-20260312190725.manifest
core-image-minimal-qemux86-64.rootfs.qemuboot.conf -> core-image-minimal-qemux86-64.rootfs-20260312190725.qemuboot.conf
core-image-minimal-qemux86-64.rootfs.spdx.json -> core-image-minimal-qemux86-64.rootfs-20260312190725.spdx.json
core-image-minimal-qemux86-64.rootfs.tar.zst -> core-image-minimal-qemux86-64.rootfs-20260312190725.tar.zst
core-image-minimal-qemux86-64.rootfs.testdata.json -> core-image-minimal-qemux86-64.rootfs-20260312190725.testdata.json
modules--6.18.13+git0+9b173d3a50_c9dde4c9d3-r0-qemux86-64-20260312190725.tgz
modules-qemux86-64.tgz -> modules--6.18.13+git0+9b173d3a50_c9dde4c9d3-r0-qemux86-64-20260312190725.tgz
```
Where the bin file is the kernel boot executable, and ext4.zst is the compressed rootfs

# Step 4 : Running image under QEMU 

Since we have all the necessary files you can use runqemu

```bash
runqemu qemux86-64 slirp nographic snapshot
```

runqemu automatically locates the kernel and the root filesystem and launch qemu with the appropriate parameters

* slirp : enable networking without need of root privilege 
* nographic : disable video console
* snapshot : don't write change back to image

Instead of snapshot mode you can decompress the rootfs and use it with :

```bash
zstd -d core-image-minimal-qemux86-64.rootfs-20260312190725.ext4.zst
runqemu qemux86-64 slirp nographic
```

Some issues can be resolved by adding TUN/TAP module to your host and add device to you container :

```bash
modprobe tun
```

```bash
docker run -it --rm -v /home/$USER/workspace/yocto-build:/workdir/yocto-build --device=/dev/net/tun poky/crops
```

Of course you can launch qemu on your host (or any other machine) with :

```bash
zstd -d core-image-minimal-qemux86-64.rootfs-20260312190725.ext4.zst
qemu-system-x86_64 -cpu max -kernel bzImage -drive file=core-image-minimal-qemux86-64.rootfs-20260312190725.ext4,format=raw -append "root=/dev/sda console=ttyS0" -m 2048 -nographic
```

<details>
<summary>kernel boot log (click me)</summary>

<div style="max-height:400px; overflow:auto; font-family:monospace; font-size:0.9em; padding:5px; background:#f8f8f8; border:1px solid #ddd;">
<pre>
{% include logs/boot.log %}
</pre>
</div>

</details>
