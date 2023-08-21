# Bootlin shortcuts
## General Bootlin commands
For general linux compilation

`export PATH=$HOME/x-tools/arm-training-linux-uclibcgnueabihf/bin/:$PATH`

`export CROSS_COMPILE=arm-linux-`

`export ARCH=arm`

## Build/Compile Variables

### U-Boot

    export CROSS_COMPILE=arm-linux-
    make DEVICE_TREE=stm32mp157a-dk1

### TF-A 
    
    export CROSS_COMPILE=arm-linux-
    export ARCH=aarch32                           #Define architecture
    export ARM_ARCH_MAJOR=7                       #Major version of Arm architecure, Cortex A7 is Armv7
    export PLAT=stm32mp1                          #Define Platform used
    export AARCH32_SP=sp_min                      #Define AArch32 Secure Payload component (What is this)
    export DTB_FILENAME=stm32mp157a-dk1.dtb       #Define the device tree
    export BL33_CFG=../u-boot/u-boot.dtb          #Device tree passed to U-Boot
    export STM32MP_SDMMC=1                        #Specify the fsbl (first piece of software) is on SD card
    export BL33=../u-boot/u-boot-nodtb.bin        #Specify the location of BL33

## Usefull Commands

### U-Boot

Set a new environemnt variable

    setenv var="Hello World"
    saveenv

To reset environment to default settings
    
    env default -a
    saveenv

Load the file 'testFile.txt' through TFTP to memory 0xc2000000

    tftp 0xc2000000 testFile.txt        #Load file to memory block
    md 0xc2000000                       #View contents of memory block 0xc2000000
#### Picocom
`picocom -b 115200 /dev/ttyACM0`

### SD Card partitioning

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


#### Trusted Firmware Setup

`export CROSS_COMPILE=arm-linux-`

`export ARCH=aarch32`                           Define architecture

`export ARM_ARCH_MAJOR=7`                       Major version of Arm architecure, Cortex A7 is Armv7

`export PLAT=stm32mp1`                          Define Platform used

`export AARCH32_SP=sp_min`                      Define AArch32 Secure Payload component (What is this)

`export DTB_FILENAME=stm32mp157a-dk1.dtb`       Define the device tree

`export BL33_CFG=../u-boot/u-boot-nodtb.bin`    Device tree passed to U-Boot

`export STM32MP_SDMMC=1`                        Specify the fsbl (first piece of software) is on SD card

`export BL=33../u-boot/u-boot-nodtb.bin`        Specify the location of BL33
 
