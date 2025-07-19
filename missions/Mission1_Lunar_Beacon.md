# 🛰️ RedMars Labs: Operation MoonBase Rescue
## Mission 1: "Blink for Backup" 🚨

> **Scenario:**  
> You’ve crash-landed on the Moon. Power is limited. Communication is down.  
> All you’ve got is an ESP32-S3 dev board, a few components, and your wits.  
> It's time to signal for help. You know that someone will be looking for you
> and with ground stations pointing cameras to your location, you may be able
> to signal with what you have!

---

## 🎯 Mission Objective

Build an **SOS beacon** using MicroPython and an LED to signal for help.  
You’ll blink the light in **Morse code** to transmit “S.O.S” repeatedly.

---

## 🧰 Supplies

| Part | Quantity |
|------|----------|
| ESP32-S3 Dev Board | 1 |
| Breadboard | 1 |
| LED (any color) | 1 |
| 100Ω resistor | 1 |
| Jumper wires | ~4 |
| USB-C cable | 1 |
| Computer with VS Code or Thonny | 1 |

---

## 🛠️ Setup

### 1. Flash MicroPython to the ESP32-S3

- Download the latest [MicroPython firmware](https://micropython.org/download/esp32/)
- Use the [esptool](https://github.com/espressif/esptool) or [Thonny](https://thonny.org/) to flash it

```bash
esptool.py --chip esp32s3 erase_flash
esptool.py --chip esp32s3 --baud 460800 write_flash -z 0x0 esp32s3-xxxx.bin
```

---

### 2. Connect the LED

```
[ GPIO 4 ] ─── [ 100Ω Resistor ] ───▶|─── GND
                                  LED
```

---

### 3. Your First Blink (Warm-up)

```python
from machine import Pin
import time

led = Pin(4, Pin.OUT)

while True:
    led.value(1)
    time.sleep(0.5)
    led.value(0)
    time.sleep(0.5)
```

---

### 4. Send “SOS” in Morse Code

```python
from machine import Pin
import time

led = Pin(4, Pin.OUT)

DOT = 0.2
DASH = DOT * 3
PAUSE = DOT
GAP = DOT * 3

S = [DOT, DOT, DOT]
O = [DASH, DASH, DASH]
MORSE = S + [GAP] + O + [GAP] + S

def sos():
    for duration in MORSE:
        led.value(1)
        time.sleep(duration)
        led.value(0)
        time.sleep(PAUSE)

while True:
    sos()
    time.sleep(5)
```

---

## 🌌 Mission Debrief

You’ve built a functional optical beacon for rescue using limited resources.  
Your lunar survival chances just went up.

---

## 🧑‍🚀 What’s Next?

**Mission 2: Buzzer Basecamp**  
Use PWM to emit SOS tones and expand your beacon to light + sound!

---

🛰 Made with ❤️ by RedMars Labs  
`Learn to Build. Build to Explore.`  
