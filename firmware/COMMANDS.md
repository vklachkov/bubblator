# Firmware Serial Commands

> [!WARNING]
> The firmware and hardware are unfinished. Do not use write, reset, purge, or other commands with a real bubble-memory module. They may change data or damage rare hardware.

> [!NOTE]
> An LLM created this document by reading `firmware.ino`. Treat it with great caution. It has not been verified.

The firmware uses the Arduino Nano USB serial port at **115200 baud**. Send one command per line. Both `LF` and `CR` line endings are accepted. Command letters are case-insensitive.

The command line is limited to 63 characters. A longer line is discarded.

## Basic Commands

| Command | Description |
| --- | --- |
| `i0` | Release reset and load normal parameters: two formatter/sense amplifier (FSA) channels and one page per transfer. |
| `i1` | Release reset and load diagnostic parameters: one FSA channel and one page per transfer. |
| `i2` | Release reset and load Write Bootloop parameters. This enables writing to the bootloop. |
| `r` | Assert controller reset when it is not busy. Run `i0`, `i1`, or `i2` afterwards to release reset. |
| `rf` | Force reset even when the controller reports busy. This may cause data loss. Run an `i` command afterwards to release reset. |
| `s` | Read and print the controller status register. |
| `fr` | Read the controller FIFO and print its contents. |
| `fw <hex bytes>` | Write hexadecimal bytes to the controller FIFO. At most 43 bytes are written. |

For `fw`, values may be separated by spaces, commas, or colons. The `0x` prefix is optional. Each value may have one or two hexadecimal digits; one digit is stored as `0x0N`. For example:

```text
fw FF 00 A5 7E
fw 0xFF,0x00,0xA5,0x7E
```

## Controller Commands

Use `c` followed by a command code. For example, `c1` sends the controller Initialize command. These commands directly access the Intel 7220 controller.

| Command | Intel 7220 operation | Notes |
| --- | --- | --- |
| `c0` | Write Bootloop Register Masked | Direct controller command. |
| `c1` | Initialize | Direct controller command. |
| `c2` | Read Bubble Data | Direct controller command. |
| `c3` | Write Bubble Data | Direct controller command. |
| `c4` | Read Seek | Direct controller command. |
| `c5` | Read Bootloop Register | Direct controller command. |
| `c6` | Write Bootloop Register | Not implemented: the firmware does not send this command. |
| `c7` | Write Bootloop | Not implemented: the firmware does not send this command. |
| `c8` | Read FSA Status | Direct controller command. |
| `c9` | Abort | Abort the current operation. After it completes, run Initialize or MBM Purge; MBM Purge may be destructive. |
| `ca` | Write Seek | Direct controller command. |
| `cb` | Read Bootloop | Direct controller command. |
| `cc` | Read Corrected Data | Direct controller command. |
| `cd` | Reset FIFO | Direct controller command. |
| `ce` | MBM Purge | Direct controller command. This is potentially destructive. |
| `cf` | Software Reset | Direct controller command. |
| `cx` | Fill FIFO with 34 ones | Not implemented. |
| `cz` | Fill FIFO with 40 ones | Not implemented. |

After each `c` command, the firmware polls for completion or failure for up to about two seconds. It prints `Operation Timed Out!` if neither status appears. This wait also runs for the unimplemented commands. Some direct commands require correctly configured registers and FIFO contents; the firmware does not validate that setup.

## Page Read and Write

Page commands are sent as controller commands with `r` or `w` as the code:

```text
cr <first page> [last page]
cw <first page> [last page]
```

Examples:

```text
cr 0
cr 10 15
cw 42
```

Pages are decimal numbers from `0` to `2047`. A comma or colon may also separate the two page numbers. An omitted or invalid first page defaults to `0`. An omitted or invalid last page defaults to the selected first page.

- `cr` reads one page or an inclusive page range. Each page contains 68 bytes: 64 data bytes and 4 error-correction bytes. The result is printed in hexadecimal and printable ASCII.
- `cw` writes one page or an inclusive page range. **The current firmware always writes `FF` to all 68 bytes of every selected page.** It makes at most five write attempts per page. The current code may print `Abort...` after the fifth attempt even when that final attempt succeeds.

Both commands can wait forever for `FIFOReady` if the controller is unavailable or incorrectly configured. There is no timeout for this wait.

Do not run `cw` on a real module.

## Status Output

The `s` command can print these flags:

- `Busy` - controller operation is in progress.
- `OpComplete` - the operation completed.
- `OpFail` - the operation failed.
- `TimingError`, `CorrectableError`, `UncorrectableError`, and `ParityError` - controller error conditions.
- `FIFOReady` - data is available in the FIFO.

## Unavailable Commands

The parser also accepts `a` and `u`, but their handlers are disabled in the current firmware. They require one parameter character (for example, `ax` or `ux`) and then have no effect. `a` or `u` alone reports a missing parameter.
