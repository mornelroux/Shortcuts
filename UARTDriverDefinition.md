Linux Driver Development

Process explanation for UART Driver configuration

- UART Registers - 
MDR1  -  Mode Definition Register 1
LCR   -  Line Control Register
DLL   -  Divisor Length Low
DLM   -  Divisor Length High




Step 1) `reg_write(serial, 0x07x UART_OMAP_MDR1`  
