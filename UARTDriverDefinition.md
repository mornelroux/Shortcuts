Linux Driver Development

Process explanation for UART Driver configuration

**UART Registers**
UART_MDR1  -  Mode Definition Register 1
UART_LCR   -  Line Control Register
UART_DLL   -  Divisor Length Low
UART_DLM   -  Divisor Length High

**Within UART_LCR register**
UART_LCR_DLAB  - BIT[7] of UART_LCR
UART_LCR_WLEN8 - BIT[0-1] of UART_LCR

Step 1) `reg_write(serial, 0x07, UART_OMAP_MDR1);`  

Step 2) `reg_write(serial, 0x00, UART_LCR);`

Step 3) `reg_write(serial, UART_LCR_DLAB, UART_LCD);`

Step 4) `reg_write(serial, baud_divisor & 0xff, UART_DLL);`

Step 5) `reg_write(serial, (baud_divisor >> 8) & 0xff, UART_DLM);`

Step 6) `reg_write(serial, UART_LCR_WLEN8, UART_LCR);`

Step 7) `reg_write(serial, 0x00, UART_OMAP_MDR1);`
