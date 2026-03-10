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


# YD-ESP32-23 GPIO 使用说明（精简版）

适用于基于 **ESP32-S3** 的 **YD-ESP32-23** 类开发板（双 Type-C、CH343P、板载 RGB LED）。

## GPIO 使用结论

### 推荐优先使用
- GPIO1
- GPIO2
- GPIO4
- GPIO5
- GPIO6
- GPIO7
- GPIO8
- GPIO9
- GPIO10
- GPIO11
- GPIO12
- GPIO13
- GPIO14
- GPIO15
- GPIO16
- GPIO17
- GPIO18
- GPIO21
- GPIO47

### 可复用但需谨慎
- GPIO0
- GPIO3
- GPIO19
- GPIO20
- GPIO39
- GPIO40
- GPIO41
- GPIO42
- GPIO43
- GPIO44
- GPIO45
- GPIO46
- GPIO48

### 不可作为普通 IO 使用
- GPIO26
- GPIO27
- GPIO28
- GPIO29
- GPIO30
- GPIO31
- GPIO32

### R8 方案额外不可用
- GPIO33
- GPIO34
- GPIO35
- GPIO36
- GPIO37

## 分类说明表

| 分类 | GPIO | 说明 |
|---|---|---|
| 推荐通用 | 1, 2, 4~18, 21, 47 | 适合普通输入输出、I2C、SPI、UART、PWM |
| 启动相关 | 0, 3, 45, 46 | Strapping 引脚，上电电平敏感 |
| 原生 USB | 19, 20 | 占用后会影响原生 USB |
| 调试相关 | 39, 40, 41, 42 | 常与 JTAG 调试复用 |
| 下载日志串口 | 43, 44 | 建议保留给烧录和串口日志 |
| 板载资源 | 48 | 常与板载 RGB LED 相关 |
| 内部存储 | 26~32 | Flash / PSRAM 内部连接，不可用 |
| R8 额外占用 | 33~37 | Octal Flash / PSRAM 连接，R8 时不可用 |

## 推荐外设分配

| 外设 | 推荐引脚 |
|---|---|
| I2C | SDA = GPIO8，SCL = GPIO9 |
| SPI | CS = GPIO10，MOSI = GPIO11，SCK = GPIO12，MISO = GPIO13 |
| SPI 控制脚 | DC = GPIO14，RST = GPIO15，BL = GPIO16 |
| 额外 UART | TX = GPIO17，RX = GPIO18 |
| 按键输入 | GPIO4，GPIO5，GPIO6，GPIO7 |
| PWM / 背光 / 蜂鸣器 | GPIO15，GPIO16，GPIO17，GPIO18，GPIO21，GPIO47 |
| 中断输入 | GPIO1，GPIO2，GPIO4，GPIO5，GPIO6，GPIO7，GPIO21 |

## 推荐开发规则

```text
1. 主力可用 GPIO：1、2、4~18、21、47
2. 建议保留：19、20、43、44、48
3. 禁止使用：26~32
4. 如果是 R8 模组，再额外禁止：33~37
5. 不要优先使用 0、3、45、46 作为按键或外部强驱动输入
```

## 默认接线建议

```text
I2C:
- SDA = GPIO8
- SCL = GPIO9

SPI:
- CS   = GPIO10
- MOSI = GPIO11
- SCK  = GPIO12
- MISO = GPIO13

控制脚:
- DC  = GPIO14
- RST = GPIO15
- BL  = GPIO16

UART:
- TX = GPIO17
- RX = GPIO18
```

## 一句话总结

```text
最稳的用法是：
优先使用 GPIO1、2、4~18、21、47；
保留 GPIO19、20、43、44、48；
禁用 GPIO26~32；
R8 模组再额外禁用 GPIO33~37。
```