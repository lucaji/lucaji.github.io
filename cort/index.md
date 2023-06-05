# Bass preamps: Bartolini MK-1 vs. MarkBass MB-1


# MK-1 vs. MB-1

I compared the inner electronics of two bass guitars made by [Cort](https://www.cortguitars.com) from the Artisan series: a B5 Element and a B4 Plus AS (Fretless).

The 5-stringer is equipped with Bartolini MK-1 active electronics, while the fretless sports the MarkBass MB-1.

| SPECIFICATIONS   | B5 Element                | B4FL+AS                          |
| ---------------- | ------------------------- | -------------------------------- |
| Construction     | Bolt-on                   | Bolt-on                          |
| Body             | Mahogany (Ash top)        | Swamp Ash                        |
| Neck             | 5pcs Walnut & Panga Panga | Maple/Wenge, 1F 20.5mm, 12F 22mm |
| Fretboard        | Roasted Maple             | Panga Panga                      |
| Fretboard Radius | 15.75" (400mm)            | n.a.                             |
| Frets            | 24                        | fretless 2 octaves               |
| Scale            | 34" (864mm)               | 34" (864mm)                      |
| Tuners           | Hipshot(r) Ultralite      | Hipshot(r) Ultralite             |
| Bridge           | MetalCraft M5 - 18mm      | EB12(4)                          |
| Pickups          | Bartolini(r) MK-1 set     | Bartolini(r) MK1-4/F & MK1-4/R   |
| Electronics      | Bartolini MK-1            | Markbass MB-1                    |
| Hardware Color   | Black                     | Black                            |
| Strings          | D'Addario(r) EXL170-SSL   | D'Addario(r) EXL165 (105-045)    |

Both guitars share the following characteristics too:

- a master volume poti;
- a passive/active switch;
- pick-up balancer poti (bridge/neck);
- three band EQ for the active mode;
- a single 9V power supply.


In this article you will find:

- reverse-engineer the Bartolini MK-1
- reverse-engineer the MarkBass MB-1
- schematics for both electronics with component values
- point out at the differences.

Both electronics share the same connectors and pinouts and the same size. But the internals are very different.

## Bartolini MK-1 Reverse Engineering

{{< image src="bartolini-mk1-01.jpg" caption="The component side of the Bartolini MK-1 PCB." >}}


The safety diode is soldered directly across the battery pins to provide protection against polarity inversion. The rest of the components are SMD and find their place on a silkscreen-free PCB which exposes only the solder pads to the components placement.
Three double operational amplifiers ICs are present, very common TL062s made by JRC.

Two tantalium capacitors (the yellow ones) provide stable bias voltage (9V/2) as visible from the supply stage schematic below.

{{< image src="bartolini-mk1-03.jpg" caption="The power supply stage with the bias voltage V/2. The battery ground conection is actually closed when the cable jack is inserted, and it is not reflected in the schematic." >}}


### MK-1 Schematic


{{< image src="bartolini-mk1-02.jpg" caption="Wiring of pickups and balancer double-potentiomer to the connector. The potentiometer's stripes are actually counter-wired against the rotating axis, as one decreases, the other increases its resistance." >}}

{{< image src="bartolini-mk1-04.jpg" caption="The very first input stage from the pickups and balancer potentiometer. The pre-out wire brings this initial signal, with unity gain, to the switch." >}}


The schematic section above clarifies both why it is not possible to play without battery nor exists a passive mode. The gain is set to 1 by the 3.9k resistors on both channels. A 100n decoupling capacitor between the pickups and the op-amp might be replaced with a 1uF or more for extended bass-range as it act as a high pass filter. The factory value might actually help in rejecting very low frequencies like microphonics due to finger taps onto the strings.

{{< image src="bartolini-mk1-05.jpg" caption="The EQ stage is connected after the switch. The whole circuit uses 5 of the 6 op-amps available, leaving the latter disconnected with its pin floating." >}}


The signal from the input stage "pre-out" is commuted either to the output jack or fed inside the EQ circuit. Here again a 100n capacitor provides AC coupling. If you feel like experimenting, a 1uF can be installed.
For some reason, the unused op-amp has been left with its inputs floating. IMHO it would be best to tie the non-inverting input to ground and connect the inverting input as buffer follower to its output to lower the noise-floor by some dB. This would mitigate unwanted noises to pollute the adiacent op-amp and reduce the risk of self-oscillating phenomena in presence of strong radiations especially RF/EMF.

## MarkBass MB-1 Reverse Engineering

{{< image src="mb1-comp-side.png" caption="Markbass MB-1 component side PCB." >}}

work in progress...

### MB-1 Schematic

will be published soon...

# Comparison table


{{< figure src="ArtisanB4FLPlusAS.png" title="Artisan B4 with Markbass MB-1 preamp" >}}

{{< figure src="B5ElementOPTRfull.png" title="Element B5 with Bartolini MK-1 preamp" >}}


| Function               | Markbass MB-1 | Bartolini MK-1 |
| ---------------------- | ------------- | -------------- |
| Switch function        | passive mode  | EQ bypass only |
| Battery-less operation | possible      | battery needed |
| Tone controls gains    | TBD           | TBD            | 
| Noise figure           | TBD           | TBD            |


