# Data Logger for Electrical Power Measurements


# Foreword

Measuring electrical power—whether consumed or supplied—over long durations can be essential for analyzing appliance behavior, validating designs, or performing reliability testing. Some measurement sessions may extend over hours or even days. In such scenarios, having a reliable software tool that can log data continuously, plot graphs, and export datasets becomes invaluable.

To support this need, I developed a utility named **GPMLink**, which interfaces with the **GW-Instek GPM8310** power meter. The software uses a Telnet-based interface to communicate with the device and periodically queries it using SCPI commands to retrieve integrated power metrics.

{{< image src="gpm8310.jpg" caption="GW-Instek Power Meter GPM8310 front panel." >}}

If you own a GPM8310, you can try out [GPMLink](https://github.com/lucaji/GPMLink). The source code is openly available. Although prebuilt binaries are not provided yet, compiling it is straightforward using Visual Studio Community or any other compatible IDE.

# GPMLink

**GPMLink** is a .NET-based utility that supports both **WinForms** and **WPF** frontends (with a .NET Core version under development). It allows users to perform extended power measurements and log them to CSV files with adjustable polling intervals.

## The Power Meter: GW-Instek GPM-8310

The **GPM-8310** is a precision digital power meter designed for single-phase (1P/2W) AC and DC measurements. Key specifications include:

- Bandwidth: DC, 0.1 Hz–100 kHz
- Sampling Rate: 300 kHz
- ADC Resolution: 16 bits
- Display: 5” TFT LCD (five-digit display)
- Measurement Parameters: 25+ (voltage, current, power, energy, phase, etc.)
- Features: waveform display, harmonic analysis, integration measurements, external sensor support

This model strikes a solid balance between performance and cost, offering high measurement precision and flexible interface options.

[Product Page →](https://www.gwinstek.com/en-global/products/detail/GPM-8310)  
[Datasheet / Specifications →](https://www.gwinstek.com/en-global/products/downloadSeriesSpec/2114)

## Communication and Polling Rate

The GPM-8310 provides several communication interfaces: **RS232C**, **USB**, **Ethernet**, and **GPIB**. **GPMLink** relies on the **Ethernet** port and communicates over **Telnet** using **SCPI (Standard Commands for Programmable Instruments)** to control the device and fetch measurements.

The logging interval can be configured between **0.5 seconds to 2 seconds**. While this may seem slow, it's important to note that the GPM samples data internally at much higher rates. It computes averages or integrated values over time, which are retrieved through SCPI queries. These include:

- Instantaneous and average voltage/current
- Active, apparent, and reactive power
- Energy totals (Wh, Ah)
- Frequency and phase angle
- Harmonics and THD (Total Harmonic Distortion)

This architecture ensures high precision even with infrequent polling, making it ideal for long-term logging.

## Gallery

{{< gallery match="gpmlink*" sortOrder="desc" rowHeight="150" margins="5" thumbnailResizeOptions="600x600 q90 Lanczos" showExif=true previewType="blur" embedPreview=true loadJQuery=true >}}

---

Whether you're monitoring power consumption in a lab, validating the behavior of an embedded device under load, or just trying to keep track of energy usage in real-time, **GPMLink** offers a customizable and transparent solution.


## SCPI Command Reference

The GW-Instek GPM-8310 supports **SCPI** (Standard Commands for Programmable Instruments), a standardized command language widely used for controlling test and measurement equipment. **GPMLink** communicates via Telnet by sending SCPI queries and parsing the responses.

Below is a brief overview of commonly used SCPI commands in the application:

| Command | Description |
|--------|-------------|
| `*IDN?` | Queries the identity string of the instrument (manufacturer, model, firmware). |
| `MEAS:VOLT?` | Measures the RMS voltage of the current channel. |
| `MEAS:CURR?` | Measures the RMS current. |
| `MEAS:POW?` | Retrieves the active power (Watts). |
| `MEAS:ENER?` | Retrieves the accumulated energy (Wh or Ah). |
| `SYST:ERR?` | Returns system error status. |
| `FUNCtion:IMMediate:POWer?` | Reads instantaneous power measurement. |
| `MEAS:ALL?` | Custom compound command to retrieve all configured measurements in a single call (used for efficient polling). |

These commands are periodically issued by the logger to build a continuous dataset, which is stored in CSV format for later analysis.

For a full list of supported SCPI commands, refer to the official GPM-8310 [programming manual](https://www.gwinstek.com/en-global/products/downloadSeriesManual/2114).

## Sample CSV Log Output

GPMLink continuously polls the power meter and appends each measurement to a CSV file. Each row represents one snapshot in time, including a timestamp and key electrical parameters.

Here's a short example of a CSV output:

```csv
Timestamp, Voltage (V), Current (A), Power (W), Energy (Wh), Power Factor
2024-12-01 14:00:00, 230.12, 0.545, 125.67, 10.35, 0.98
2024-12-01 14:00:01, 229.95, 0.548, 126.12, 10.37, 0.97
2024-12-01 14:00:02, 230.08, 0.550, 126.56, 10.39, 0.98
...



## SCPI Parsing Logic

Once a SCPI command is sent via Telnet, the GPM-8310 replies with a plain text string, typically terminated by a newline (`\n`). The structure of the reply depends on the command.

### Example 1 – Single Value

```
> MEAS:VOLT?
< 230.12
```

This can be parsed directly as a floating-point number.

### Example 2 – Compound Measurement

```
> MEAS:ALL?
< 230.12,0.545,125.67,10.35,0.98
```

When multiple values are returned, they are comma-separated. In this case:

- `230.12` → Voltage (V)
- `0.545` → Current (A)
- `125.67` → Active Power (W)
- `10.35` → Energy (Wh)
- `0.98` → Power Factor

These values are split by comma and mapped to their corresponding headers in the CSV file.

### Python-style Pseudocode

```python
response = "230.12,0.545,125.67,10.35,0.98"
fields = response.strip().split(",")
voltage = float(fields[0])
current = float(fields[1])
power = float(fields[2])
energy = float(fields[3])
pf = float(fields[4])
```

Error handling is also implemented: if the device returns an unexpected value or times out, the software skips that row and logs the issue separately, ensuring data integrity over long logging sessions.

