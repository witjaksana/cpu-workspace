# CPU Workspace
This repository contains my experiment on CPU performance

## Compilation
### qemu
```
git clone --recursive https://github.com/witjaksana/qemu.git iss-qemu
cd qemu
./runcompile.sh
```

### buildroot
wget -c https://buildroot.org/downloads/buildroot-2024.02.3.tar.gz
tar xvf buildroot-2024.02.3.tar.gz
```
cd buildroot-2024.02.3
make ARCH=arm64 qemu_aarch64_virt_defconfig
make ARCH=arm64 menuconfig

BR2_TARGET_ROOTFS_CPIO=y
BR2_TARGET_GENERIC_GETTY=y
BR2_TARGET_GENERIC_GETTY_PORT=”ttyAMA0″

sudo make -j8
```

### linux
wget -c https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.9.tar.xz
tar xvf linux-6.9.tar.xz

```
cd linux-6.9
make CROSS_COMPILE=aarch64-none-linux-gnu- ARCH=arm64 defconfig

CONFIG_CMDLINE=”console=ttyAMA0″
CONFIG_INITRAMFS_SOURCE=”<buildroot>/output/images/rootfs.cpio”

make CROSS_COMPILE=aarch64-none-linux-gnu- ARCH=arm64 menuconfig
make CROSS_COMPILE=aarch64-none-linux-gnu- ARCH=arm64 -j8
```

## Running
```
qemu/build/qemu-system-aarch64 \
	-machine virt \
	-cpu cortex-a76 \
	-machine type=virt \ 
	-nographic -smp 2 -m 4096 \
	-kernel linux-6.9/arch/arm64/boot/Image
```
