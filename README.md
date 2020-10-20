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