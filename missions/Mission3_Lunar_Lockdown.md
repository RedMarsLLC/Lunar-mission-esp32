# 🛰️ RedMars Labs: Operation MoonBase Rescue  
## Mission 3: "Lunar Lockdown – Tap to Talk" 🧑‍🚀🔘

> **Scenario:**  
> Your SOS beacon has been running for hours. Suddenly—**contact**.  
> The communications system comes partially online, but you have no microphone.  
> You can send signals through it… just not voice.  
> With your ESP32-S3 and a push button, you’ll now **manually tap out Morse code messages** to the ground.

---

## 🎯 Mission Objective

Create a **manual Morse Code transmitter** using a button and buzzer on the ESP32-S3.  
Send full messages using taps and holds—just like a real telegraph.

---

## 🧰 Supplies

| Part | Quantity |
|------|----------|
| ESP32-S3 Dev Board | 1 |
| Breadboard | 1 |
| Push Button | 1 |
| 10kΩ Resistor (pull-down) | 1 |
| Piezo Buzzer | 1 |
| Jumper Wires | Several |
| USB-C Cable | 1 |

---

## 📡 Mission Explanation

In this mission, you will:
- Read **button press durations**
- Translate them into **dots and dashes**
- Output via buzzer for **audible feedback**
- Display the corresponding **Morse letter**
- Learn the full **Morse alphabet**

---

## 📘 The Morse Code Reference

```
A: .-      B: -...    C: -.-.    D: -..     E: .
F: ..-.    G: --.     H: ....    I: ..      J: .---
K: -.-     L: .-..    M: --      N: -.      O: ---
P: .--.    Q: --.-    R: .-.     S: ...     T: -
U: ..-     V: ...-    W: .--     X: -..-    Y: -.--    Z: --..
```

---

## ⚡ Wiring the Button

```
[ GPIO 12 ] ────────┐
                   │
                [ Button ]
                   │
                [ GND ]
Pull-down:
[ GPIO 12 ] ──┐  
              └── [10kΩ] ── GND
```

---

## 🧠 Core Code Logic

```python
from machine import Pin, PWM
import time

buzzer = PWM(Pin(10))
buzzer.freq(1000)

button = Pin(12, Pin.IN, Pin.PULL_DOWN)

DOT_TIME = 0.2  # Max duration for a "dot"
GAP_TIME = 1.5  # Time between characters

morse_buffer = ""
last_press_time = 0

MORSE_DICT = {
    '.-': 'A', '-...': 'B', '-.-.': 'C', '-..': 'D', '.': 'E',
    '..-.': 'F', '--.': 'G', '....': 'H', '..': 'I', '.---': 'J',
    '-.-': 'K', '.-..': 'L', '--': 'M', '-.': 'N', '---': 'O',
    '.--.': 'P', '--.-': 'Q', '.-.': 'R', '...': 'S', '-': 'T',
    '..-': 'U', '...-': 'V', '.--': 'W', '-..-': 'X', '-.--': 'Y', '--..': 'Z'
}

def beep(duration):
    buzzer.duty_u16(32768)
    time.sleep(duration)
    buzzer.duty_u16(0)

print("Start tapping to send Morse...")

while True:
    if button.value() == 1:
        press_start = time.ticks_ms()
        while button.value() == 1:
            pass
        duration = (time.ticks_ms() - press_start) / 1000

        if duration < DOT_TIME:
            morse_buffer += '.'
            beep(0.1)
        else:
            morse_buffer += '-'
            beep(0.3)

        print("Current: ", morse_buffer)

        last_press_time = time.time()

    # Detect character pause
    if morse_buffer and time.time() - last_press_time > GAP_TIME:
        letter = MORSE_DICT.get(morse_buffer, '?')
        print("→", letter)
        morse_buffer = ""
```

---

## 🌕 Mission Debrief

You’ve built a **manual Morse communicator**.  
It may not be fast—but in space, **every signal counts**.  
Your creativity just gave you a lifeline home.

---

## 🧩 Challenge Modes

- Add LED or screen to show sent message
- Store the message in a list or send over WiFi in a future mission
- Add multi-button support for dash/dot mode

---

🛰 Made with ❤️ by RedMars Labs  
`Learn to Build. Build to Explore.`  
