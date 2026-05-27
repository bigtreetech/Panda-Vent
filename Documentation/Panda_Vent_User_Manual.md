# Panda Vent User Manual

> **Firmware Version:** V1.0.0  
> **Updated:** May 27, 2026  
> **Applicable Device:** Panda Vent (ESP32-based platform)  
> **Powered By Bigtreetech — Bifröst Engine**

---

## 📋 Table of Contents

1. [Product Overview](#1-product-overview)
2. [Hardware Overview](#2-hardware-overview)
3. [First Use](#3-first-use)
4. [Wi-Fi Configuration](#4-wi-fi-configuration)
5. [Binding a Bambu Lab Printer](#5-binding-a-bambu-lab-printer)
6. [RGB Lighting Settings](#6-rgb-lighting-settings)
7. [Fan/Motor Control (Button Operation)](#7-fanmotor-control-button-operation)
8. [OTA Firmware Upgrade](#8-ota-firmware-upgrade)
9. [Restore Factory Settings](#9-restore-factory-settings)
10. [FAQ](#10-faq)

---

## 1. Product Overview

**Panda Vent** is a Smart Vent Riser designed for FDM 3D printers. It monitors print status in real time and automatically controls the fan and lighting, making your printer smarter, quieter, and cooler.

##  Key Features

- 🌈 **RGB Status Light:**  
  Uses WS2812 LED strips to reflect printer status in real time, including idle, preparing, printing, paused, completed, and error states — all visible at a glance.

- 🌀 **Smart Fan Control:**  
  Supports **Auto Mode**, which follows printer status automatically, and **Manual Mode**, which allows the fan to be toggled with a button.

- 🌡️ **Improved Heat Dissipation:**  
  Optimizes internal airflow and actively exhausts hot air, effectively helping prevent nozzle clogs caused by excessive chamber temperature and improving stability during long prints.

- 🏗️ **Riser Design:**  
  Raises the top glass or AMS mounting space, significantly reducing friction between the PTFE tube and the glass top cover. This helps minimize tube wear, friction noise, and printing failures.

- 📡 **MQTT Communication:**  
  Communicates with Bambu Lab printers through the local MQTT protocol. No cloud connection is required, enabling fast and reliable response.

- 🌐 **Web Management UI:**  
  Provides a built-in Web UI for configuring Wi-Fi, MQTT, lighting, fan settings, and other functions from any browser.

- 🔄 **OTA Upgrade:**  
  Supports firmware upgrades through the Web UI, making maintenance simple and convenient.

- 🔌 **Automatic LED Strip Detection:**  
  Automatically detects whether 1 LED strip group or 2 LED strip groups are connected:
  - **1 strip:** 16 LEDs
  - **2 strips:** 27 LEDs

  No manual setup is required — plug and play.

---

## Use Cases

Panda Vent is suitable for users who:

- Need better chamber cooling or improved airflow.
- Want to view printer status intuitively through lighting without constantly checking the printer screen.
- Enjoy DIY smart peripherals and want to integrate the device into existing **Home Assistant** or **MQTT** systems.
- Want to optimize PTFE tube routing, reduce friction noise, and minimize tube wear.
---

## 2. Hardware Overview

### Physical Buttons

| Button | Location | Function |
|--------|----------|----------|
| **LED Button** (GPIO 12) | Device panel | Single click: toggle the fan on/off; long press (3 seconds): switch between automatic and manual mode |

| Manual Mode |

- In automatic mode, single-clicking the button switches back to manual mode and immediately reverses the fan blade state.
- In manual mode, single-clicking immediately reverses the fan blade state.

| **BOOT Button** (GPIO 0) | Side of the device | Long press (3 seconds): restore factory settings |

### Indicator Light

| Status | LED Behavior |
|--------|--------------|
| **Automatic Mode** | LED stays off |
| **Manual Mode** | LED flashes at 0.5-second intervals |

### Interfaces and Light Groups

- Supports **1 light strip group** (16 LEDs) or **2 light strip groups** (27 LEDs).
- The device automatically detects the number of connected light groups through ADC voltage. No manual configuration is required.

---

## 3. First Use

### Step 1: Power On

1. Power on Panda Vent. The device will start automatically.
2. On first startup, the device automatically enables the **Wi-Fi hotspot** (AP mode).

### Step 2: Connect to the Device Hotspot

1. Search for Wi-Fi on your phone or computer and find a hotspot starting with `Panda_Vent_`.
2. Enter the default password: **`987654321`**.
3. After connecting successfully, open a browser.

### Step 3: Open the Web Management Interface

1. Enter the following address in the browser address bar: **`192.168.254.1`**.
2. Or, when on the same local network, visit: **`http://PandaVent.local`** (requires the device to already be connected to Wi-Fi).
3. The first page shown is the **language selection page**.

### Step 4: Select a Language

- Supports **English** and **Simplified Chinese**.
- After selecting a language, click **Next** to enter Wi-Fi configuration.

---

## 4. Wi-Fi Configuration

### Connect to Home Wi-Fi (STA Mode)

1. In the Web UI, click the **Wi-Fi** icon in the bottom navigation bar to enter network settings.
2. Click the **Scan** button to search for nearby Wi-Fi networks.
3. Select your Wi-Fi network from the drop-down list.
4. Enter the Wi-Fi password.
5. Click **Connect**.
6. After a successful connection, the page will display the **IP address** assigned to the device.

> ⚠️ **Important:** Panda Vent and the Bambu Lab printer must be connected to the **same local network** to communicate properly.

### Set the Hostname

- The default hostname is `PandaVent`.
- You can customize the hostname. After setting it, you can access the device using `<hostname>.local`.
- After editing, click **Set Hostname** to save the change.

### Hotspot (AP) Settings

| Setting | Default Value | Description |
|---------|---------------|-------------|
| Hotspot Switch | Enabled | Can be manually disabled to save resources |
| Hotspot Name | `Panda_Vent_XXXX` | The suffix is the device MAC address identifier |
| Hotspot Password | `987654321` | It is recommended to change this to a custom password |
| Hotspot IP | `192.168.254.1` | Customizable |

---

## 5. Binding a Bambu Lab Printer

### Preparation

Make sure the following conditions are met:

- Panda Vent is connected to Wi-Fi.
- The Bambu Lab printer is connected to the **same local network**.
- The printer **SN** and **Access Code** are ready.

### Get Printer Information

- **SN:** View it in printer settings → device information.
- **Access Code:** View it in printer settings → LAN mode (8-character code).
- **IP Address:** View it in the printer network settings.

### Binding Steps

1. In the Web UI, click the **Printer** icon in the bottom navigation bar.
2. Click the **Scan** button. The device will automatically discover Bambu Lab printers on the LAN using UDP (SSDP protocol).
3. Select your printer from the drop-down list. The SN and IP will be filled in automatically.
4. Manually enter the printer **Access Code**.
5. Click **Bind**.
6. Wait for the connection. After a successful connection, the UI will display the connection status.

### Connection Status

| Status | Description |
|--------|-------------|
| Connected | MQTT connection successful; working normally |
| Connecting | Attempting to establish MQTT connection |
| IP Error | Incorrect printer IP address |
| SN Error | Printer SN does not match |
| Access Code Error | Incorrect Access Code |

---

## 6. RGB Lighting Settings

Click the **Theme** icon in the bottom navigation bar to enter the lighting configuration page.

### 6.1 Main Light Switch

- **Light Switch:** Main switch for enabling/disabling all RGB lighting effects.
- **Warning Override:** When enabled, warning lighting effects override normal effects when the printer enters a warning state such as high temperature.
- **Follow Printer Mode:** The light effect color and effect automatically follow the current printer status.
- **Follow Ventilation Mode:** The light effect color and effect follow the fan status.
- **Reverse Direction:** Reverses the flowing direction of LED strip effects.

### 6.2 Light Modes

The device provides **three light modes**:

#### Mode 1: Simple Mode

Suitable for users who only want to set one fixed lighting effect.

**Available Effects:**

| Effect | Description |
|--------|-------------|
| Static | Solid light with a fixed color |
| Breathing | Light slowly brightens and dims |
| Strobing | Light flashes quickly |
| Wave | Light flows like a wave |
| Marquee | LEDs light up one by one and move along the strip |
| Color Cycle | Colors automatically cycle |
| Rainbow | Rainbow gradient effect |

**Adjustable Parameters:**

- 🎨 **Color:** Click the color button to open the HSL color picker and freely select a color.
- 💡 **Brightness:** 0% to 100%, in 5% steps.
- ⚡ **Speed:** 0% to 100%, in 5% steps. Some effects do not support speed adjustment.

#### Mode 2: Advanced Mode (Advance Mode / H2D)

Configures separate lighting effects for **each printer working status**.

| Status Button | Corresponding Printer Status | Default Color |
|---------------|-------------------------------|---------------|
| Idle | Printer idle | White (FFFFFF) |
| Preparation | Downloading / slicing preparation | Orange (F8A323) |
| Printing | Printing | Rainbow effect |
| Paused | Print paused | White (FFFFFF) |
| Completed | Print completed | Green (00FF2A) |
| Error | Print error / alarm | Red (FF0000) |

Each status can be configured independently with its own effect type, color, brightness, and speed.

#### Mode 3: Temperature Warning Mode (Warning Hot Mode)

Displays lighting effects based on the printer temperature status, divided into **safe** and **danger** levels:

- 🟢 **Safe Status:** Green lighting effect; Static or Strobing can be selected.
- 🔴 **Danger Status:** Red lighting effect; Static or Strobing can be selected.

Brightness and speed can be set independently for each level.

---

## 7. Fan/Motor Control (Button Operation)

Panda Vent supports two fan operating modes:

### Automatic Mode (AUTO)

- The **default mode**. The device automatically controls the fan according to printer status.
- The LED indicator **stays off**.
- Exhaust ventilation turns on automatically during printing and turns off automatically when idle.

### Manual Mode (MANUAL)

- The LED indicator **flashes**.
- The fan switch is completely controlled manually by the user.

### Button Operation Guide

| Operation | Function |
|----------|----------|
| **LED Button Single Click** (in automatic mode) | Switches to manual mode and toggles the fan state (on ↔ off) |
| **LED Button Single Click** (in manual mode) | Toggles the fan state (on ↔ off) |
| **LED Button Long Press for 3 Seconds** | Switches from manual mode back to automatic mode |
| **BOOT Button Long Press for 3 Seconds** | Restores factory settings (see Section 9) |

> 💡 **Tip:** Mode switching status is automatically saved to NVS (non-volatile storage). After power loss and restart, the previous mode setting will be restored.

### Motor Groups

The device supports 4 motor channel groups (GROUP 1 to 4). Each motor group is equipped with:

- An independent Hall sensor for position detection.
- Forward/reverse PWM control.
- Gradual startup to prevent impact caused by sudden motor startup.

---

## 8. OTA Firmware Upgrade

### Upgrade Steps

1. In the Web UI, click the **Settings** icon in the bottom navigation bar.
2. Check the current firmware version.
3. Click the **Firmware Upgrade** button.
4. Select the new firmware file in `.bin` format. The maximum size is approximately 4.5 MB.
5. Wait for the upload and flashing process to complete. The UI will display progress.
6. After the upgrade is complete, the device restarts automatically.

> ⚠️ **Notes:**
> - **Do not power off or disconnect the network** during the upgrade.
> - During the upgrade, the RGB LED strip will display a white OTA animation.
> - If the upgrade fails, the device will roll back to the previous firmware version.

---

## 9. Restore Factory Settings

### Method 1: Physical Button

- **Long press the BOOT button (GPIO 0) for 3 seconds**. The device will clear all configurations and restart.

### Method 2: Web UI

1. Go to the **Settings** page.
2. Click **Restore Factory Settings**.
3. Click Confirm in the confirmation pop-up.

### Factory Reset Clears the Following Data:

- ✅ Wi-Fi connection information (STA configuration)
- ✅ Printer binding information (SN, Access Code, IP)
- ✅ RGB lighting custom configuration
- ✅ Fan mode preferences
- ✅ Language settings
- ✅ Custom hotspot configuration

> After restoring factory settings, the device will re-enter the initial configuration process.

---

## 10. FAQ

### Q1: What should I do if Panda Vent cannot connect to the printer?

**A:** Check the following:

1. Whether Panda Vent and the printer are on the **same local network**.
2. Whether the printer **LAN mode** is enabled.
3. Whether the **Access Code** is correct. Pay attention to letter case.
4. Whether the printer **IP address** is correct. You can confirm it on the printer side.
5. Try scanning for the printer again in the Web UI.

### Q2: Why does only part of the LED strip light up?

**A:** The device automatically identifies the number of light groups based on ADC detection:

- 1 light strip group = 16 LEDs
- 2 light strip groups = 27 LEDs
- If no light strip is detected (voltage is 0), lighting effects will not be displayed.

Check whether the LED strip connection is secure.

### Q3: Why does the button not respond?

**A:**

- The button has 5 ms debounce processing. Make sure it is pressed properly.
- Long press requires holding for **3 seconds** to trigger.
- Single-click response has a delay of about 300 ms to distinguish between single clicks and multiple clicks.

### Q4: Why I cannot access the Web UI?

**A:**

1. If connected through the hotspot, visit `192.168.254.1`.
2. If connected through the local network, visit `PandaVent.local` or the IP address assigned to the device.
3. Make sure the browser and the device are on the same network.
4. Try clearing the browser cache and retrying.

### Q5: Will the configuration be lost after power-off?

**A:** No. The following configurations are automatically saved to NVS (non-volatile storage):

- Wi-Fi configuration
- Printer binding information
- RGB lighting settings
- Fan mode preferences
- Language settings

All configurations will be restored automatically after power-off and restart.

### Q6: Which Bambu Lab printer models are supported?

**A:** In theory, all Bambu Lab printers with LAN mode enabled are supported, including but not limited to:

- Bambu Lab P1P / P1S
- Bambu Lab X1 / X1 Carbon
- Bambu Lab A1 / A1 Mini

---

## 📝 Web UI Navigation

The bottom navigation bar contains the following five functional areas:

| Icon | Page | Function |
|------|------|----------|
| 🎨 Theme | Theme | RGB lighting configuration |
| ⚙️ Settings | Settings | Language / firmware upgrade / factory reset |
| 🖨️ Printer | Printer | Bambu Lab printer binding and management |
| 📡 Wi-Fi | STA | Wi-Fi connection and hostname settings |
| 📶 Hotspot | AP | Device hotspot configuration |


