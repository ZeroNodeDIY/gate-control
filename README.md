# 🚪 Gate Control

> **ESP32-C3 Super Mini + CC1101 — compact Sub-GHz controller for your own compatible static OOK/ASK remotes.**

<p align="center">
  <img alt="ESP32-C3" src="https://img.shields.io/badge/MCU-ESP32--C3-00979D?logo=espressif&logoColor=white" />
  <img alt="CC1101" src="https://img.shields.io/badge/RF-CC1101-4B7BEC" />
  <img alt="Bands" src="https://img.shields.io/badge/Profiles-315%20%7C%20433.920%20%7C%20868.350%20MHz-7B61FF" />
  <img alt="Storage" src="https://img.shields.io/badge/Memory-20%20slots-F39C12" />
</p>

Gate Control is a DIY controller built around an **ESP32-C3 Super Mini**, a **CC1101** transceiver and a **128×64 I²C OLED**. It captures timing of compatible static OOK/ASK transmissions, stores them in ESP32 non-volatile memory and can replay the selected saved signal.

The project is designed as a clear, small hardware build: three buttons, an on-device OLED interface, and no mobile app or external server required.

> [!WARNING]
> Use this project only with equipment that you own or are expressly authorized to test. It is not a tool for bypassing access-control systems. Gate Control does **not** decrypt protected remotes, recover cryptographic keys, defeat rolling codes, or provide interference/jamming functions. Local radio regulations and the rules for the applicable frequency band still apply.

## ✨ Features

- 📥 **RAW OOK/ASK capture** through a GPIO interrupt, preserving pulse timing;
- 📤 **RAW replay** of the saved timing sequence through the CC1101;
- 💾 **20 independent slots** stored in ESP32 NVS, retained after power-off;
- 🔁 capture of up to three repeated packets and selection of the closest matching one;
- 🧹 filtering of very short noise pulses before saving;
- 📡 radio profiles for **315.000**, **433.920** and **868.350 MHz**;
- 📶 passive RSSI frequency scan to help choose one of those profiles;
- 🔂 selectable transmit repeat count: **1–5**;
- 🖥️ OLED status screens for slot, frequency, recording progress, RSSI and result;
- 🗑️ clearing of an individual saved slot.

## ⚙️ Hardware

| Part | Quantity | Notes |
|---|---:|---|
| ESP32-C3 Super Mini | 1 | Main controller |
| CC1101 module | 1 | Use an antenna matched to the selected band |
| SSD1306 OLED, 128×64, I²C | 1 | Firmware uses I²C address `0x3C` |
| Momentary push buttons | 3 | REC, TX and SLOT |
| 3.3 V power source | 1 | **Do not power CC1101 from 5 V** |

## 🔌 Wiring

This table is checked against the firmware pin definitions. All modules share a common ground. The buttons use `INPUT_PULLUP`: connect one side of each button to its GPIO and the other side to **GND** — no external pull-up resistors are needed.

| Device | Signal | ESP32-C3 Super Mini |
|---|---|---:|
| OLED SSD1306 | VCC | 3V3 |
| OLED SSD1306 | GND | GND |
| OLED SSD1306 | SDA | GPIO8 |
| OLED SSD1306 | SCL | GPIO9 |
| CC1101 | VCC | 3V3 |
| CC1101 | GND | GND |
| CC1101 | SCK | GPIO4 |
| CC1101 | MISO | GPIO5 |
| CC1101 | MOSI | GPIO6 |
| CC1101 | CSN / SS | GPIO2 |
| CC1101 | GDO0 | GPIO20 |
| REC button | Signal | GPIO0 |
| TX button | Signal | GPIO1 |
| SLOT button | Signal | GPIO3 |

> [!IMPORTANT]
> The antenna is not “universal”: match it to the frequency band you use. A profile selects the CC1101 tuning; it does not make an incompatible antenna or modulation work.

## 🎛️ Controls

| Button | Short press | Long press (≥1.2 s) |
|---|---|---|
| `REC` — GPIO0 | Record to the current slot | Open settings |
| `TX` — GPIO1 | Transmit the current slot | — |
| `SLOT` — GPIO3 | Select next slot | Clear current slot |

During Settings: `SLOT` selects the next item, `REC` changes or starts the selected item, and `TX` exits to the home screen.

## 🧠 How it works

Many simple fixed-code remotes transmit the same OOK/ASK pulse pattern several times while their button is held. During recording, Gate Control measures transitions from the CC1101 `GDO0` output using an ESP32 GPIO interrupt. It keeps the pulse durations, filters obvious glitches and compares repeated frames before saving one RAW timing sequence.

For transmission, the saved pulse durations are replayed in their original order. This is intentionally RAW replay, rather than a claim that the device has identified every radio protocol.

### Compatibility limits

Successful recording is not automatically proof that a remote can be duplicated:

- **Static OOK/ASK remotes:** may be compatible when the frequency, modulation and timing match.
- **Rolling-code / encrypted remotes:** capturing a transmission does not provide a valid future authorization code; replay is normally rejected by the receiver.
- **Other modulation types (for example FSK):** are outside the current RAW OOK/ASK capture path.

## 📁 Repository layout

```text
gate-control/
├── README.md
├── docs/
│   └── WIRING.md
└── …
```

Firmware source and a compiled release image will be added in a later update.

## 🛠️ Firmware requirements

When source is published, it is intended for **Arduino IDE** with board profile **ESP32C3 Dev Module** and these libraries:

- `Adafruit GFX Library`
- `Adafruit SSD1306`
- `ELECHOUSE CC1101 SRC DRV`

## 📜 License

No license has been selected yet. Until one is added, this repository does not grant permission to reuse, modify or redistribute its contents.

---

Built by [@ZeroNodeDIY](https://github.com/ZeroNodeDIY) · ESP32-C3 · CC1101 · OLED
