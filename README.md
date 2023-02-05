# Docker Kernel for Sony pdx206

A Docker compatible Android kernel for Sony Xperia 5 II (pdx206).

可透過Termux於Android手機原生執行Docker容器的Sony Xperia 5 II核心。

This kernel adds supports for running Docker on Android. binfmt moudle is also included. It is based on [LineageOS kernel for sm8250](https://github.com/LineageOS/android_kernel_sony_sm8250)

![](https://i.postimg.cc/3RDgdndZ/Screenshot-20230203-183742-Termux.png)


## Usage

Tested on LineageOS 20 unofficial build. A prebuilt boot.img is available at [Releases](https://github.com/ivon852/android_kernel_sony_pdx206_docker/releases).

1. Flash the boot.img to `boot` partition
```bash
fastboot flash boot boot.img
```

2. Install Docker in Termux
```bash
pkg install root-repo
pkg install docker docker-compose
```

3. Mount cgroups
```bash
sudo mount -t tmpfs -o uid=0,gid=0,mode=0755 cgroup /sys/fs/cgroup
```

4. Start docker daemon
```bash
sudo dockerd --iptables=false
```

5. Test running
```bash
sudo docker run hello-world
```

6. You can also enable binfmt to run x86 docker images on ARM.
```bash
su
mount binfmt_misc -t binfmt_misc /proc/sys/fs/binfmt_misc
echo 1 > /proc/sys/fs/binfmt_misc/status
```


## Compliation Guide

Please refer to [LineageOS Wiki](https://wiki.lineageos.org/devices/pdx203/build) to learn how to build LineageOS ROM for your device.

The configuration for running docker is already written in `pdx206_defconfig`.

To build kernel only, run:
```bash
source build/envsetup.sh
breakfast pdx206
export ARCH=arm64
make clean
mka bootimage
```


## See also

[Freddie Oliveira's Tutorial of running Docker on Android](https://gist.github.com/FreddieOliveira/efe850df7ff3951cb62d74bd770dce27)
