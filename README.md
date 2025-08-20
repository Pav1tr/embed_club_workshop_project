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

## Important Variables

```cpp
int tonePin = 2;       // Pin connected to buzzer/speaker
int toneFreq = 1000;   // Buzzer sound frequency (Hz)
int ledPin = 13;       // Pin connected to LED
int buttonPin = 8;     // Button input pin
int dotLength = 240;   // Basic Morse timing unit (ms) → ~5 words/minute
```

From `dotLength`, the program calculates:

* `dashLength = dotLength * 3`
* `letterSpace = dotLength * 3`
* `wordSpace = dotLength * 7`

So dots, dashes, gaps between letters and words all follow Morse code timing.

---

## Morse Code Storage

The Morse patterns are stored in arrays:

```cpp
char* letters[] = { ".-", "-...", "-.-.", ... };   // A–Z
char* numbers[] = { "-----", ".----", "..---", ... }; // 0–9
```

So:

* `letters[0] = ".-"` → "A"
* `letters[1] = "-..."` → "B"
* `numbers[0] = "-----"` → "0"

---

## setup()

* Sets pin modes.
* Starts Serial at `9600 baud`.
* Prints welcome text and Morse speed.
* Tests LED + buzzer.
* Demonstrates Morse code for **A, B, C**.

---

## loop()

This is where the real action happens. Two modes:

### Serial Input → Encode to Morse

```cpp
if (Serial.available() > 0)
```

* Reads characters typed into Serial Monitor.
* If it’s a letter:

  * Converts to uppercase.
  * Looks up its Morse code sequence from `letters[]`.
  * Prints both text + Morse sequence in Serial.
  * Flashes LED + buzzer to output Morse.
* If it’s a number:

  * Same process but using `numbers[]`.
* If it’s a space:

  * Adds a word gap.

Example:
Typing `C` → Arduino prints:

```
C -.-.
```

and LED + buzzer play `-.-.`

---

### Button Input → Decode from Morse

```cpp
if (digitalRead(buttonPin) == LOW)
```

* Button press starts a timer (`t1`).
* When released, timer stops (`t2`).
* Press duration = `onTime = t2 - t1`.

Logic:

* If `onTime` ≤ 1.5 × dot → **dot** (`.`)
* Else → **dash** (`-`)

Each dot/dash gets added to a sequence:
`dashSeq = dashSeq + "."` or `dashSeq + "-"`

---

### Detecting End of Letter/Word

After button presses:

* If the **gap** after release ≥ `letterSpace` → that’s the end of a letter.
  → Compare `dashSeq` with stored `letters[]` and `numbers[]`.
  → Print the matching character, or `?` if not found.

* If the gap ≥ `wordSpace` → insert `_` (represents space between words).

---

## Flash and Beep Functions

These just control LED + buzzer for dots and dashes.

```cpp
void flashDotOrDash(char dotOrDash) {
  digitalWrite(ledPin, HIGH);
  tone(tonePin, toneFreq);
  if (dotOrDash == '.') delay(dotLength);
  else delay(dashLength);
  digitalWrite(ledPin, LOW);
  noTone(tonePin);
  delay(dotLength); // gap between elements
}
```

So each dot/dash is a **sound + LED blink** with correct timing.

---

## In Short

* **Keyboard input**: Text → Morse (LED + buzzer).
* **Button input**: Morse (presses) → Text (Serial).
* Uses **timing** to decide dot/dash and gaps.
* Looks up patterns in arrays to encode/decode.

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




