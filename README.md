# jlink-core-id
##### Get CPU core identifier for target board by name

## Description

`jlink-core-id` is a small Perl 5 script for translating common board names to their respective CPU core identifier. This is intended for use with JLink SWD debugger tools that require a CPU core when connecting to a target.

## Usage

Simply provide the name of your board (with or without quotes) as argument(s) at the command-line:

```
$ jlink-core-id grand central
selected Grand Central M4 Express (ATSAMD51P20)
ATSAMD51P20
```

Note that the CPU core is printed to stdout, but the other messages are printed to stderr. This way you don't have to parse the output when used with other scripts. If you don't want to see the other messages, you can do something like the following (bash shell):

```sh
if core=$( jlink-core-id ${input[@]} 2>/dev/null ); then
	echo "success! using ${core}"
else
	echo "error: no core found: ${input[@]}"
fi
```

Running the script without any arguments will print all known devices and prompt for user input:

```
$ jlink-core-id
known devices:
  ATmega328P      (Metro Mini 328)
  ATSAMD21G18     (Circuit Playground Express)
  ATSAMD21G18     (Hallowing M0 Express)
  ATSAMD21G18     (ItsyBitsy M0 Express)
  ATSAMD21E18     (Gemma M0)
  ATSAMD21E18     (Trinket M0)
  ATSAMD51G19     (ItsyBitsy M4 Express)
  ATSAMD51J19     (Feather M4 Express)
  ATSAMD51J19     (Matrix Portal M4)
  ATSAMD51J20     (PyPortal)
  ATSAMD51P20     (Grand Central M4 Express)
  STM32F405RG     (Feather STM32F405 Express)
  MIMXRT1062xxx6A (MIMXRT1062-EVK)

target device or CPU core (or <return> to quit):
=> grand central
selected Grand Central M4 Express (ATSAMD51P20)
ATSAMD51P20
```

If input matches multiple devices, all matching devices and an error message are printed to stderr before exiting with a non-zero exit code:

```
$ jlink-core-id feather
multiple devices found:
  ATSAMD51J19 (Feather M4 Express)
  STM32F405RG (Feather STM32F405 Express)
error: too many devices: feather
```

### Adding target devices

Just add a new element to the `@device` slice (defined at the top of the script) with a format consistent with the existing elements.

## Installation

Download the script, ensure execute permissions are set, and place it somewhere in `$PATH`:

```
wget https://github.com/ardnew/jlink-core-id
chmod +x jlink-core-id
sudo cp jlink-core-id /usr/local/bin
```

