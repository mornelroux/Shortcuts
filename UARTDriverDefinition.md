Linux Driver Development

Process explanation for UART Driver configuration

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

Step 1) `reg_write(serial, 0x07, UART_OMAP_MDR1);`

BIT[0-2] (MODESELECT) of UART_MDR1 register is used set the operation mode. Writing `0x07` to this registers sets the UART controller to defaults (disabled) state. This Disables the UART to access the UART_DLL and UART_DLH registers.

Step 2) `reg_write(serial, 0x00, UART_LCR);`

Resets the LCR data to ensure only the desired bits are set.

Step 3) `reg_write(serial, UART_LCR_DLAB, UART_LCD);`

The Divisor Latch Access bit allows the DLL and DLH to be accessed as dedicated addresses.

Step 4) `reg_write(serial, baud_divisor & 0xff, UART_DLL);`

Writes LSB of the divider calculated from clock frequency and baudrate.

Step 5) `reg_write(serial, (baud_divisor >> 8) & 0xff, UART_DLM);`

Writes MSB of the divider calculated from clock frequency and baudrate.

Step 6) `reg_write(serial, UART_LCR_WLEN8, UART_LCR);`

BIT[0-1] (WLS - Word Length Select) of UART_LCR allows to select the word length of data handled.

Step 7) `reg_write(serial, 0x00, UART_OMAP_MDR1);`

BIT[0-2] (MODESELECT) of UART_MDR1 register is set to UART 16x mode
