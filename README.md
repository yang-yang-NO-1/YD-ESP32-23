# YD-ESP32-23

![img](https://raw.githubusercontent.com/rtek1000/YD-ESP32-23/main/yd_esp32_s3-23.jpg)

- Ref.: [YD-ESP32-S3 N16R8](https://circuitpython.org/board/yd_esp32_s3_n16r8/)

- The device uses the ESP32-S3 chip, which can be used for the test prototype of the Internet of Things application and can also be used for practical applications. It is equipped with two USBs, one is a hardware USB-to-serial port (CH343P WCH Qinheng), and the other is ESP32-S3 usb port.

- - [CircuitPython 9.2.8] Built-in modules available: _asyncio, _bleio, _eve, _pixelmap, adafruit_bus_device, adafruit_pixelbuf, aesio, alarm, analogbufio, analogio, array, atexit, audiobusio, audiocore, audiomixer, audiomp3, binascii, bitbangio, bitmapfilter, bitmaptools, board, builtins, builtins.pow3, busdisplay, busio, busio.SPI, busio.UART, canio, codeop, collections, countio, digitalio, displayio, dualbank, epaperdisplay, errno, espcamera, espidf, espnow, espulp, fontio, fourwire, framebufferio, frequencyio, getpass, gifio, hashlib, i2cdisplaybus, io, ipaddress, jpegio, json, keypad, keypad.KeyMatrix, keypad.Keys, keypad.ShiftRegisterKeys, keypad_demux, keypad_demux.DemuxKeyMatrix, locale, math, max3421e, mdns, memorymap, microcontroller, msgpack, neopixel_write, nvm, onewireio, os, os.getenv, paralleldisplaybus, ps2io, pulseio, pwmio, qrio, rainbowio, random, re, rgbmatrix, rotaryio, rtc, sdcardio, sdioio, select, sharpdisplay, socketpool, socketpool.socketpool.AF_INET6, ssl, storage, struct, supervisor, synthio, sys, terminalio, tilepalettemapper, time, touchio, traceback, ulab, usb, usb_cdc, usb_hid, usb_midi, vectorio, warnings, watchdog, wifi, zlib; Included frozen(?) modules: neopixel

Pinout:
![img](https://raw.githubusercontent.com/rtek1000/YD-ESP32-23/main/yd-esp32-s3-devkitc-1-clone-pinout.jpg)

- Ref.: [VCC-GND Studio YD-ESP32-S3 (DevKitC 1 clone): high-resolution pinout and specs](https://mischianti.org/vcc-gnd-studio-yd-esp32-s3-devkitc-1-clone-high-resolution-pinout-and-specs/)

#### Note:
- The ‘ESP32 S3 DevKitC1 Clone’ board has a jumper called 'RGB', another called 'IN-OUT', and another called 'USB-OTG', all open. But it may be necessary to solder the jumper for the devices to work.
- - The 'IN-OUT' jumper, when closed, bypasses one diode, making USB VBus power coming to 5Vin. If 5Vin is also connected to external source, it can get back-fed by USB, which is usually undesirable. But USB bus is protected by another diode, it cannot get back-fed by external source. When In-Out is open, 5Vin and USB VBus are separated by diode, USB power does not come to 5Vin.
- - The 'USB-OTG' jumper, when closed, connects together USB VBus lines from both USB-C connectors.
- - Ref.: [Third-party ESP32-S3 development boards 'IN-OUT' and 'USB-OTG' pads - what do they do?](https://www.reddit.com/r/esp32/comments/10rdngp/thirdparty_esp32s3_development_boards_inout_and/?rdt=39953)


- The RGB LED did not work with common digitalWrite() commands. RGB LED only worked with neopixelWrite() commands.
- - Arduino IDE: There is a BlinkRGB under the ESP32->GPIO examples that uses the onboard RGB LED.
- - Ref.: https://forum.arduino.cc/t/esp32-s3-devkit-problems/1136923/4
- - Need add: '#define RGB_BUILTIN 48'
- - Avoid looking directly at the LED, place a sheet of paper or a piece of white plastic material over the LED to serve as a diffuser.

Schematic (Jumpers were not included):
![img](https://raw.githubusercontent.com/rtek1000/YD-ESP32-23/main/schematic.png)

-----

Example of ESP32-S3 and support for mini keyboard with built-in touchpad: [here](https://github.com/rtek1000/ESP32-S3_USB_Host_HID_Keyboard)

![img](https://raw.githubusercontent.com/rtek1000/ESP32-S3_USB_Host_HID_Keyboard/main/Mini%20Keyboard%20With%20Touchpad%20Built-in.jpg)

-----
-----

#### Warning:

User [j2s](https://github.com/rtek1000/YD-ESP32-23/issues/3) reported that on his board the 5V pin works as another 3V3 pin, check your board before using.

![img](https://raw.githubusercontent.com/rtek1000/YD-ESP32-23/refs/heads/main/YD-ESP32-23_V1120.jpg)


# 通用GPIO
| GPIO 引脚 | 类型 | 备注（使用优势）                     |
| --------- | ---- | ------------------------------------ |
| GPIO10    | 通用 | 无特殊复用，开发板通常无焊接占用     |
| GPIO11    | 通用 | 同上，与 GPIO10 成对使用更方便       |
| GPIO12    | 通用 | 兼容大部分 ESP32-S3 开发板           |
| GPIO13    | 通用 | 无启动 / 下载相关复用                |
| GPIO14    | 通用 | 无特殊功能绑定                       |
| GPIO15    | 通用 | 无系统级复用                         |
| GPIO16    | 通用 | 可兼容 PWM/ADC（若需扩展功能）       |
| GPIO17    | 通用 | 同上                                 |
| GPIO18    | 通用 | 无默认复用                           |
| GPIO19    | 通用 | 无默认复用                           |
| GPIO20    | 通用 | 无默认复用                           |
| GPIO21    | 通用 | 无默认复用                           |
| GPIO22    | 通用 | 无默认复用                           |
| GPIO23    | 通用 | 无默认复用                           |
# 复用GPIO
| GPIO 引脚 | 核心复用功能                | 使用注意事项                                                                                                   |
| --------- | --------------------------- | -------------------------------------------------------------------------------------------------------------- |
| GPIO0     | BOOT 启动模式引脚           | 1. 悬空 = 高电平，接 GND = 进入下载模式；2. 启动后可作为普通 GPIO，但复位后仍会影响启动；3. 不推荐做输入（易误触发下载模式） |
| GPIO1     | USB_BOOT 引脚（USB 下载）   | 1. 硬件默认下拉，常态低电平；2. 复用为 USB DFU 下载功能，解除复用后仍可能被系统占用；3. 做输入时电平始终偏低（你遇到的问题根源） |
| GPIO2     | UART0 RX（串口下载）        | 1. 部分开发板复用为串口下载的 RX 引脚；2. 做普通 GPIO 需先禁用串口下载功能                                       |
| GPIO3     | UART0 TX（串口下载）        | 1. 部分开发板复用为串口下载的 TX 引脚；2. 做输出时可能干扰程序下载                                             |
| GPIO4     | SD 卡数据引脚               | 1. 若开发板带 SD 卡槽，会被占用；2. 无 SD 卡时可解除复用做普通 GPIO                                             |
| GPIO5     | SD 卡数据引脚               | 同上                                                                                                           |
| GPIO6-9   | SPI Flash 引脚（QSPI）      | 1. 核心存储引脚，绝对禁止使用；2. 占用会导致程序无法启动 / 崩溃                                                 |
| GPIO40    | USB D + 引脚（USB 数据）    | 1. 板载 USB 口的差分数据引脚；2. 占用会导致 USB 下载 / 虚拟串口失效                                           |
| GPIO41    | USB D - 引脚（USB 数据）    | 同上                                                                                                           |
| GPIO42    | RTC 时钟 / 触摸引脚         | 1. 默认复用为 RTC 低功耗时钟；2. 做普通 GPIO 需关闭 RTC 功能                                                   |
| GPIO43    | UART2 TX（硬件串口）        | 1. 系统默认 UART2 的 TX 引脚；2. 做普通 GPIO 需先解除 UART2 绑定（Serial2.end()）                               |
| GPIO44    | UART2 RX（硬件串口）        | 1. 系统默认 UART2 的 RX 引脚；2. 做普通 GPIO 需先解除 UART2 绑定                                               |
| GPIO45    | LEDC/PWM 引脚               | 1. 部分开发板默认作为板载 LED 引脚；2. 做输入需先关闭 PWM 输出                                                 |
| GPIO46    | LEDC/PWM 引脚               | 同上  
#  总结                                           

1. 通用 GPIO：GPIO10-23 无默认复用，是普通输入 / 输出的首选，无需额外配置；
2. 高危复用引脚：GPIO0/GPIO1/GPIO6-9 绝对不推荐做普通 GPIO，易导致启动失败 / 电平异常；
3. 低危复用引脚：GPIO43/GPIO44（UART2）、GPIO45/GPIO46（PWM）可解除复用后使用，但需先关闭对应功能。
