# How to setup GDB for remote debugging of Embedded Devices

Due to unexpected errors during GDB setup in a seperate tutorial, it was decided to dive into a in-depth investigation on how to setup GDB for remote debugging and the debugging of the setup itself. This will include configuation needed aswell as issues that arised. This will be based on *Buildroot* configuration settings and serve purely as note keeping for Morne Le Roux

## Buildroot Configuration

```
Toolchain
--> Enable 'Copy gdb server to the Target'
```
## Custom Program

The custom program called `example.c`
  ```
  #include <stdio.h>

  int main () {
      printf("Welcome to debug example");
      printf("Flag 1");
      printf("Flag 2");
      printf("Flag 3");
  }
  ```

Compile `example.c` with `-g` flag to include debugging symbols.

```
arm-linux-gcc -g -o example example.c
```

### Target

Under the target side, run the program under `gdbserver`, who will listen on a TCP port for a connection from `gdb` on the host.
```
gdbserver localhost:2345 /usr/lib/example
```

### Host

Run `arm-linux-gdb example` which loads the debugging information from the `example` binary.

Set the `sysroot`:
```
(gdb) set sysroot /home/morne/bls/buildroot/output/staging
```

Connect to remote target
```
(gdb) target remote <target-ip-address>:2345
```

Start Program
```
(gdb) continue
```

Helpfull Links
[Buildroot.org Text](https://buildroot.org/downloads/manual/using-buildroot-debugger.txt)

