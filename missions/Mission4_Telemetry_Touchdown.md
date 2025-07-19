# 🌐 RedMars Labs: Operation MoonBase Rescue  
## Mission 4: “Telemetry Touchdown” 📡

> **Scenario:**  
> You’ve survived the crash. The lights are on.  
> Your emergency Morse communicator works… but what if you're not right at the system?  
> It's time to take advantage of your ESP32-S3's built-in **Wi-Fi** to build a **command dashboard**.  
> This webpage lets you check if your systems are functioning—and maybe even send a **custom Morse message back to Earth**.

---

## 🎯 Mission Objective

Build a **Wi-Fi-based system health dashboard** served from the ESP32-S3.  
The dashboard should:
- Show the current status of key systems (LED, buzzer, Morse input)
- Let you access it from your laptop or phone (via ESP32 access point)
- Include a hard challenge: send a message from the browser and transmit it in **Morse code** using the buzzer!

---

## 🧰 Supplies

| Part | Quantity |
|------|----------|
| ESP32-S3 Dev Board | 1 |
| Buzzer (Piezo) | 1 |
| LEDs | 1–3 |
| Push Button | 1 (optional) |
| Breadboard | 1 |
| Jumper Wires | Several |

---

## 📟 Code: ESP32 Morse Dashboard

```python
import network
import socket
from machine import Pin, PWM
import time

# === Hardware Setup ===
led = Pin(4, Pin.OUT)
buzzer = PWM(Pin(10))
buzzer.freq(1000)
buzzer.duty_u16(0)

# === Morse Dictionary ===
MORSE_CODE = {
    'A': '.-', 'B': '-...', 'C': '-.-.', 'D': '-..', 'E': '.',
    'F': '..-.', 'G': '--.', 'H': '....', 'I': '..', 'J': '.---',
    'K': '-.-', 'L': '.-..', 'M': '--', 'N': '-.', 'O': '---',
    'P': '.--.', 'Q': '--.-', 'R': '.-.', 'S': '...', 'T': '-',
    'U': '..-', 'V': '...-', 'W': '.--', 'X': '-..-', 'Y': '-.--',
    'Z': '--..', ' ': '/'
}

# === Wi-Fi AP ===
def start_ap():
    ap = network.WLAN(network.AP_IF)
    ap.active(True)
    ap.config(essid='ESP32_MoonBase', password='moonmission')
    while not ap.active():
        pass
    print('AP Ready:', ap.ifconfig())
    return ap

# === Morse Transmission ===
def play_morse(msg):
    unit = 0.2  # 200ms
    for char in msg.upper():
        code = MORSE_CODE.get(char, '')
        for symbol in code:
            if symbol == '.':
                buzzer.duty_u16(32768)
                time.sleep(unit)
            elif symbol == '-':
                buzzer.duty_u16(32768)
                time.sleep(3 * unit)
            buzzer.duty_u16(0)
            time.sleep(unit)
        time.sleep(2 * unit)

# === Web Page ===
def web_page(status):
    return f"""<!DOCTYPE html>
<html><head><title>MoonBase Telemetry</title></head>
<body style='background:black; color:#39ff14; font-family:monospace'>
<h2>🚀 RedMars MoonBase Telemetry</h2>
<p>LED: {'ON' if status['led'] else 'OFF'}</p>
<p>Buzzer: {'ON' if status['buzzer'] else 'OFF'}</p>
<hr>
<form action="/morse" method="get">
  <label>Send Morse Message:</label><br>
  <input name="msg" maxlength="30">
  <button type="submit">Transmit</button>
</form>
</body></html>
"""

# === Web Server ===
def start_server():
    status = {'led': False, 'buzzer': False}
    s = socket.socket()
    s.bind(('0.0.0.0', 80))
    s.listen(1)
    print("Server running at 192.168.4.1")

    while True:
        conn, addr = s.accept()
        request = conn.recv(1024).decode()
        print("Request:", request)

        if '/morse?msg=' in request:
            msg = request.split('/morse?msg=')[1].split(' ')[0]
            msg = msg.replace('%20', ' ')
            print("Transmitting:", msg)
            led.value(1)
            play_morse(msg)
            led.value(0)

        response = web_page(status)
        conn.send('HTTP/1.1 200 OK\r\nContent-Type: text/html\r\n\r\n')
        conn.sendall(response.encode())
        conn.close()

# === Run It All ===
start_ap()
start_server()
```

---

## 🔤 Morse Code Reference Table

```
A: .-      B: -...    C: -.-.    D: -..     E: .
F: ..-.    G: --.     H: ....    I: ..      J: .---
K: -.-     L: .-..    M: --      N: -.      O: ---
P: .--.    Q: --.-    R: .-.     S: ...     T: -
U: ..-     V: ...-    W: .--     X: -..-    Y: -.--    Z: --..
```

---

## 🧩 Challenge Extensions

- Display your custom Morse messages on an OLED or web console
- Track system uptime or log errors to a file
- Use an LED as a Morse visual signal alongside the buzzer
- Add authentication to the dashboard for “Mission Command Access Only”

---

🛰 Made with ❤️ by RedMars Labs  
`Learn to Build. Build to Explore.`  
