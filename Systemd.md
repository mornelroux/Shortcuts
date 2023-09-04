# Systemd Issues with Bootlin installation

## Issue - Systemd hangs after reboot

SystemD hangs after reboot, leaving user without any terminal access. It appears to be randomly, since sometimes it executes normally and log a login prompt. 

## Cause

Within the first few lines of systemd execution, the error arises `/usr/lib/systemd/system-generators/systemd-getty-generator terminated`. The `getty` process is responsible for providing terminal access through a `tty` terminal. However, it seems like sometimes this service does not get start for no known reason. 

This way there is no access to the terminal.

## Solution - Restart `getty@ttySTM0` through SSH

Even though this tty terminal can be replaced by ssh access, we will want to know how t access the terminal though TTY. First you need to make sure that `Dropbear` is configured in `menuconfig`. Then the `autorised_keys` with the public ssh key should be placed in the root overlay directory under `root/.ssh/authorized_keys`.

Restart your device and wait until systemd hangs. On the host PC, ssh in using `ssh root@192.168.0.100`. Once in, type the following command

`systemctl restart getty@ttySTM0`

This should restart the terminal and give you a Buildroot login prompt.




