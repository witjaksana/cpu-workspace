# CPU Workspace
This repository contains my experiment on CPU performance

## Compilation
### qemu
```
cd iss-qemu
./runcompile.sh
```

### buildroot
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
