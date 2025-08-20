# embed_club_workshop_project
# Morse Code Decoder/Encoder using Arduino

This project implements a **Morse code encoder and decoder** using an Arduino.  
You can either:
- **Key in a Morse code sequence** using a button, and the Arduino will decode it into letters/numbers.
- **Type letters/numbers into the Serial Monitor**, and the Arduino will encode them into Morse code using an LED and buzzer.

---

## Features
- Encode text typed in the Serial Monitor into Morse code (LED + buzzer).
- Decode Morse code entered using a button into text printed on the Serial Monitor.
- Supports **A–Z letters** and **0–9 numbers**.
- Provides real-time feedback with **beeps and LED flashes**.
- Adjustable **Words Per Minute (WPM)** speed by changing `dotLength`.

---

## Components Required
- Arduino (Uno/Nano/etc.)
- Push button
- LED (with 220Ω resistor)
- Piezo buzzer or small speaker
- Jumper wires
- Breadboard

---

## Circuit Diagram
- **Piezo buzzer/speaker** → Pin `2` and GND  
- **Push button** → Pin `8` and GND  
- **LED (+)** → Pin `13`  
- **LED (-)** → 220Ω resistor → GND  

---

## Code Details
- `dotLength` defines Morse speed. Default is `240ms` (≈ 5 WPM).  
  - Formula: `WPM = 1200 / dotLength`
- Dot (`.`) = 1 × `dotLength`  
- Dash (`-`) = 3 × `dotLength`  
- Letter space = 3 × `dotLength`  
- Word space = 7 × `dotLength`

When typing text in Serial Monitor:
- Letters/numbers are flashed and beeped as Morse.
- Spaces insert word breaks.

When using the button:
- Short press → dot (`.`)  
- Long press → dash (`-`)  
- Short gap between presses → continues same letter.  
- Longer gap → ends letter.  
- Very long gap → ends word.  

---

## How to Use
1. **Build the circuit** as described above.
2. Upload the provided sketch to Arduino.
3. Open **Serial Monitor** (baud rate: `9600`).
4. Options:
   - **Type text** → Arduino flashes and beeps Morse.
   - **Press the button** → Arduino decodes Morse into text.

---

## Example
- Type `HELLO` in Serial Monitor → LED and buzzer output:
H ....
E .
L .-..
L .-..
O ---


