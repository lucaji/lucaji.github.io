# Bass Guitar preamps comparison: Bartolini MK-1 vs. MarkBass MB-1


{{< figure src="ArtisanB4FLPlusAS.png" title="Artisan B4 with Markbass MB-1 preamp" >}}  
{{< figure src="B5ElementOPTRfull.png" title="Element B5 with Bartolini MK-1 preamp" >}}

This article explores and compares two preamps commonly found in mid-tier factory bass guitars: the **Bartolini MK-1** and the **Markbass MB-1**. These two systems—despite being similarly specced in terms of controls and layout—differ substantially in design philosophy, implementation, and performance. The goal is to provide insight into their internal architecture and practical implications for musicians and tinkerers.

## Instrument Specifications

| SPECIFICATIONS   | B5 Element                | B4FL+AS                          |
| ---------------- | ------------------------- | -------------------------------- |
| Construction     | Bolt-on                   | Bolt-on                          |
| Body             | Mahogany (Ash top)        | Swamp Ash                        |
| Neck             | 5pcs Walnut & Panga Panga | Maple/Wenge, 1F 20.5mm, 12F 22mm |
| Fretboard        | Roasted Maple             | Panga Panga                      |
| Fretboard Radius | 15.75" (400mm)           | n.a.                             |
| Frets            | 24                        | Fretless, 2 octaves              |
| Scale            | 34" (864mm)              | 34" (864mm)                     |
| Tuners           | Hipshot® Ultralite        | Hipshot® Ultralite               |
| Bridge           | MetalCraft M5 - 18mm      | EB12(4)                          |
| Pickups          | Bartolini® MK-1 set       | Bartolini® MK1-4/F & MK1-4/R     |
| Electronics      | Bartolini MK-1            | Markbass MB-1                    |
| Hardware Color   | Black                     | Black                            |
| Strings          | D'Addario® EXL170-SSL     | D'Addario® EXL165 (105–045)      |

**Shared Features**:
- Master volume potentiometer  
- Pickup balancer (bridge/neck)  
- Three-band EQ (active mode)  
- Passive/active switch  
- 9V battery power supply

{{< admonition type=tip >}}
**Note**: On the Bartolini MK-1, the switch *only* bypasses the EQ section — the circuit still requires a battery to function.
{{< /admonition >}}

## Overview of the Preamp Architectures

| Function               | Markbass MB-1       | Bartolini MK-1        |
|------------------------|---------------------|------------------------|
| Switch function        | Full passive mode   | EQ bypass only         |
| Battery-less operation | Yes                 | No                     |
| Tone control gains     | TBD                 | TBD                    |
| Noise figure           | TBD                 | TBD                    |

While both preamps share identical pinouts and footprint (e.g., interchangeable within similar bass bodies), their internal circuitry reveals a stark contrast in engineering choices.

## Bartolini MK-1 Reverse Engineering

{{< image src="bartolini-mk1-01.jpg" caption="Component side of the Bartolini MK-1 PCB." >}}

The MK-1’s circuit is composed entirely of **surface-mount components** on a silkscreen-free PCB. A polarity protection diode is soldered directly across the battery terminals, and all signal processing is handled by **three TL062 dual op-amps** (JRC).

Two **tantalum capacitors** act as low-leakage filters, stabilizing the **V/2 bias voltage** derived from the 9V power supply.

{{< image src="bartolini-mk1-03.jpg" caption="Power supply stage. Battery ground is activated by cable insertion, not shown here." >}}

### MK-1 Signal Path and Schematic

{{< image src="bartolini-mk1-02.jpg" caption="Wiring of pickups and balancer dual-potentiometer to the PCB connector." >}}

The input stage consists of passive pickups routed through a **balancer potentiometer** and passed to a unity-gain op-amp stage. The initial preamp output is then routed via the switch to either:

- The output jack (EQ bypassed), or  
- The three-band EQ section.

{{< image src="bartolini-mk1-04.jpg" caption="Input buffer stage. Unity gain is set via 3.9k resistors." >}}

The input coupling capacitors are 100nF, forming a **high-pass filter**. Substituting these with 1uF+ electrolytics would extend the bass range but may increase susceptibility to microphonic or subsonic noise (e.g., from finger taps).

{{< image src="bartolini-mk1-05.jpg" caption="EQ stage after the bypass switch. One op-amp section remains unused with floating pins." >}}

An important note: one op-amp remains entirely unconnected, with **floating inputs**, which is generally considered poor practice. Connecting the non-inverting input to ground and configuring it as a buffer (unity gain) would reduce potential noise and mitigate risk of RF interference or parasitic oscillation.

## Markbass MB-1 Reverse Engineering

{{< image src="mb1-comp-side.png" caption="Markbass MB-1 component side PCB." >}}

(*Section under construction — the following will be added soon:*)

- Full schematic diagram  
- Component analysis and values  
- Tone control architecture  
- Comparison of output impedance and gain stages

## Conclusion

This side-by-side comparison of the MK-1 and MB-1 reveals a shared design context (modern active bass electronics with 3-band EQ and pickup balance), but divergent internal strategies.

The **Bartolini MK-1** is minimal and efficient but lacks a true passive mode and contains some questionable design choices like unused op-amps.  
The **Markbass MB-1** appears to offer a more flexible and serviceable approach, with passive fallback — but a detailed teardown will confirm or refute this in the upcoming update.

