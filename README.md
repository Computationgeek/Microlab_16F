# MicroLab 16F - PIC16F877A Development Board  
A hand-soldered PIC16F877A development board built entirely on perfboard. This project was born from a desire to gain a deep understanding of embedded systems hardware by building everything from scratch - no pre-made modules, no shortcuts.

<img width="749" height="562" alt="hkio" src="https://github.com/user-attachments/assets/2c940034-9846-4d7e-9929-de731b72f40c" />

## 🎯 Vision & Motivation

In 2026, most developers rely on modern MCUs like ESP32 and STM32. But there's something special about PIC microcontrollers - they force you to understand registers, bit manipulation, and hardware at a fundamental level: no HAL abstraction, no hand-holding.

I built MicroLab 16F because:
- **Learning by doing** - Soldering each connection taught me more than any tutorial
- **Legacy matters** - PIC16F877A is still widely used in education and industry
- **Maker ethos** - If you can't build it yourself, you don't truly understand it
- **Fun over perfection** - This isn't a product, it's a learning journey

## ✨ Features

### Hardware
- **PIC16F877A** microcontroller (40-pin DIP)
- **20MHz HS crystal** oscillator with 22pF load capacitors
- **Mini breadboard** integrated prototyping area
- **10kΩ potentiometer** connected to RA0 (ADC input)
- **Active buzzer** driven by BC547 NPN transistor on RD1
- **User-addressable LED** on RB0
- **Two tactile buttons** - User button (configurable) and Reset button (MCLR)
- **USB-C power input** with 5V regulation
- **PICkit3 programming header** (ICSP interface)
- **Labeled pin headers** for all GPIO ports (PORTA, PORTB, PORTC, PORTD, PORTE)

### Power System
- USB-C input with 5V output
- Dedicated power LED indicator
- Decoupling capacitors for stable operation
- External power compatible (PICkit3 or USB-C)

## 📋 Detailed Specifications

<img width="565" height="475" alt="PIC16F877A_Pinout_600x600" src="https://github.com/user-attachments/assets/ad9777ec-0b56-462c-929f-302a223e2305" />

| Parameter | Value |
|-----------|-------|
| **Microcontroller** | PIC16F877A (Microchip) |
| **Architecture** | 8-bit RISC |
| **Program Memory** | 14.3 KB (8192 words) Flash |
| **RAM** | 368 bytes |
| **EEPROM** | 256 bytes |
| **Clock Frequency** | 20 MHz (HS Crystal) |
| **I/O Pins** | 33 |
| **ADC Channels** | 8 (10-bit resolution) |
| **Timers** | 3 (Timer0/1/2) |
| **PWM Channels** | 2 (CCP modules) |
| **Serial Interfaces** | USART, I²C, SPI |
| **Operating Voltage** | 5V DC |
| **Power Source** | USB-C (5V) |
| **Programming Interface** | ICSP (In-Circuit Serial Programming) |
| **Programmer** | PICkit3

## 🔧 Components Used

### Main Components
| Component | Specification | Quantity | Purpose |
|-----------|---------------|----------|---------|
| PIC16F877A-I/P | 40-pin DIP | 1 | Main microcontroller |
| Crystal Oscillator | 20 MHz HC-49S | 1 | Clock source |
| Ceramic Capacitor | 22pF | 2 | Crystal load capacitors |
| Ceramic Capacitor | 100nF | 5+ | Decoupling/bypass |
| Electrolytic Capacitor | 10µF 16V | 2 | Power filtering |
| BC547 NPN Transistor | TO-92 | 1 | Buzzer driver |
| Active Buzzer | 5V | 1 | Audio output |
| LED (5mm) | Red/Green | 3 | Power, User, Status indicators |
| Resistor | 10kΩ | 5+ | Pull-ups, current limiting |
| Resistor | 330Ω | 3 | LED current limiting |
| Resistor | 1kΩ | 1 | Base resistor for BC547 |
| Potentiometer | 10kΩ linear | 1 | Analog input (RA0) |
| Tactile Switch | 6×6mm | 2 | User button, Reset button |
| Mini Breadboard | 170 points | 1 | Prototyping area |
| USB-C Connector | Breakout module | 1 | Power input |
| Pin Headers | 2.54mm pitch | Multiple | GPIO breakout |
| Perfboard | Single-sided | 1 | PCB substrate |
| IC Socket | 40-pin DIP | 1 | PIC mounting (optional) |

## 🛠️ Software & Tools

### Development Environment
- **IDE:** MPLAB X IDE v6.20 (Last version with PICkit3 support)
- **Compiler:** MPLAB XC8 v2.46 (Free edition)
- **Programmer:** PICkit3 In-Circuit Debugger/Programmer
- **OS:** Tested on Linux Mint 21.3 & Windows 10/11

