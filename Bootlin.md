# Bootlin shortcuts
## General Bootlin commands
For general linux compilation

`export PATH=$HOME/x-tools/arm-training-linux-uclibcgnueabihf/bin/:$PATH`

`export CROSS_COMPILE=arm-linux-`

`export ARCH=arm`


#### Picocom
`picocom -b 115200 /dev/ttyACM0`


#### Trusted Firmware Setup

`export CROSS_COMPILE=arm-linux-'

'export ARCH=aarch32'                           Define architecture

'export ARM_ARCH_MAJOR=7'                       Major version of Arm architecure, Cortex A7 is Armv7

'export PLAT=stm32mp1'                          Define Platform used

'export AARCH32_SP=sp_min'                      Define AArch32 Secure Payload component (What is this)

'export DTB_FILENAME=stm32mp157a-dk1.dtb'       Define the device tree

'export BL33_CFG=../u-boot/u-boot-nodtb.bin'    Device tree passed to U-Boot

'export STM32MP_SDMMC=1'                        Specify the fsbl (first piece of software) is on SD card

'export BL=33../u-boot/u-boot-nodtb.bin'        Specify the location of BL33
 
