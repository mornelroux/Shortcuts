# Bootlin shortcuts
## Linux Kernel

### Build Configuration

    export PATH=$HOME/x-tools/arm-training-linux-uclibcgnueabihf/bin/:$PATH
    export CROSS_COMPILE=arm-linux-
    export ARCH=arm

If kernel does not load, check menu configuration. Sometime, the STM32MP family is deselected.

### Embedded System Startup Setup

    1) Create file 'inittab' under the `etc/` directory
    2) Within 'inittab' provide the following

            #etc/inittab

            ::sysinit:/etc/init.d/rcS
            ttySTM0::askfirst:/bin/sh

    4) Create directory `etc/init.d/`
    5) Within `etc/init.d` directory, create file `rcS`
    6) Add execution permisions with `sudo chmod +x rcS`
    7) Within `rsC`, add startup script commands

            #!/bin/sh
            #Mount virtual file systems
            mount -t proc nodev /proc
            mount -t sysfs nodev /sys

Common issue: If `ttySTM0::askfirst:/bin/sh` is not present, the kernel will freeze during load at `Run /sbin/init as init process`

### Encourtered Issues

#### Issue 1

When loading the new zImage on the target, process freezes completely (without automactic restart). Final message on terminal:

`Starting kernel ...`

Solution: For some reason during the last `make menuconfig` the System type was deselected.

## Device Tree Configuration

### Adding a customized device tree

1) Create new file under `~/embedded-linux-labs/kernel/linux/arch/arm/boot/dts` with `.dts` extension

   Example for `stm32mp157a-dk1-custom.dts`

       /dts-v1/;
       #include "stm32mp157a-dk1.dts"

       &i2c5 {
           status = "okay";
       };
2) Modify `Makefile` to include the new device tree under a suitable variable ex. `dtb-$(CONFIG_ARCH_STM32)`
3) Run `make dtbs` in the `~/embedded-linux-labs/kernel/linux/` directory
4) Add the new DTB to the TFTP server directory
5) Modify `bootcmd` in U-boot to load the new DTB
   
## U-Boot

### Build Configuration

    export CROSS_COMPILE=arm-linux-
    make DEVICE_TREE=stm32mp157a-dk1

### CLI commands

    setenv var="Hello World"            #Set an environment variable 'var' to "Hello World"
    saveenv                             #Save current environment variables
    env default -a                      #Set environment variables to system default
    tftp 0xc2000000 testFile.txt        #Load file from TFTP server to memory block
    md 0xc2000000                       #View contents of memory block 0xc2000000

## TF-A 

### Build Configuration
    
    export CROSS_COMPILE=arm-linux-
    export ARCH=aarch32                           #Define architecture
    export ARM_ARCH_MAJOR=7                       #Major version of Arm architecure, Cortex A7 is Armv7
    export PLAT=stm32mp1                          #Define Platform used
    export AARCH32_SP=sp_min                      #Define AArch32 Secure Payload component (What is this)
    export DTB_FILENAME=stm32mp157a-dk1.dtb       #Define the device tree
    export BL33_CFG=../u-boot/u-boot.dtb          #Device tree passed to U-Boot
    export STM32MP_SDMMC=1                        #Specify the fsbl (first piece of software) is on SD card
    export BL33=../u-boot/u-boot-nodtb.bin        #Specify the location of BL33

## Busybox

### Build Configuration

    export PATH=$HOME/x-tools/arm-training-linux-uclibcgnueabihf/bin/:$PATH
    export CROSS_COMPILE=arm-linux-
    export ARCH=arm

### CLI Commands

    ps                                   #Information about kernel processes
    halt                                 #Umount filesystems safely before shutting down
    mount -t proc nodev /proc            #Mounting the proc filesystem
    mount -t sysfs nodev /sys            #Mounting the sys filesystem

Example inittab sources available at https://elixir.bootlin.com/busybox/latest/source/examples/inittab
    
## Picocom

Startup UART communication through host terminal's USB

    picocom -b 115200 /dev/ttyACM0

## System Processes 

### Parted - SD Card partitioning

Clear the first 128MB

    sudo dd if=/dev/zero of=/dev/sda bs=1M count=128

Use `parted` program to create partitions on uSD card
    
    sudo parted /dev/sda

For `embedded-linux-labs` course, the following commands is executed in `parted`:
    
    (parted) mklabel gpt
    (parted) mkpart fsbl1 0% 4095s
    (parted) mkpart fsbl2 4096s 6143s
    (parted) mkpart fip 6144s 10239s
    (parted) mkpart bootfs 10240s 131071s
    (parted) print
    (parted) quit

## I2C Nunchuck Configuration

Red        -> VCC -> CN16 Pin 4
White      -> SCL -> CN13 Pin 10 (D15)
Yellow     -> SDA -> CN13 Pin 9  (D14)
Green      -> GND -> CN16 Pin 6 

 
