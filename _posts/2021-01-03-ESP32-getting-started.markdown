---
layout: post
title:  "ESP32 Getting Started Guide"
date:   2021-01-03 13:21:35 +0530
categories: jekyll update
---

Look at ESP32 and identify the USB-TTL chip model.

Based on that install the drives for that chipset. This will enable us to connect to ESP32 using the serial consoles like Putty(Windows), minicom(Linux) & screen(Linux).

```
(base) C:\Users\rchinnathambi>esptool --port COM7 flash_id
esptool.py v2.8
Serial port COM7
Connecting.....
Detecting chip type... ESP32
Chip is ESP32D0WDQ6 (revision 1)
Features: WiFi, BT, Dual Core, 240MHz, VRef calibration in efuse, Coding Scheme None
Crystal is 40MHz
MAC: 8c:aa:b5:8b:f1:a4
Uploading stub...
Running stub...
Stub running...
Manufacturer: 5e
Device: 4016
Detected flash size: 4MB
Hard resetting via RTS pin...

(base) C:\Users\rchinnathambi\Downloads>esptool --chip esp32 --port COM7 erase_flash
esptool.py v2.8
Serial port COM7
Connecting....
Chip is ESP32D0WDQ6 (revision 1)
Features: WiFi, BT, Dual Core, 240MHz, VRef calibration in efuse, Coding Scheme None
Crystal is 40MHz
MAC: 8c:aa:b5:8b:f1:a4
Uploading stub...
Running stub...
Stub running...
Erasing flash (this may take a while)...
Chip erase completed successfully in 12.4s
Hard resetting via RTS pin...

(base) C:\Users\rchinnathambi\Downloads>esptool --chip esp32 --port COM7 --baud 460800 write_flash -z 0x1000 esp32-idf3-20200902-v1.13.bin
esptool.py v2.8
Serial port COM7
Connecting.....
Chip is ESP32D0WDQ6 (revision 1)
Features: WiFi, BT, Dual Core, 240MHz, VRef calibration in efuse, Coding Scheme None
WARNING: Detected crystal freq 41.01MHz is quite different to normalized freq 40MHz. Unsupported crystal in use?
Crystal is 40MHz
MAC: 8c:aa:b5:8b:f1:a4
Uploading stub...
Running stub...
Stub running...
Changing baud rate to 460800
Changed.
Configuring flash size...
Auto-detected Flash size: 4MB
Compressed 1448768 bytes to 926007...
Wrote 1448768 bytes (926007 compressed) at 0x00001000 in 22.1 seconds (effective 523.4 kbit/s)...
Hash of data verified.

Leaving...
Hard resetting via RTS pin...
```

**FLASHING MICRO PYTHON ON ESP32**


**ESP32 BOARD**
```
esptool --port COM7 flash_id
esptool --chip esp32 --port COM7 erase_flash
esptool --chip esp32 --port COM7 --baud 460800 write_flash -z 0x1000 esp32-idf3-20200902-v1.13.bin
```

**ESP8266 BOARD**
```
esptool --port COM7 flash_id
esptool --chip esp8266 --port COM7 erase_flash
esptool --chip esp8266 --port COM7 --baud 460800 write_flash --flash_size=detect 0 esp8266-20200911-v1.13.bin
```

Open REPL Terminal in MicroPython
                1. Open Putty
                2. Select "Serial"
                3. Select COMX based on "Windows Device Manager"
                4. Speed 115200
                5. Open

 

**SAMPLE COMMANDS TO TURN ON LED PIN**

```python
## Turn on LED
from machine import Pin
from time import sleep
led = Pin(2,Pin.OUT)
led.value(1)
sleep(1)
led.value(0)

## Blink LED 9 times
from machine import Pin
from time import sleep

led = Pin(2,Pin.OUT)
i=1
While i < 10:
    i = i + 1
    led.value(not led.value())
    sleep(1)
```

**CONFIGURE ESP32 AS WIFI CLIENT**

**Connect to WiFi**

```python
import network
wlan = network.WLAN(network.STA)    ## For ESP32
wlan = network.WLAN(network.STA_IF) ## For ESP8266
essid = "skanda-dl"
essid_password = "Knock!Knock100"
wlan.active()
wlan.active(True)
wlan.active()
wlan.scan()
wlan.connect(essid,essid_password)
wlan.isconnected()
wlan.ifconfig()
wlan.config("mac")
```

**Disconnect from WiFi**
```python
wlan.disconnect()
wlan.isconnected()
```

**SIMPLE ESP32 AND ESP8266 WIFI - SHORT COMMANDS**

```python
import network
wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect('skanda-dl', 'Knock!Knock100')
wlan.ifconfig()
```

**SIMPLE OF WEBREPL**

```python
import network
import webrepl

wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect('skanda-dl', 'Knock!Knock100')
wlan.ifconfig()
webrepl.start()
```

**FETCH INFORMATION FROM INTERNET**

```python
import urequests
response = urequests.get('https://ifconfig.me/')
print(response.content)
```

**CREATE NEW PYTHON VIRTUAL ENVIRONMENT IN ANACONDA**

**In putty serial REPL (To avoid errors and interfering file transfers)**

```
>>> import esp
>>> esp.osdebug(None)
```

**In Anaconda command prompt**

```
conda create -n ampy python=3.7

conda activate ampy

(ampy) > pip install ampy-adafruit
(ampy) > ampy --help
(ampy) > ampy --port COM4 --baud 115200 ls
(ampy) > ampy --port COM4 --baud 115200 get boot.py
(ampy) > ampy --port COM4 --baud 115200 get boot.py boot.py
(ampy) > pwd
(ampy) > ampy --port COM4 --baud 115200 get boot.py
```