# LT6502b
Second, slimmer, revision of my [LT6502](https://github.com/TechPaula/LT6502/tree/main) 6502 laptop.
This is very much a work in progress.

## Specs
* 65C02 running at 14MHz (hopefully)
* 46KByte of user RAM 
* EhBASIC 22p2
* eWOZMON
* Compact Flash for storage
* Built in battery (7400mAh currently)
* USB-PD Rechargable
* 10.1" Screen
* nanoSwinSID for sound (as well as beeps)

Features improved on since previous version;
* Better placement of keys
* Use an FFC for the display
* Simpler case
* Battery level indicator (CAPS Lock will flash when getting low)
* USB charge and data on single USB-C connector
* Single PCB for keyboard and logic/processor

## Images
2026-03-07 - Render of case design in progress
![Render of case, not yet finished](https://github.com/TechPaula/LT6502b/blob/main/Images/LT6502b_Assembly_2026-Mar-07_08-01_noHinge.png?raw=true?raw=true)
2026-03-21 - Picture of PCB mid assembly
![PCB being assembled](https://github.com/TechPaula/LT6502b/blob/main/Images/PCB_MidAssembly.jpg?raw=true?raw=true)


## Updates
* 2026-03-21 - PCB assembly underway (power tests passed)
* 2026-03-07 - Case coming together, currently 31mm in height, vs the 70mm of the previous version)
* 2026-03-01 - PCB sent for manufacture


## In progress
* Board assembly
* Glue Logic


## To do
* Get larger display working (probably on PC6502 first)
* CPLD Glue logic working
* Firmware (removing the display parts for now as the new display uses a different driver chip)
* Keyboard/Modem code (needs a change due to using the chip directly and I need to add in serial port driver for Modem)
* Testing, lots of testing.
* Finish the case design and print it.

## Memory Map
The memory map is copied from the previous LT6502, with removal of expansion port and addition of Modem and SID

#### High Level
| Start | End | Size (Dec) | Size (Hex) | What is it | Notes |
|-------|-----|----|----|----|---------------|
| 0x0000|0xBEAF| 48816 | 0xBEB0 | RAM | This includes Zeropage and other bits BASIC may need (more below) |
| 0xBF00|0xBFFF| 512 | 0x200 | peripherals | This is where the peripherals are mapped (see below) |
| 0xC000|0xFFFF| 12288 | 0x3000 | ROM | holding EhBASIC, eWoz monitor, bootstrap and vectors |

##### ROM breakdown
| Start | End | Size (Dec) | Size (Hex) | What is it | Notes |
|-------|-----|----|----|----|---------------|
| 0xC000|0xFAFF| 15104 | 0x3B0 | EhBASIC | EhBASIC 2.22p5 |
| 0xF000|0xF2FF| 768 | 0x300 | eWozMon | [Enhanced Wozmon](https://gist.github.com/BigEd/2760560) |
| 0xF300|0xFFF9| 3322| 0xCFA | Bootstrap | startup messages and also input/output/load/save functions |
| 0xFFFA|0xFFFF| 6 | 0x0A | 6502 Vectors | |

##### RAM breakdown
| Start | End | Size (Dec) | Size (Hex) | What is it | Notes |
|-------|-----|----|----|----|---------------|
| 0x0000|0x02FF| 768 | 0x300 | RAM | This includes Zeropage and other bits BASIC may need |
| 0x0300|0x07FF| 1280 | 0x500 | RAM | This is going to be for the compact flash reading/writing |
| 0x0800|0xBDFF| 46592 | 0xB6B0 | ROM | BASIC RAM available |

##### peripherals
| Address | subAddr range | RW | What is it | Notes |
|-------|-----|----|----|---------------|
|0xBF00|00-1F|RW| nanoSwinSID | |
|0xBF20|0-1|RW| Modem | |
|0xBF30|30-9F|RW| Unused Currently | |
|0xBFAO|0-0| W| Beeper | just write 0xFF and 0x00 to turn on/off the speaker |
|0xBFBO|0-7|RW| Compact Flash |  |
|0xBFCO|0-F|RW| 65C22 |  on board VIA |
|0xBFDO|0-F|0-1| Display |   |
|0xBFEO|0-F|RW| Atmega644p | internal keyboard  |
|0xBFF0|0-1|RW| Console | FTDI USB console port   |
