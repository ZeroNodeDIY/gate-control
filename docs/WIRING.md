# Wiring reference

This page is the complete text wiring reference. It matches the current Gate Control firmware exactly.

## ESP32-C3 Super Mini connections

| ESP32-C3 pin | Connected part | Purpose |
|---:|---|---|
| `3V3` | OLED VCC, CC1101 VCC | 3.3 V supply |
| `GND` | OLED GND, CC1101 GND, all button ground contacts | Shared ground |
| `GPIO8` | OLED SDA | I²C data |
| `GPIO9` | OLED SCL | I²C clock |
| `GPIO4` | CC1101 SCK | SPI clock |
| `GPIO5` | CC1101 MISO | SPI data: CC1101 → ESP32 |
| `GPIO6` | CC1101 MOSI | SPI data: ESP32 → CC1101 |
| `GPIO2` | CC1101 CSN / SS | SPI chip select |
| `GPIO20` | CC1101 GDO0 | RAW OOK/ASK timing input/output |
| `GPIO0` | REC button | Record / Settings |
| `GPIO1` | TX button | Transmit / Exit settings |
| `GPIO3` | SLOT button | Select / Clear slot |

## Button wiring

Each button has two terminals:

```text
GPIO ──[ momentary button ]── GND
```

The firmware enables the ESP32 internal pull-up for each button. Therefore a released button reads `HIGH` and a pressed button reads `LOW`.

## Power and antenna notes

- Supply the OLED and CC1101 from **3.3 V**, never from the ESP32 board's 5 V pin.
- Connect all GND pins together before applying power.
- Choose a CC1101 antenna for the profile being used: 315, 433.92 or 868.35 MHz.
- The supported profiles describe CC1101 tuning for the current OOK/ASK firmware; they are not a guarantee of compatibility with every remote in that band.
