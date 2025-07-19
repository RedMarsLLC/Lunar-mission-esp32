# 🛰️ RedMars Labs: Operation MoonBase Rescue
## Mission 2: "Buzzer Basecamp" 🔊

> **Scenario:**  
> Outside your lunar module, your LED beacon flashes silently.  
> Inside, the air is still—until you wire up your **buzzer heartbeat monitor**.  
> Sound can’t travel in space, but **inside your pressurized cabin**, it can.

---

## 🎯 Mission Objective

Add a **buzzer** to your ESP32-S3 using MicroPython and PWM.  
Create an audible **SOS signal**, a **heartbeat ping**, or a **status alarm**.

---

## 🧰 Supplies

| Part | Quantity |
|------|----------|
| ESP32-S3 Dev Board | 1 |
| Breadboard | 1 |
| Piezo Buzzer (active/passive) | 1 |
| Jumper wires | ~3 |
| USB-C cable | 1 |

---

## 🌌 Science Debrief

- Sound can’t travel in the vacuum of the Moon.
- But it **can** inside your air-filled habitat.
- This mission simulates your **internal status alarm system**.

---

## 🛠️ Circuit

```
[ GPIO 10 ] ─────▶|──── GND  
               Buzzer
```

*(Note: GPIO 10 is used for PWM in this example.)*

---

## 🧪 Example 1: Heartbeat Beep

```python
from machine import Pin, PWM
import time

buzzer = PWM(Pin(10))
buzzer.freq(1000)  # 1 kHz tone

while True:
    buzzer.duty_u16(32768)  # 50% volume
    time.sleep(0.1)
    buzzer.duty_u16(0)      # Off
    time.sleep(3)           # Heartbeat interval
```

---

## 🛰 Example 2: SOS Morse Code Tone

```python
from machine import Pin, PWM
import time

buzzer = PWM(Pin(10))
buzzer.freq(1000)

DOT = 0.2
DASH = DOT * 3
PAUSE = DOT
GAP = DOT * 3

S = [DOT, DOT, DOT]
O = [DASH, DASH, DASH]
MORSE = S + [GAP] + O + [GAP] + S

def beep(duration):
    buzzer.duty_u16(32768)
    time.sleep(duration)
    buzzer.duty_u16(0)
    time.sleep(PAUSE)

def sos():
    for duration in MORSE:
        beep(duration)

while True:
    sos()
    time.sleep(5)
```

---

## 🧑‍🚀 Mission Debrief

You've created a **functional audio system** for internal alerts—  
critical for keeping track of beacon operations in a sealed environment.

---

## 🧩 Challenge Mode

- Add a **button** that triggers the buzzer manually (prep for Mission 3)
- Combine this with LED blinking for a dual-mode beacon
- Use different tones for system states

---

## 📡 Next Up...

**Mission 3: Lunar Lockdown**  
Add buttons to interact with your system manually: toggle lights, trigger beacons, or reset status!

---

🛰 Made with ❤️ by RedMars Labs  
`Learn to Build. Build to Explore.`  
