---
author: Tarun Kujur
operator: Tarun Kujur
---

# HWStats

**HWStats is a lightweight, high-performance utility that reads
real-time PC hardware statistics (CPU, GPU, RAM, Network) and transmits
them via Serial Port (USB) to external microcontrollers.**

**It is designed specifically for users building custom external
displays using hardware like ESP32, Arduino, or STM32.**

## ⚠️ CRITICAL: READ BEFORE INSTALLING

### 🔑 False Positive Warning (Windows Defender)

**You may receive a virus warning when downloading or running this
application.**

- **Status: False Positive**

- **Reason: This application is \"unsigned\" (does not have a purchased
  digital certificate) and accesses low-level system drivers to read
  hardware temperatures and voltage. Heuristic scanners (like Windows
  Defender) often flag this behavior in unknown apps.**

- **Solution: You must Exclude the application folder in Windows
  Security (Instructions below).**

### 🔑 Administrator Rights

**This application requires Administrator Privileges to operate.**

- **Reason: Windows restricts access to hardware sensors (CPU Temp, Fan
  Speeds) and COM ports for standard users. Without Admin rights, the
  app will return 0 values or fail to open.**

## 🔌 Hardware Requirements

**To use this software effectively, you need:**

- **PC OS: Windows 10 or Windows 11 (64-bit).**

- **Microcontroller: ESP32, ESP8266, Arduino, or similar.**

- **Connection: USB Data Cable (Ensure it is not a \"Charge Only\"
  cable).**

- **Drivers: You must have the correct USB-to-UART drivers installed for
  your board:**

  - ***For ESP32/NodeMCU:* \[CP210x Drivers\] or \[CH340 Drivers\].**

## 🚀 Features

- **Real-Time Monitoring: Fetches CPU Load, Temperature, GPU usage, RAM
  utilization, and Network Upload/Download speeds.**

- **Serial Communication: Sends formatted data strings (JSON or CSV) to
  your connected microcontroller.**

- **Low Resource Usage: Optimized to run in the background with minimal
  impact on gaming performance.**

- **Auto-Connect: Automatically attempts to reconnect to the selected
  COM port if the device is unplugged and replugged.**

- **Configurable Refresh Rate: Adjust how often data is sent (e.g.,
  100ms, 500ms, 1s) to match your display\'s refresh capabilities.**

## ⚙️ Installation & Setup

### 1. Download & Extract

