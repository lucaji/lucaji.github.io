# AM Radio on Arduino UNO


## 🛠️ Introduction

This blog article gathers a series of **DIY radio experiments**, ranging from early analog detection with *Galena crystal receivers* to digitally driven AM transmission via **Arduino microcontrollers**. Inspired by a love for vintage radio sets and low-power communication techniques, this collection explores both the simplicity of early unpowered detection and the elegance of amplitude modulation (AM) implemented in modern embedded systems. These are primarily **short-range (QRP)** experiments — ideal for educational settings, hobbyists, and radio tinkerers looking to bridge past and present technologies.

## 📻 Arduino AM Modulator

The first project demonstrates a **very low-power AM transmitter** centered around an **Arduino UNO**. Operating on the **LW (Longwave) radio band at ~734 kHz**, this setup modulates an audio signal onto a PWM-generated carrier, which can be picked up by standard analog radio receivers — particularly older models with wideband AM capability.

The Arduino plays a dual role:
- It uses its **Timer2 hardware peripheral** to synthesize a high-frequency PWM signal (~730–760 kHz), which acts as the **AM carrier wave**.
- Simultaneously, the Arduino samples audio input using its **10-bit ADC** (analog-to-digital converter), and modulates the duty cycle of the carrier in real-time according to the input amplitude — thus implementing **Amplitude Modulation** in software.

The circuit uses just a few passive components:
- **L1 (47 µH)** and **C2 (1 nF)** form a **tank circuit** to filter and tune the PWM output.
- The antenna is driven directly from a digital output pin, typically through a series capacitor to block DC offset.
- A pair of resistors and a capacitor at the analog input side filter and attenuate the audio signal.


### ARDUINO AM SOURCE CODE

```cpp
// Simple AM Radio Signal Generator :: Markus Gritsch
// http://www.youtube.com/watch?v=y1EKyQrFJ-o

#define INPUT_PIN  0  // ADC input pin
#define TIMER_PIN  3  // PWM output pin, OC2B (PD3)
#define DEBUG_PIN  2  // to measure the sampling frequency
#define LED_PIN   13  // displays input overdrive

#define SHIFT_BY   3  // 2 ... 7 input attenuator
#define TIMER_TOP 20  // determines the carrier frequency
#define A_MAX TIMER_TOP / 4

void setup() {
    pinMode(DEBUG_PIN, OUTPUT);
    pinMode(TIMER_PIN, OUTPUT);
    pinMode(LED_PIN, OUTPUT);

    // set ADC prescaler to 16 to decrease conversion time
    ADCSRA = (ADCSRA | _BV(ADPS2)) & ~( _BV(ADPS1) | _BV(ADPS0));

    // configure timer: non-inverting, fast PWM, no prescaling
    TCCR2A = 0b10100011;
    TCCR2B = 0b00001001;

    // 16E6 / (OCR2A + 1) = ~762 kHz at TIMER_TOP = 20
    OCR2A = TIMER_TOP;
    OCR2B = TIMER_TOP / 2; // 50% duty cycle
}

void loop() {
    digitalWrite(DEBUG_PIN, HIGH);
    int8_t value = (analogRead(INPUT_PIN) >> SHIFT_BY) - (1 << (9 - SHIFT_BY));
    digitalWrite(DEBUG_PIN, LOW);

    if (value < -A_MAX) {
        value = -A_MAX;
        digitalWrite(LED_PIN, HIGH);
    } else if (value > A_MAX) {
        value = A_MAX;
        digitalWrite(LED_PIN, HIGH);
    } else {
        digitalWrite(LED_PIN, LOW);
    }

    OCR2B = A_MAX + value;
}
```

## 🧰 Arduino AM Source Code Breakdown

The source code leverages **Timer2 in fast PWM mode**, where `OCR2A` determines the carrier frequency and `OCR2B` the amplitude (duty cycle). The modulation occurs by updating `OCR2B` with values derived from the analog input:

- **Input signal** is sampled with reduced resolution using a bit-shift (`>> SHIFT_BY`) to match the PWM granularity.
- **Clipping logic** ensures that the duty cycle remains within a usable range, preventing distortion or signal overflow.
- A **debug pin** toggles during each read to measure the effective sampling rate (about 34 kHz).

💡 This implementation demonstrates a clever use of limited hardware to emulate AM transmission using nothing more than software PWM and some passive RF filtering.

