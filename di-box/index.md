# Diretta Iniezione


# DI Box

A handcrafted **direct injection box** — a compact, AC-powered **audio preamplifier** designed for dynamic microphones, capable of cleanly converting mic-level signals to standard **line-level output (-10dBV)**.

### Features:
- Class A **FET input stage** with high linearity and extended bandwidth  
- XLR input with adjustable gain  
- Balanced or unbalanced **line-level output**  
- Low noise, low distortion performance  
- Wide frequency response  

---

## 🎙️ Motivation & Design Approach

While trying to record audio using a **Shure SM58** dynamic microphone directly into a Mac’s line-in jack, I encountered the usual issues: low gain, poor signal-to-noise ratio, and uninspiring tone. So, I decided to build my own solution — a clean, simple **preamplifier** with tone characteristics I personally enjoy.

The input stage is modeled after the classic **12AX7 triode front-end** found in vintage Fender amplifiers — but implemented here with a JFET, taking advantage of its **tube-like character**, low-noise performance, and analog simplicity. No USB or digital interference: the unit is **powered by a noiseless internal AC supply**, intentionally avoiding computer-induced hum.

---

## 🔌 Input Stage: FET vs. Triode

In terms of behavior, the **FET booster** circuit is functionally very close to a triode: both operate in **Class A**, both shape their transconductance via negative feedback, and both offer a pleasing analog response curve.

{{< image src="fetzervalve.png" caption="FET booster." >}}

To emulate this sound, I based my design loosely on the well-known [Fetzer Valve](http://runoffgroove.com/fetzervalve.html) preamp, originally conceived for guitar pedals but easily adapted to microphone-level signals.

I tested a few JFETs from my stash — **2SK30** and **2N5457** — and settled on the **2N5457** for its smooth response and availability. The circuit runs at **18V**, giving a large headroom and excellent dynamic range.

---

## 📈 Testing & Waveform Results

Using a -20 dBu sine wave (1 kHz), AC-coupled into the DI box, the output wave looks very stable and distortion-free at full volume.

{{< image src="diretta-iniezione-wave.jpg" caption="Direct injection of a -20 dBu 1kHz wave, output volume at max." >}}

Switching to a live capture using the Shure SM58 in front of the Mac's internal speaker (playing the same tone), we observe minor distortion — expected, given the poor acoustic source and uncontrolled conditions.

{{< image src="diretta-iniezione-wave2.jpg" caption="Captured via Shure SM58 from Mac speaker output, 1kHz tone." >}}

A small **DC offset** was observed during biasing, later traced to a **faulty trimmer** in the bias voltage divider.

Nonetheless, the resulting **voice recordings** are crisp, warm, and have an analog texture that I find very pleasant.

---

## 🎧 Audio Sample

Want to hear how it sounds? Here's a short **voice recording** using this DI box:  
🎙️ [Listen on Vimeo](https://vimeo.com/97137760)

---

## 🔬 Future Experiment: Cascode Configuration

For the next iteration, I’m considering a **cascode configuration** using dual FETs or FET + BJT to further increase gain and linearity.

![](diretta-iniezione-cascode.jpg)

{{< image src="diretta-iniezione-cascode.jpg" caption="Considering future development..." >}}

---

## 🛠️ Final Assembly

{{< image src="diretta-iniezione-01.jpg" caption="The assembled DI Box." >}}

The entire build fits in a compact metal enclosure, with XLR input and standard jack outputs. Simple, effective, analog.

If you’re looking for a reliable mic-to-line converter with vintage character — this little **Diretta Iniezione** box just might do the trick.


