# g474_cmake_demo

Simple CMake-based demo project for *Nucleo-G474RE* board.

Created using ["CLion for embedded development"](https://blog.jetbrains.com/clion/2016/06/clion-for-embedded-development/) by analogy.

LD2(PA5) led blinks every 500 ms and PA8 GPIO pin toggled upon occurring falling edge on blue B1 User Push-Button.

## Requirements

* gcc-arm-none-eabi 9+, `path/to/gcc-arm-none-eabi/bin/` should be added in `$PATH`.
* cmake 3.5+
* ST-LINK V3 drivers, ST-LINK Utility v4.5.0 (optionally)
* The last OpenOCD (optionally)

## Building

```bash
mkdir build && cd build
cmake -Wdev -G "Unix Makefiles" -DCMAKE_TOOLCHAIN_FILE=../STM32G474RETx.cmake ..
make
```
## Evaluation

Upload `g474_cmake_demo.hex` to the board and try tp press on B1 Push-button.

## Debug on Windows

* Download binaries of the last OpenOCD (for example, from [here](https://sysprogs.com/getfile/1180/openocd-20200729.7z))

* Connect *Nucleo-G474RE* board to the computer

* Try to connect with OpenOCD like this:

  ```bash
  $ /path/to/bin/openocd.exe --search /path/to/share/openocd/scripts -f /path/to/interface/stlink.cfg -f ./my_st_nucleo_g4.cfg
  Open On-Chip Debugger 0.10.0 (2020-07-29) [https://github.com/sysprogs/openocd]
  Licensed under GNU GPL v2
  libusb1 09e75e98b4d9ea7909e8837b7a3f00dda4589dc3
  For bug reports, read
          http://openocd.org/doc/doxygen/bugs.html
  Warn : Interface already configured, ignoring
  Error: already specified hl_layout stlink
  Info : The selected transport took over low-level target control. The results might differ compared to plain JTAG/SWD
  srst_only separate srst_nogate srst_open_drain connect_deassert_srst

  Info : Listening on port 6666 for tcl connections
  Info : Listening on port 4444 for telnet connections
  Info : clock speed 2000 kHz
  Error: libusb_open() failed with LIBUSB_ERROR_NOT_SUPPORTED
  Info : STLINK V3J4M2 (API v3) VID:PID 0483:374E
  Info : Target voltage: 3.318876
  Info : stm32g4x.cpu: hardware has 6 breakpoints, 4 watchpoints
  Info : starting gdb server for stm32g4x.cpu on 3333
  Info : Listening on port 3333 for gdb connections
  ```

* Now you can run `gdb`. Listing from my installation:

  ```bash
  $ ./arm-none-eabi-gdb /k/Y_exps_drafts_debugs_logs_exmpls/goryachev_soft/g474_cmake_demo/build/g474_cmake_demo.elf
  GNU gdb (GNU Tools for Arm Embedded Processors 9-2019-q4-major) 8.3.0.20190709-git
  Copyright (C) 2019 Free Software Foundation, Inc.
  License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
  This is free software: you are free to change and redistribute it.
  There is NO WARRANTY, to the extent permitted by law.
  Type "show copying" and "show warranty" for details.
  This GDB was configured as "--host=i686-w64-mingw32 --target=arm-none-eabi".
  Type "show configuration" for configuration details.
  For bug reporting instructions, please see:
  <http://www.gnu.org/software/gdb/bugs/>.
  Find the GDB manual and other documentation resources online at:
      <http://www.gnu.org/software/gdb/documentation/>.

  For help, type "help".
  Type "apropos word" to search for commands related to "word"...
  Reading symbols from K:/Y_exps_drafts_debugs_logs_exmpls/goryachev_soft/g474_cmake_demo/build/g474_cmake_demo.elf...
  (gdb) b MX_GPIO_Init
  Breakpoint 1 at 0x80002da: file L:/Y_exps_drafts_debugs_logs_exmpls/goryachev_soft/g474_cmake_demo/Src/main.c, line 157.
  (gdb) target extended-remote :3333
  Remote debugging using :3333
  (gdb) run
  Starting program: K:\Y_exps_drafts_debugs_logs_exmpls\goryachev_soft\g474_cmake_demo\build\g474_cmake_demo.elf

  Breakpoint 1, MX_GPIO_Init ()
      at L:/Y_exps_drafts_debugs_logs_exmpls/goryachev_soft/g474_cmake_demo/Src/main.c:157
  157     in L:/Y_exps_drafts_debugs_logs_exmpls/goryachev_soft/g474_cmake_demo/Src/main.c
  (gdb) 
  ...
  ```