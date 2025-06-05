# DeLorean Time Circuits


# BTTF

This is a semi-serious rendition of the story behind making this project:

A replica of the time machine circuits from the **Back To The Future** blockbuster equipped with a talking alarm clock.

Apart from actual time travel, which might be even possible depending on your time-zone, internet provider and your skills in creating your own [flux-capacitor](https://backtothefuture.fandom.com/wiki/Flux_capacitor), it works as a nice talking alarm clock with the most famous quotes from the original movie.


> If my calculations are correct, when this baby hits 88 miles per hour... you're gonna see some serious shit.
> — <cite>Dr. Emmett Brown[^1]</cite>

[^1]: The remarkable quote is obviously excerpted from Robert Zemeckis's [Back To The Future](https://www.youtube.com/watch?v=PAAkCSZUG1c) Blockbuster.


{{< image src="back-to-the-future-prototype-dark.jpg" caption="Time Circuits Alarm Clock Prototype by Luca Cipressi (2014)" >}}


## The Committent

Once upon every now-and-then comes this pretty eclectic and talented guitarist, with a penchant in collectables, along my road. He approached me from distance and waved.
As he was drawing near I noticed he was holding something in his hand. 

- It was no beer glass (he does not drink).
- It was no cigarette (he does not smoke).
- It was a tiny model car of a DeLorean.

He expressed his desire to add another "Back to the Future" thinghy to his collection, something more lively, something more electric, like his Fender guitars.

- He wanted a talking alarm clock (because he oversleeps).
- He wanted to travel time with it (because he wants to change history).
- He wanted to play with it (all work and no play makes Jack a dull boy).

After thinking a bit, I embarked myself into the task of making him a replica of the beloved **Time Circuits**. He said he would have provided the Flux capacitor.
Spoiler alert: he did.

## Feature List

- three displays with same lettering and coloring
- keyboard based input
- the same sound FX from the movie when configured
- loudly speak out some of the best quotes from the original movie

## Prototype design

It was all about creating one sample: the first and only prototype. No mass-production, no hard-times, no hassles, just fun.

*That ain't working, that's the way to do it!*

The mechanical part needed help from another friend, who offered his skills and his workshop full of metal-working machinery.

### The build size and the displays

When buiding some hardware, the most important thing after all is the final volume in space it is going to occupy.
Being a replica of something already existing should be easy: determining the dimensions from the one visible in the movie inside the DeLorean it is possible to estimate something a bit wider than an autoradio. Surely a number of different mock-ups have been used for the different scenes where the "Time Circuits" appear throughout the movie.
It is worth to notice that the "month" indication in more than one scene in the movie is made by a back-lit plastic cover without actual alphanumeric display beneath it.
In this build, the matching factors were:

- color availability: red, green, orange/yellow
- use of two display types: alphanumeric (14 segments) and numeric (7 segments)
- color consistency across each single row and display type
- lettering size across each row and display type

After long research I ended up selecting the Sunbright series with 0.8" font size. This choice has determined the final build size to be quite bulkier than the original.

A Raspberry Pi model A (first series) has been selected as the main controller, making it easier to handle mp3 audio and a speaker output, as well as having some NTP protocol support.

{{< image src="back-to-the-future-displaybboard-schematic-wm.jpg" caption="The display assembly schematic (c) 2014 Luca Cipressi." >}}


The Display Interface

{{< image src="back-to-the-future-display-test-01.jpg" caption="Testing the multiplexed display interface (c) 2014 Luca Cipressi." >}}

{{< image src="back-to-the-future-display-test-03.jpg" caption="The display uses the multiplexing technique driving 360 bits from the I2C serial line." >}}



## Handcrafted

Authentic craftmanship and lots of patience for this prototype. Designing a custom PCB still would have been the best approach quality-wise with all its advantages, but I do not fear precise hand-soldering and, in fact, completing the operation was much faster than spending lots of hours on Kicad, export the files, send them out to the manufacturer, wait for delivery, finally solder the parts and test. Main reason to stick with the manual soldering, moreover, was to avoid the boring layout routing along those 16 segments displays by using a double layer PCB. Yes, this way was **quicker**!

{{< image src="back-to-the-future-displayboard-03.jpg" caption="The hand wired connections to each display board (c) 2014 Luca Cipressi." >}}

With a hand-driven router/milling machine the slots for the displays have been opened through the aluminium panels.

{{< image src="back-to-the-future-displayboard-04.jpg" caption="Routing the openings (c) 2014 Luca Cipressi." >}}

and cleaned up from the dust and debris. The three display boards have found their placement inside the panels. The driver boards are on the bottom side, not yet placed. The latter are the only actual PCB I designed for this prototype.

{{< image src="back-to-the-future-displayboard-02.jpg" caption="Assembling the display panel (c) 2014 Luca Cipressi." >}}


The final photograph of the completed prototype after setting up the time of the day as it was the 7th of April 2014.

{{< image src="bttfac-bg.jpg" caption="Time Circuits Alarm Clock (c) 2014 Luca Cipressi." >}}


## BOM

The following BOM list prices from 2014, which are in EUR and available in Italy at the time. Shipping costs and I.V.A. (italian GST) is indicated where relevant as well.
Some parts might not be available anymore (long-live the glorious Rpi model A!)

| SUPPLIER                          | DESCRIPTION                            | QTY | PRICE       | TOTAL    |
| ---------------------------------:| -------------------------------------- |:---:| -----------:| --------:|
| MDSRL.it                          |                                        |     |             |          |
|                                   | Double sided PCB     160x50 (10days)   | 2   | € 17,57     | € 35,14  |
|                                   | *Shipping costs*                       |     |             | € 3,30   |
|                                   | *GST*                                  |     |             | € 8,46   |
|                                   |                                        |     | SUBTOTAL    | € 46,90  |
| Distrelec.it                      |                                        |     |             |          |
|                                   | Display PSC08-11GWA (green 16 segs)    | 3   | € 2,10      | € 6,30   |
|                                   | Display PSC08-11EWA (red 16 segs)      | 3   | € 1,35      | € 4,05   |
|                                   | Display SC08-11EWA (red)               | 10  | € 1,40      | € 14,00  |
|                                   | Display SC08-11GWA (green)             | 10  | € 1,40      | € 14,00  |
|                                   | Display PSC08-11YWA (yellow 16 segs)   | 3   | € 1,35      | € 4,05   |
|                                   | Display SC08-11YWA (yellow)            | 10  | € 0,88      | € 8,80   |
|                                   | IC 74HC595D                            | 9   | € 0,32      | € 2,88   |
|                                   | *Shipping costs*                       |     |             | € 10,00  |
|                                   | *GST*                                  |     |             | € 14,59  |
|                                   |                                        |     | SUBTOTAL    | € 78,67  |
| Techstore A.G.                    |                                        |     |             |          |
|                                   | Numeric Keypad 4x4                     | 1   | € 5,00      | € 5,00   |
|                                   | Strip Connector IDC 2x13 pin           | 2   | € 1,90      | € 3,80   |
|                                   | 4 Mini Connectors eq. JST-XH 5-pin     | 1   | € 2,50      | € 2,50   |
|                                   | Kit RTC DS1307 Real Time Clock I2C     | 1   | € 3,90      | € 3,90   |
|                                   | *Shipping costs*                       |     |             | € 4,65   |
|                                   |                                        |     | SUBTOTAL    | € 19,85  |
| E.B.M. Store di Gaetano Filigrana |                                        |     |             |          |
|                                   | breadboard 233x160         single side | 2   | € 6,50      | € 13,00  |
|                                   | DIL18 IC socket (10 pieces per pack)   | 1   | € 1,50      | € 1,50   |
|                                   | PCF8574AP I/O expander i2c 8 bit       | 1   | € 2,30      | € 2,30   |
|                                   | LM386N audio amplifier 1W              | 1   | € 1,20      | € 1,20   |
|                                   | DIL16 IC socket (10 pieces per pack)   | 3   | € 1,50      | € 4,50   |
|                                   | *Shipping costs*                       |     |             | € 6,50   |
|                                   |                                        |     | SUBTOTAL    | € 29,00  |
| enovaz.it                         |                                        |     |             |          |
|                                   | Rasperry Pi 256 MB                     | 1   | € 29,90     | € 29,90  |
|                                   | *GST*                                  |     |             | € 6,58   |
|                                   | *Shipping costs*                       |     |             | € 10,00  |
|                                   |                                        |     | SUBTOTAL    | € 46,48  |
|                                   | Miscellaneous                          |     |             | € 10,00  |
|                                   |                                        |     | GROSS TOTAL | € 230,90 |

## PEOPLE & CREDITS

- Emiliano Sabatini (the committent)
- Luca Cipressi (sw & hw)
- Antonio Di Tonto (metalworks)
- special thanks to Guido Cammisano

# EXTRAS

## Praise of Creativity on TV

Shortly after having made this gadget replica, we were contacted by a TV journalist and interviewed on the theme of "Creativity". The footage below is a summary from the local italian RAI TV news broadcasted on 12th April 2014 on RAI3 TGR Abruzzo for an episode of "Elogio della Creatività" series or, in English "Praise of Creativity".

{{< vimeo 211174500 >}}