### Programming Setup
```bash
# Linux users: Add udev rules for PICkit3
sudo nano /etc/udev/rules.d/26-microchip.rules

# Add this line:
SUBSYSTEM=="usb", ATTR{idVendor}=="04d8", ATTR{idProduct}=="900a", MODE="0666", GROUP="plugdev"

# Reload rules
sudo udevadm control --reload-rules
sudo udevadm trigger
```

### Configuration Bits (Standard Setup)
```c
#pragma config FOSC = HS        // High-Speed crystal oscillator
#pragma config WDTE = OFF       // Watchdog Timer disabled
#pragma config PWRTE = ON       // Power-up Timer enabled
#pragma config BOREN = ON       // Brown-out Reset enabled
#pragma config LVP = OFF        // Low-Voltage Programming disabled (CRITICAL for ICSP)
#pragma config CPD = OFF        // Data EEPROM Code Protection off
#pragma config WRT = OFF        // Flash Program Memory Write Enable off
#pragma config CP = OFF         // Flash Program Memory Code Protection off
```

## 🚀 Getting Started

### 1. Power Up
Connect the USB-C cable to a 5V power source (laptop, power bank, or wall adapter).
The power LED should illuminate.

### 2. Connect Programmer
**PICkit3 Pinout (6-pin header):**
1 - MCLR  → PIC Pin 1 (MCLR/VPP)
2 - VDD   → PIC Pin 11, 32 (VDD) - For sensing only
3 - GND   → PIC Pin 12, 31 (VSS)
4 - PGD   → PIC Pin 40 (RB7/PGD)
5 - PGC   → PIC Pin 39 (RB6/PGC)
6 - NC    → Not connected

## 🐛 Troubleshooting

### PICkit3 "Target VDD too low" Error
**Problem:** VDD measured as 4.75V instead of 5.0V during programming.

**Cause:** Board peripherals (buzzer, LEDs) draw more current than PICkit3 can supply (~30-50mA limit).

**Solution:** 
1. In MPLAB X: Project Properties → PICkit3 → Power → UNCHECK "Power target from tool"
2. Power board via USB-C only
3. PICkit3 is used for programming signals, not power

### Programming Fails at Address 0x0000
**Cause:** Incorrect config bits or MCLR circuit issue.

**Solution:**
- Ensure `LVP = OFF` in config bits
- Verify 10kΩ pull-up resistor on MCLR pin
- Try a slower programming speed in MPLAB X settings

See [docs/troubleshooting.md](docs/troubleshooting.md) for more.

## 📚 Learning Resources

### PIC16F877A Deep Dive
- [Microchip Official Datasheet](https://www.microchip.com/wwwproducts/en/PIC16F877A)
- [Gooligum Electronics PIC Tutorials](https://www.gooligum.com.au/PIC-tutorials) - Best assembly/register-level guide
- Microchip Application Notes: AN526 (Math routines), AN617 (Fixed-point math)

### Recommended Reading
- "PIC Microcontroller Project Book" by John Iovine
- "Programming PIC Microcontrollers with XC8" by Armstrong Subero
- Embedded.fm podcast episodes on PIC architecture

## 🎓 Why PIC16F877A in 2026?

**Yes, it's 25+ years old. Here's why it still matters:**

1. **Educational Gold Standard** - Used in universities worldwide to teach embedded fundamentals
2. **Register-Level Programming** - No bloated HALs hiding what's actually happening
3. **Industry Workhorse** - Still found in industrial equipment, automotive, and legacy systems
4. **Simplicity** - 35 instructions vs 100+ in ARM Cortex
5. **Availability** - Cheap, plentiful, easy to source
6. **Hands-On Learning** - DIP package = breadboard/perfboard friendly

Modern isn't always better. Sometimes you need to go back to basics.

## 📜 License

MIT License - Build, modify, share freely. Just give credit.

## 🔗 Links

- **Build Story:** [Hackster.io Project](https://www.hackster.io/aamirayaaz5/building-a-pic16f877a-development-board-from-scratch-in-2026-7d29bc)
- **LinkedIn Post:** [LinkedIn Post about the board](https://www.linkedin.com/posts/aamir-ayaaz-b5549a220_embeddedsystems-electronicsengineering-hardwaredesign-activity-7425852984116404224-cy4W?utm_source=share&utm_medium=member_desktop&rcm=ACoAADeZ0vEBUnqS9fmuq7yIjKdyj92gUf-e6x0)

## 🙏 Acknowledgments

Built with:
- Lots of solder fumes
- Multiple debugging sessions
- A stubborn refusal to give up when VDD dropped to 4.75V
- Coffee

**Special thanks to the PIC community and all the forum posts from 2005-2015 that saved this project.**

---

*"If you can't build it yourself, you don't truly understand it."*