## 📡 Arduino AM CW (Morse Code Transmission)

The second project transmits **Morse code (CW)** over AM by toggling a digital output pin in software. The transmitter generates bursts of a square wave at approximately **1.3 MHz**, again driving a simple wire antenna.

Key characteristics:
- **DITs and DAHs** (short and long Morse pulses) are defined in terms of "port toggles", allowing precise control over timing using software loops.
- The signal is created by **manually cycling PORTB** through values with tight `while` loops, making it hardware-timed without delay functions.
- A clever use of inline assembly (`asm volatile ("inc %0")`) ensures efficient register manipulation and consistent timing.
- The modulation scheme is essentially **on-off keying (OOK)** — a simple form of AM where the carrier is turned on and off to represent digital data.

The experiment shows that with **just a wire antenna and an Arduino**, one can send intelligible signals up to **several meters away**, depending on antenna length and environmental factors.


```cpp
long millisAtStart = 0;
long millisAtEnd = 0;

const long period_broadcast = 8;
#define LENGTH_DIT 50

const int length_dit = LENGTH_DIT;
const int pause_dit = LENGTH_DIT;
const int length_dah = 3 * LENGTH_DIT;
const int pause_dah = LENGTH_DIT;
const int length_pause = 7 * LENGTH_DIT;

void dit(void);
void dah(void);
void pause(void);
void broadcast(int N_cycles);
void dontbroadcast(int N_cycles);

#define ASM_INC(reg) asm volatile ("inc %0" : "=r" (reg) : "0" (reg))

void setup() {
    Serial.begin(9600);
    DDRB = 0xFF;

    millisAtStart = millis();
    dit();
    millisAtEnd = millis();

    Serial.print(millisAtEnd - millisAtStart);
    Serial.print(" ");
    Serial.print((length_dit + pause_dit) * period_broadcast * 256 / (millisAtEnd - millisAtStart) / 2);
    Serial.print("kHz ");
    Serial.println();
}

void loop() {
    // S
    dit(); char_pause(); dit(); char_pause(); dit(); char_pause();
    // O
    dah(); char_pause(); dah(); char_pause(); dah(); char_pause();
    // S
    dit(); char_pause(); dit(); char_pause(); dit(); char_pause();
}

void dit(void) {
    for (int i = 0; i <= length_dit; i++)
        broadcast(period_broadcast);
}

void dah(void) {
    for (int i = 0; i <= length_dah; i++)
        broadcast(period_broadcast);
}

void char_pause(void) {
    for (int i = 0; i <= length_dit; i++)
        dontbroadcast(period_broadcast);
}

void word_pause(void) {
    for (int i = 0; i <= length_pause; i++)
        dontbroadcast(period_broadcast);
}

void broadcast(int N_cycles) {
    unsigned int portvalue;
    for (int i = 0; i < N_cycles; i++) {
        do {
            PORTB = portvalue;
            ASM_INC(portvalue);
        } while (portvalue < 255);
    }
}

void dontbroadcast(int N_cycles) {
    unsigned int portvalue;
    PORTB = 0x00;
    for (int i = 0; i < N_cycles; i++) {
        do {
            ASM_INC(portvalue);
            asm volatile ("NOP");
        } while (portvalue < 255);
    }
}
```

⚠️ Due to the nature of RF emission, even low-power transmitters like this may **fall under regional radio regulations**. Use only in shielded environments or for near-field demonstrations.

## ⚗️ Galena Crystal Radios

> “Powerless, timeless, and analog perfection.”

The **Galena crystal radio** is a *passive receiver* — it operates entirely without batteries or external power. It relies on the **semiconducting properties of Galena (PbS)** to detect amplitude-modulated (AM) radio signals.

### Why Galena Works:

- Galena acts as a **point-contact diode**, rectifying the radio signal and allowing the audio component to be extracted.
- A **cat’s whisker** (a fine wire) touches the crystal at a sensitive point, forming a crude diode junction.
- The detected audio is passed to a **very high-impedance earphone** (usually >10kΩ) to minimize signal loss.

### Construction Tips:

- The antenna should be **long and elevated** for best reception.
- Coil winding must be tuned to the desired frequency range (typically MW or SW).
- Tuning capacitors can be improvised or scavenged from vintage electronics.

This elegant device demonstrates the essence of radio — a form of communication born of natural materials, signal physics, and nothing more than wire and rock.

