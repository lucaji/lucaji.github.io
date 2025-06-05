# Data Logger for Electrical Power Measurements


# Foreword

Measuring power (consumption or supplied) in certain electric (and electronic) appliances or projects sometimes requires very long running times. Some might even last for days or more.
Having a software tool that can reliabily take care of storing the measured data and plot some nice graphs and even export the whole data-set is a great deal of help. Especially when such software can be customized to fulfill any kind of requirements.

I have written this utility which interfaces with a hardware power measurement tool: the GPM8310 made by GW-Instek.
It relies on an Telnet client which periodically queries the GPM to get the exact integrated values of power.

{{< image src="gpm8310.jpg" caption="GW-Instek Power Meter GPM8310 front panel." >}}


If you have a GPM8310 laying around, you can try-out this software utility called [GPMLink](https://github.com/lucaji/GPMLink) whose source code is available at the link.

A complete build is not provided yet, but shall be pretty easy to build it yourself using Visual Studio Community or any other IDE.

# GPMLink

 GW-Instek Power Meter Logging Utility

 GW-Instek GPM8310 AC/DC Power Meter data logger utility.

This project includes a .NET Winforms and a WPF application (and .NETCore work in progress) to perform long measurements and data logging sessions with the above power meter device.

## The Power Meter

GW Instek GPM-8310 is a digital power meter for single-phase (1P/2W) AC power measurement. Features include DC, 0.1Hz~100kHz test bandwidth, 16bits A/D, and 300 kHz sampling rate. It adopts 5” TFT LCD screen with a five-digit measurement display and provides 25 power measurement related parameters, and has a high-precision measurement capability. It also features the ability to display waveform (voltage/current/power), the integration measurement function, harmonic measurement and analysis of each order, external sensor input terminals, and various communication interfaces, etc., to help users achieve clear, convenient and accurate power measurements. This power meter is a most cost-effective power meter with most complete functionalities among the products of the same category.

[Product Page](https://www.gwinstek.com/en-global/products/detail/GPM-8310)

[Specifications](https://www.gwinstek.com/en-global/products/downloadSeriesSpec/2114)

## Connection and speed

The GPM-8310 has RS232C, USB port, Ethernet and GBIP ports. This program relies on its Ethernet port and implements a simple Telnet class to remote control and retrieve the measurements data out of the GPM via SCPI commands.

The data requests are polled at intervals from half a second to two seconds intervals.

This "slow" speed or interval lag has nothing to do with the accuracy of the measurements: the device itself samples and computes data at very high rates and make it available through an internal Integrator that is accessible via proper SCPI commands, delivering precise estimation for the requested electrical magnitudes (voltage, current, power, energy, phase, etc.)

## Gallery

{{< gallery match="gpmlink*" sortOrder="desc" rowHeight="150" margins="5" thumbnailResizeOptions="600x600 q90 Lanczos" showExif=true previewType="blur" embedPreview=true loadJQuery=true >}}

