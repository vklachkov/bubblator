# Bubblator

> [!WARNING]
> This project is unfinished. Nothing is known to work yet. Do not use this board with real bubble-memory modules. It may damage rare hardware. Please wait for tested hardware, firmware, and clear operating instructions.

Bubblator is an experimental board for reading, writing, and attempting to restore Intel 7110 bubble-memory modules.

It follows the Intel Bubble Memory System design and uses the following main parts:

- Intel 7110 / 7110A(Z) One Megabit Bubble Memory Unit
- Intel 7220-1 Bubble Memory Controller
- Intel 7230 Current Pulse Generator
- Intel 7242 Formatter/Sense Amplifier
- Intel 7250 Coil Predriver
- 2x Intel 7254 driver transistors
- Arduino Nano

Firmware serial commands are described in [firmware/COMMANDS.md](firmware/COMMANDS.md).

## Documentation Used

The following Intel documents were used during development:

- [Megabits to Megabytes: Bubble Memory System Design and Board Layout (AP-187, 1984)](https://archive.org/details/bitsavers_intelbubbl87MegabitsToMegabytesBubbleMemorySystemD_3867950) - system design, board layout, and implementation guidance.
- [Intel Bubble Memory Design Handbook](https://archive.org/details/IntelBubbleMemoryDesignHandbook) - bubble-memory principles and design reference.
- [Intel BPK72 Bubble Storage Prototype Kit](https://web.archive.org/web/20190419090939/https://www.wolfgangrobel.de/museum/files_bubble5/intel%20BPK72%20Prototype%20kit%20users%20manual.pdf) - reference prototype system and kit documentation.
- [Intel 7220 Controller for 1Mbit BPK 70A Bubble Memory Subsystem](https://web.archive.org/web/20250912182130/https://wolfgangrobel.de/museum/files_bubble5/intel%207220%20bubble%20memory%20controller.pdf) - datasheet for the Intel 7220 bubble-memory controller.

Further project background:
- [ArduBubble / ReBubbling on the Unix Haters Wiki](https://wiki.unix-haters.org/doku.php?id=grid:rebubbling)

## Authors

- **sabur** - original firmware code.
- **grm** - initial hardware design.
- **BOOtak** - created the second PCB revision and assembled the prototype.
- **vklachkov** - continued the work of others.
