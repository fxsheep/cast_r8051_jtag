# cast_r8051_jtag
Attempting to reverse engineer CAST R8051 JTAG debug protocol. Note that all info below is either from bruteforce guessing or from sniffing JTAG with a logic analyzer, which can be inaccurate or completely wrong.

## IR (IRLen = 0x4)
|IR|Meaning|
|:----|:----|
|0x1|IDCODE (0x50000321 == R8051XC2?)|
|0xA|Unknown, 8bit DR, changes when CPU is running|
|0xB|8051 Debug|

## 8051 Debug (when IR = 0xB)
- DR follows regaddr-data-regaddr-data sequence
- all even numbered registers are readonly mirrors of its smaller odd (due to how JTAG works)
- a TAP reset resets debug logic (register values)

### Debug registers
|Regaddr|Length(bits)|Access|Meaning|
|:----|:----|:----|:----|
|0x80|11|RW|Debug Control|
|0x81|11|RO|Mirror of 0x80|
|0x82|8|RO|ACC value|
|0x83|8|RO|Mirror of 0x82|
|0x84|16|RO|PC value|
|0x85|16|RO|Mirror of 0x84|
|0x86|24|RW|8051 Instruction Inject|
|0x87|24|RO|Mirror of 0x86|
|0x88|8|RW|Unknown|
|0x89|8|RO|Mirror of 0x88|
|0x8A|?|?|Unknown|
|0x8B|?|RO|Mirror of 0x8A|
|0x8C|8|RW|Unknown|
|0x8D|8|RO|Mirror of 0x8C|
|0x8E|8|RW|Unknown|
|0x8F|8|RO|Mirror of 0x8E|

#### Debug Control Register (0x80)
|Bit|Meaning|
|:----|:----|
|0|Unknown|
|1|write 1 to run insn in inject register(0x86), auto clears to 0|
|2|write 1 to pause CPU (requires bit4 to be 1)|
|3|bit2 status?|
|4|debug mode enter?|
|5|Unknown|
|6|Unknown|
|7|Unknown|
|8|Unknown|
|9|Unknown|
|10|Unknown|