**Download the latest `.zip` from the Releases tab. Extract it to a
permanent location (e.g., `C:``\``Tools``\``HWStats.exe`).**

### 2. Whitelist in Windows Defender

**Do this BEFORE running the app to prevent the `.exe` from being
deleted.**

1.  **Open Windows Security \> Virus & threat protection.**

2.  **Click Manage settings.**

3.  **Scroll to Exclusions \> Add or remove exclusions.**

4.  **Click + Add an exclusion \> Folder.**

5.  **Select the folder where you extracted the app.**

### 3. Run the Application

1.  **Right-click `HWStats``.exe`.**

2.  **Select Run as Administrator.**

3.  ***(**Recommended**)* Right-click \> Properties \> Compatibility \>
    Check \"Run this program as an administrator\" so it always works.**

## 📡 Usage Guide

1.  **Connect your Device: Plug your microcontroller into the USB
    port.**

2.  **Select Port: In the app, select the correct COM port (e.g.,
    `COM3`) from the dropdown.**

3.  **Set Baud Rate: Match the Baud Rate in the app (e.g., `115200`) to
    the `Serial.begin(115200)` in your microcontroller firmware.**

4.  **Click Start: The app will begin streaming data.**

**Example Data Format sent to device:**

**Plaintext**
**A34|B23|C45|D34   and so on**

A = CPU TEMP

34 = CURRENT CPU TEMP

B = CPU LOAD

23 = CURRENT CPU LOAD

C = GPU TEMP

45 = CURRENT GPU TEMP

D = GPU LOAD

34 = CURRENT GPU LOAD

| → VALUE SEPARATOR

## 📡 Serial Communication Protocol

**To minimize latency and optimize buffer usage on the microcontroller,
this application uses a compact serialization format. Instead of sending
full string labels (e.g., \"CPU_TEMP\"), data is encoded using
single-character identifiers.**

### Data Packet Structure

**The data payload consists of Key-Value pairs separated by a pipe
delimiter (`|`).**

**Format: `[ID][Value]|[ID][Value]|[ID][Value]|`**

### 🔑 Identifier Mapping Table

<table>
<tbody>
<tr class="odd">
<td><p><strong>Identifier (ID)</strong></p></td>
<td><p><strong>Parameter</strong></p></td>
<td><p><strong>Unit</strong></p></td>
<td><p><strong>Description</strong></p></td>
<td><p><strong>Example</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>*</strong></p></td>
<td><p><strong>SPECIAL_SYMBOL</strong></p></td>
<td><p><strong>NA</strong></p></td>
<td><p><strong>Special symbol attached at the beginning of config file if sent by clicking (flash to ESP32) button.</strong></p></td>
<td><p><strong>*</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>~</strong></p></td>
<td><p><strong>SPECIAL_SYMBOL</strong></p></td>
<td><p><strong>NA</strong></p></td>
<td><p><strong>Special symbol attached at the beginning of data string and sends which mode is selected.</strong></p></td>
<td><p><strong>~0</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>A</strong></p></td>
<td><p><strong>CPU_TEMP</strong></p></td>
<td><p><strong>°C</strong></p></td>
<td><p><strong>CPU Package Temperature</strong></p></td>
<td><p><strong>A50</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>B</strong></p></td>
<td><p><strong>CPU_LOAD</strong></p></td>
<td><p><strong>%</strong></p></td>
<td><p><strong>CPU Total Utilization</strong></p></td>
<td><p><strong>B23</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>C</strong></p></td>
<td><p><strong>GPU_TEMP</strong></p></td>
<td><p><strong>°C</strong></p></td>
<td><p><strong>GPU Core Temperature</strong></p></td>
<td><p><strong>C33</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>D</strong></p></td>
<td><p><strong>GPU_LOAD</strong></p></td>
<td><p><strong>%</strong></p></td>
<td><p><strong>GPU Core Utilization</strong></p></td>
<td><p><strong>D45</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>E</strong></p></td>
<td><p><strong>RAM_USAGE</strong></p></td>
<td><p><strong>GB</strong></p></td>
<td><p><strong>RAM Usage</strong></p></td>
<td><p><strong>E3.5</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>F</strong></p></td>
<td><p><strong>RAM_AVAIL</strong></p></td>
<td><p><strong>GB</strong></p></td>
<td><p><strong>Total usable RAM</strong></p></td>
<td><p><strong>F16</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>G</strong></p></td>
<td><p><strong>RAM_LOAD</strong></p></td>
<td><p><strong>%</strong></p></td>
<td><p><strong>RAM Load</strong></p></td>
<td><p><strong>G45</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>H</strong></p></td>
<td><p><strong>CPU_PWR</strong></p></td>
<td><p><strong>W</strong></p></td>
<td><p><strong>CPU Package Power</strong></p></td>
<td><p><strong>H34</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>I</strong></p></td>
<td><p><strong>CPU_CLK</strong></p></td>
<td><p><strong>MHZ</strong></p></td>
<td><p><strong>CPU Clock</strong></p></td>
<td><p><strong>I2556</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>J</strong></p></td>
<td><p><strong>GPU_PWR</strong></p></td>
<td><p><strong>W</strong></p></td>
<td><p><strong>GPU Package Power</strong></p></td>
<td><p><strong>J25</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>K</strong></p></td>
<td><p><strong>GPU_CLK</strong></p></td>
<td><p><strong>MHZ</strong></p></td>
<td><p><strong>GPU Core Clock</strong></p></td>
<td><p><strong>K1666</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>L</strong></p></td>
<td><p><strong>VRAM_TOTAL</strong></p></td>
<td><p><strong>GB</strong></p></td>
<td><p><strong>Total VRAM Available</strong></p></td>
<td><p><strong>L8</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>M</strong></p></td>
<td><p><strong>VRAM_USED</strong></p></td>
<td><p><strong>GB</strong></p></td>
<td><p><strong>Total VRAM Used</strong></p></td>
<td><p><strong>M1.2</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>N</strong></p></td>
<td><p><strong>VRAM_LOAD</strong></p></td>
<td><p><strong>%</strong></p></td>
<td><p><strong>VRAM Load</strong></p></td>
<td><p><strong>N12</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>O</strong></p></td>
<td><p><strong>CURRENT_FPS</strong></p></td>
<td><p><strong>INT</strong></p></td>
<td><p><strong>FPS</strong></p></td>
<td><p><strong>O120</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>P</strong></p></td>
<td><p><strong>AVG_FPS</strong></p></td>
<td><p><strong>INT</strong></p></td>
<td><p><strong>Average FPS</strong></p></td>
<td><p><strong>P100</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>Q</strong></p></td>
<td><p><strong>FRAMETIME</strong></p></td>
<td><p><strong>FLOAT</strong></p></td>
<td><p><strong>Frame Time</strong></p></td>
<td><p><strong>Q2.46</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>R</strong></p></td>
<td><p><strong>MASTER_VOLUME</strong></p></td>
<td><p><strong>%</strong></p></td>
<td><p><strong>Volume Load</strong></p></td>
<td><p><strong>R23</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>S</strong></p></td>
<td><p><strong>DOWN_SPEED</strong></p></td>
<td><p><strong>Mb/s</strong></p></td>
<td><p><strong>Download Speed</strong></p></td>
<td><p><strong>S3.3</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>T</strong></p></td>
<td><p><strong>UP_SPEED</strong></p></td>
<td><p><strong>Mb/s</strong></p></td>
<td><p><strong>Upload Speed</strong></p></td>
<td><p><strong>T9.0</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>U</strong></p></td>
<td><p><strong>VIRTUAL_MEM_USED</strong></p></td>
<td><p><strong>GB</strong></p></td>
<td><p><strong>Total Virtual Memory Used</strong></p></td>
<td><p><strong>U7.5</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>V</strong></p></td>
<td><p><strong>VIRTUAL_MEM_AVAIL</strong></p></td>
<td><p><strong>GB</strong></p></td>
<td><p><strong>Total Virtual Memory Available</strong></p></td>
<td><p><strong>V8.5</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>W</strong></p></td>
<td><p><strong>VIRTUAL_MEM_LOAD</strong></p></td>
<td><p><strong>%</strong></p></td>
<td><p><strong>Total Virtual Memory Load</strong></p></td>
<td><p><strong>W46</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>X</strong></p></td>
<td><p><strong>VRM_TEMP</strong></p></td>
<td><p><strong>°C</strong></p></td>
<td><p><strong>VRM Temperature</strong></p></td>
<td><p><strong>X34</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>Y</strong></p></td>
<td><p><strong>CMOS_VOLTAGE</strong></p></td>
<td><p><strong>V</strong></p></td>
<td><p><strong>CMOS Battery Voltage</strong></p></td>
<td><p><strong>Y3.07</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>Z</strong></p></td>
<td><p><strong>CPU_FAN_SPEED</strong></p></td>
<td><p><strong>RPM</strong></p></td>
<td><p><strong>CPU Fan Speed</strong></p></td>
<td><p><strong>Z1022</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>a</strong></p></td>
<td><p><strong>CPU_FAN_LOAD</strong></p></td>
<td><p><strong>%</strong></p></td>
<td><p><strong>CPU Fan/AIO Fan Load</strong></p></td>
<td><p><strong>a43</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>b</strong></p></td>
<td><p><strong>GPU_FAN_SPEED</strong></p></td>
<td><p><strong>RPM</strong></p></td>
<td><p><strong>GPU Fan Speed (Primary)</strong></p></td>
<td><p><strong>b1200</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>c</strong></p></td>
<td><p><strong>GPU_FAN_LOAD</strong></p></td>
<td><p><strong>%</strong></p></td>
<td><p><strong>GPU Fan Load</strong></p></td>
<td><p><strong>c40</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>d</strong></p></td>
<td><p><strong>CPU_PUMP_SPEED</strong></p></td>
<td><p><strong>RPM</strong></p></td>
<td><p><strong>AIO Pump Speed</strong></p></td>
<td><p><strong>d2000</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>e(i)</strong></p></td>
<td><p><strong>DRIVE(NUMBER)_TEMP</strong></p></td>
<td><p><strong>°C</strong></p></td>
<td><p><strong>e(0) = 1st Drive Temp, e(1) = 2nd Drive Temp.</strong></p></td>
<td><p><strong>e(0)35</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>f(i)</strong></p></td>
<td><p><strong>DRIVE(NUMBER)_USED</strong></p></td>
<td><p><strong>%</strong></p></td>
<td><p><strong>f(0) = 1st Drive Used Space, f(1) = 2nd Drive Used.</strong></p></td>
<td><p><strong>f(0)93</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>@</strong></p></td>
<td><p><strong>TIME</strong></p></td>
<td><p><strong>HH:MM:TT</strong></p></td>
<td><p><strong>Current Time in PC</strong></p></td>
<td><p><strong>@10:37 AM</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>%</strong></p></td>
<td><p><strong>DATE</strong></p></td>
<td><p><strong>Date</strong></p></td>
<td><p><strong>Current Date in PC</strong></p></td>
<td><p><strong>%Wed 25.12.2025</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>$</strong></p></td>
<td><p><strong>CPU_NAME</strong></p></td>
<td><p><strong>STRING</strong></p></td>
<td><p><strong>Complete CPU Name</strong></p></td>
<td><p><strong>$12th gen Intel Core i5</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>&</strong></p></td>
<td><p><strong>GPU_NAME</strong></p></td>
<td><p><strong>STRING</strong></p></td>
<td><p><strong>Complete GPU Name</strong></p></td>
<td><p><strong>&Nvidia Geforce RTX 3060 Ti</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>^</strong></p></td>
<td><p><strong>GAME_NAME</strong></p></td>
<td><p><strong>STRING</strong></p></td>
<td><p><strong>Current DirectX Game Name</strong></p></td>
<td><p><strong>^The Last of Us</strong></p></td>
</tr>
<tr class="odd">
<td><p><strong>#</strong></p></td>
<td><p><strong>CUSTOM_TEXT</strong></p></td>
<td><p><strong>STRING</strong></p></td>
<td><p><strong>Custom text sent via serial port</strong></p></td>
<td><p><strong>#Hello World</strong></p></td>
</tr>
</tbody>
</table>

### 📝 Decoding Example

**Raw Serial String:**

**Plaintext**

**`A50|B45|C33|D34|``    AND SO ON`**

**Interpretation:**

1.  **Segment 1: `A50`**

    - **ID: `A -> `CPU Temperature**

    - **Value: `50` -\> 50°C**

<!-- -->

1.  **Segment 2: `B45`**

    - **ID: `B` -\> CPU Load**

    - **Value: `45` -\> 45%**

<!-- -->

1.  **Segment 3: `C33`**

    - **ID: `C` -\> GPU Temperature**

    - **Value: `33` -\> 33°C**

<!-- -->

1.  **Segment 4: `D34`**

    - **ID: `D` -\> GPU Load**

    - **Value: `34` -\> 34%**

### 💻 Parsing Logic (Pseudo-code)

**When writing firmware for your microcontroller (ESP32/Arduino), use
the following logic to parse the string:**

1.  **Read the incoming String until the newline character.**

2.  **Split the String by the delimiter `|`.**

3.  **For each substring:**

    - **Char at Index 0 is the ID (e.g., \'A\').**

    - **Substring from Index 1 to end is the Value (e.g., \"50\").**

## 🛠️ Troubleshooting

|                                   |                                                                                           |
|-----------------------------------|-------------------------------------------------------------------------------------------|
| **Issue**                         | **Solution**                                                                              |
|                                   |                                                                                           |
| **App closes immediately**        | **You likely didn\'t run as Administrator, or the config file is missing. Run as Admin.** |
| **\"Access Denied\" on COM Port** | **Close any other software (like Arduino IDE serial monitor) using that port.**           |
| **Zero values (0°C / 0%)**        | **The app lacks permission to read sensors. Restart as Administrator.**                   |
| **Windows SmartScreen Popup**     | **Click \"More Info\" -\> \"Run Anyway\".**                                               |
|                                   |                                                                                           |

## 📄 License

**This project is licensed under the MIT License - feel free to modify
and distribute.**

### A Note to Users

***I am an independent developer building this tool for the community. I
cannot afford the expensive code-signing certificates that big companies
use, which is why the antivirus warning appears. The code is open
source**---**feel free to inspect it if you have concerns!***
