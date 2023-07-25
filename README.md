# Note-of-Gowin-FPGA-Sipeed-Tang-20K

## IO
### IO logic mode
_(DS102E pg.16)_
- The I/O (or I/O differential pair) can be configured as output, input, and inout (tristate output). IO logic has 17 modes
+ Basic mode:
The TC, DO, DI can connect directly to the internal cores.
<p align="center">
  <img src="https://github.com/atfox272/Note-of-Gowin-FPGA-Sipeed-Tang-20K-/assets/99324602/a0078c5e-60ea-447f-875b-db82da5e7634" width=75% height=75%/>
</p>
<p align="center">
  <img src="https://github.com/atfox272/Note-of-Gowin-FPGA-Sipeed-Tang-20K-/assets/99324602/2654c600-258c-4bb6-b809-9296b51ee202" width=50% height=50%/>
</p>

+ SDR mode:
SDR use 3 FFs for 3 types of port (in, out, tri). 
* This mode is one of the solutions to Debounce button
</p>
<p align="center">
  <img src="https://github.com/atfox272/Note-of-Gowin-FPGA-Sipeed-Tang-20K-/assets/99324602/6388ea78-afdd-4ca5-936b-48db208b4251" width=50% height=50%/>
</p>
CLK enable O_CE and I_CE can be configured as active high or active low;
O_CLK and I_CLK can be either rising edge trigger or falling edge trigger;
Local set/reset signal O_SR and I_SR can be either synchronized reset, synchronized set, asynchronous reset, asynchronous set, or no-function;
I/O in SDR mode can be configured as basic register or latch.

+ Generic DDR Mode:
...
+ IDES4:
...

### Add delay option in IO port
_(DS102E pg.13)_
- Output block example:
<p align="center">
  <img src="https://github.com/atfox272/Note-of-Gowin-FPGA-Sipeed-Tang-20K-/assets/99324602/bc6097e9-2fe5-424c-af27-2af7189baaa4" width=50% height=50%/>
</p>
- Input block example:
<p align="center">
  <img src="https://github.com/atfox272/Note-of-Gowin-FPGA-Sipeed-Tang-20K-/assets/99324602/2654c600-258c-4bb6-b809-9296b51ee202" width=50% height=50%/>
</p>
- Each I/O of the GW2A series of FPGA products has an IODELAY cell. A total of 128(0~127) step delay is provided, with one-step delay time of about 18ps. (overview)
<p align="center">
  <img src="https://github.com/atfox272/Note-of-Gowin-FPGA-Sipeed-Tang-20K-/assets/99324602/8ecd0978-0ab9-46b1-8d40-895e9c26204f" width=60% height=60%/>
</p>
- The delay cell can be controlled in two ways:
+ Static control. (default value)
+ Dynamic control: Usually used to sample delay window together with IEM. The IODELAY cannot be used for both input and output at the same time (can adjust)

## Clock
### Global Clock (GCLK)
- GCLK is distributed in the GW2A devices as four quadrants. Each quadrant provides eight GCLKs
<p align="center">
  <img src="https://github.com/atfox272/Note-of-Gowin-FPGA-Sipeed-Tang-20K-/assets/99324602/d823d19c-35ea-476c-b2f2-293a7b73d93e" alt="Sublime's custom image"/>
</p>

### High-speed Clock (HCLK)
- HCLK is the high-speed clock in the GW2A series FPGA products, which can support high-speed data transfer and is mainly suitable for source synchronous data transfer protocols
<p align="center">
  <img src="https://github.com/atfox272/Note-of-Gowin-FPGA-Sipeed-Tang-20K-/assets/99324602/f54792d4-c794-4a8a-b2ac-ce5e6cc0aabc" width=60% height=60%/>
</p>
- There is an 8: 1 HCLKMUX module in the middle of the high-speed clock HCLK. HCLKMUX can send HCLK clock signal from any Bank to any other bank, which makes the use of HCLK more flexible.
- CLK can provide user with the function modules as follows:
+ DHCEN: Dynamic high-speed clock enable module, functions similar to DQCE. Dynamically turn on / off high-speed clock signal.
+ CLKDIV/ CLKDIV2: high-speed clock divider module, each bank has a CLKDIV. Generates a clock divided by the input clock phase, which is used in the IO logic mode.
+ DCS: Dynamic High Speed Clock Selector.
+ DLLDLY: The dynamic delay adjustment module, the clock signal for the dedicated clock pin input.

### DC Electrical Characteristic
<p align="center">
  <img src="https://github.com/atfox272/Note-of-Gowin-FPGA-Sipeed-Tang-20K-/assets/99324602/934d06e8-44e7-4b85-9107-5b9a40e626ed" width=150% height=150%/>
</p>

## Constraints
- Chip Array displays I/O, CFU, CLU, DSP, PLL, BSRAM, and DQS
- Supports real-time display of all constraints locations, and also supports the functions of zoom in/out and dragging
### IO Constraints 
* Name table in "Packet view" window
- Unplace: Cancel placement
- Reset Properties: Reset Port properties
- Highlight: Highlight constraints location
- IO Type: Set the level standard
- Drive: Set drive voltage
- Pull Mode: Set pull-up mode
- PCI Clamp: Set the switch of PCI protocol
- Hysteresis: Set hysteresis
- Open Drain: Set the switch of open-drain circuit
- Vref: Set reference voltage
- Single Resistor: Set the switch of single-ended resistor
- Diff Resistor: Set the switch of differential resistor
- Bank VccIo: Set BANK voltage

* Mapping name (location) = <Character of row><Number of column>
- Example: Location of I/O pin in green circle is G14
<p align="center">
  <img src="https://github.com/atfox272/Note-of-Gowin-FPGA-Sipeed-Tang-20K-/assets/99324602/be320a41-76e8-451a-9262-c7697980c66d" alt="Sublime's custom image"/>
</p>

### Primitive Constraints
* Adjust internal primitives
### Group Constraints
### Resource Reservation (Reservation Constraints)
### Clock net Constraints
_(SUG935E pg.69)_
- Clock Net Constraints is used to constrain a specific net to the global clock wire or non-clock wire in the design; it can implement the global clock constraints for the net of specific signal_type (CLK/CE/SR/LOGIC).
+ BUFG[0-7] means the net is routed to PCLK.
+ BUFS means the net is routed to SCLK.
+ LOCAL_CLOCK means this net is not routed to the global clock wire.
- The CLK signal is the signal connected to the clock pin. The CE signal is the signal connected to the clock enable pin. The SR signal is the signal connected to the SET/RESET/CLEAR/PRESET pins, and the LOGIC is the signal connected to logic input pins.
### HCLK Primitive Constraints
### Vref Constraints
- Vref Constrains is used for the external reference voltage of the BANK where the constraint is located
<p align="center">
  <img src="https://github.com/atfox272/Note-of-Gowin-FPGA-Sipeed-Tang-20K-/assets/99324602/e6146cef-c980-403e-888a-71fef1be2a04" alt="Sublime's custom image"/>
</p>

### Timing Adjust 

## Keywords
- GCLK: global clock
- HCLK: high-speed clock
