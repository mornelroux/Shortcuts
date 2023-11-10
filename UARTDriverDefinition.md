Linux Driver Development

# Process explanation for UART Driver configuration

## Involved Registers

**UART Registers**

UART_MDR1  -  Mode Definition Register 1
UART_LCR   -  Line Control Register
UART_DLL   -  Divisor Length Low
UART_DLM   -  Divisor Length High

**Within UART_LCR register**

UART_LCR_DLAB  - BIT[7] of UART_LCR
UART_LCR_WLEN8 - BIT[0-1] of UART_LCR

## Methodology

-  Step 1) `reg_write(serial, 0x07, UART_OMAP_MDR1);`

BIT[0-2] (MODESELECT) of UART_MDR1 register is used set the operation mode . Writing `0x07` to this registers sets the UART controller to defaults (disabled) state. This Disables the UART.

-  Step 2) `reg_write(serial, 0x00, UART_LCR);`

The LCR regsiters is used to se tthe 'register access mode' whic hdifferenciates between Configuration mode A, condifuration mode B and operation mode. Writing `0x00` resets the LCR. Although `UART_)LCR` is defined in `serial_reg.h` as the value `3`, this is multiplied by 4 in the `writel` function to ultimately come to the `0xC` offset described in the reference sheet.

-  Step 3) `reg_write(serial, UART_LCR_DLAB, UART_LCD);`

The Divisor Latch Access bit set the Register access mode to  Configuration mode A, which allows the DLL and DLH to be accessed as dedicated addresses.

-  Step 4) `reg_write(serial, baud_divisor & 0xff, UART_DLL);`

Writes LSB of the divider calculated from clock frequency and baudrate. The ` & 0xff` allows to extract only the last 8bits of the 16 bit value

-  Step 5) `reg_write(serial, (baud_divisor >> 8) & 0xff, UART_DLM);`

Writes MSB of the divider calculated from clock frequency and baudrate. The ` >> 8` swifts the bits with 8 places to the left, which removes the lower 8 bits and only leaves the MSB. Finally, with the 2 byte exposed, the ` & 0xff` is used to ensure only 8bits are transfered.

-  Step 6) `reg_write(serial, UART_LCR_WLEN8, UART_LCR);`

BIT[0-1] (WLS - Word Length Select) of UART_LCR allows to select the word length of data handled.

-  Step 7) `reg_write(serial, 0x00, UART_OMAP_MDR1);`

BIT[0-2] (MODESELECT) of UART_MDR1 register is set to UART 16x mode
