# Solar-Powered STM32 Irrigation Controller — Bill of Materials

*Last updated: 2026-05-13 — prices validated against DigiKey API*

---

## Architecture

```
6V Solar Panel ──> Li-ion Charger (TP4056) ──> 18650 Li-ion Cell (3.7V)
                                                    |
                                      ┌─────────────┴──────────────┐
                                      v                            v
                              LDO (3.3V)                   Boost (12V)
                                  |                            |
                            MCU + Sensors              MOSFET + Solenoid
```

Two power domains from a single 18650 cell:
- **3.3V rail** via LDO — powers MCU and soil moisture sensors
- **12V rail** via boost converter — powers solenoid valve (seconds/day duty cycle)

For bench testing: power MCU via USB, solenoid via 12V wall wart. No solar/battery needed.

---

## DigiKey Components

### MCU & Peripherals

| # | Part | MPN | Manufacturer | Qty | Price | DK Part # | Link |
|---|------|-----|-------------|-----|-------|-----------|------|
| 1 | STM32 Nucleo-64 | NUCLEO-F411RE | STMicroelectronics | 1 | $18.82 | 497-14711-ND | [DigiKey](https://www.digikey.com/en/products/detail/stmicroelectronics/NUCLEO-F411RE/4866485) |
| 2 | Capacitive soil sensor | SEN0193 | DFRobot | 2 | $5.90 | — | [DigiKey](https://www.digikey.com/en/products/detail/dfrobot/SEN0193/6588605) |
| 3 | Status LED (green) | LTL-4231N | Lite-On | 1 | $0.15 | 160-1142-ND | [DigiKey](https://www.digikey.com/en/products/detail/liteon/LTL-4231N/214440) |
| 4 | Low-battery LED (red) | LTL-4224 | Lite-On | 1 | $0.17 | 160-1128-ND | [DigiKey](https://www.digikey.com/en/products/detail/liteon/LTL-4224/217584) |

### Solenoid Drive Circuit

| # | Part | MPN | Manufacturer | Qty | Price | DK Part # | Link |
|---|------|-----|-------------|-----|-------|-----------|------|
| 5 | MOSFET (logic-level) | IRLZ44NPBF | Infineon | 1 | $1.96 | IRLZ44NPBF-ND | [DigiKey](https://www.digikey.com/en/products/detail/infineon-technologies/IRLZ44NPBF/811808) |
| 6 | Flyback diode | 1N4007 | Diotec | 1 | $0.14 | 4878-1N4007CT-ND | [DigiKey](https://www.digikey.com/en/products/detail/diotec-semiconductor/1N4007/18833652) |
| 7 | Solenoid valve (12V, 6.5mm barb) | WATER-VALVE-6.5MM-12VDC | Olimex | 1 | $4.62 | — | [DigiKey](https://www.digikey.com/en/products/detail/olimex-ltd/WATER-VALVE-6-5MM-12VDC/21661974) |

### Passives

| # | Part | MPN | Manufacturer | Qty | Price | DK Part # | Link |
|---|------|-----|-------------|-----|-------|-----------|------|
| 8 | Gate resistor — 220 ohm | CF14JT220R | Stackpole | 1 | $0.10 | CF14JT220RCT-ND | [DigiKey](https://www.digikey.com/en/products/detail/stackpole-electronics-inc/CF14JT220R/1741346) |
| 9 | Gate pull-down — 10K ohm | CFR-25JB-52-10K | YAGEO | 1 | $0.10 | 13-CFR-25JB-52-10K-ND | [DigiKey](https://www.digikey.com/en/products/detail/yageo/CFR-25JB-52-10K/338) |
| 10 | LED resistor — 330 ohm | CFR-25JB-52-330R | YAGEO | 2 | $0.10 | 330QBK-ND | [DigiKey](https://www.digikey.com/en/products/detail/yageo/CFR-25JB-52-330R/1636) |
| 11 | Voltage divider — 100K ohm | CFR-25JB-52-100K | YAGEO | 2 | $0.10 | 13-CFR-25JB-52-100K-ND | [DigiKey](https://www.digikey.com/en/products/detail/yageo/CFR-25JB-52-100K/245) |
| 12 | Filter cap — 1uF ceramic | K105M20X7RF53H5 | Vishay | 2 | $1.02 | K105M20X7RF53H5-ND | [DigiKey](https://www.digikey.com/en/products/detail/vishay-beyschlag-draloric-bc-components/K105M20X7RF53H5/2820555) |

### Power

| # | Part | MPN | Manufacturer | Qty | Price | DK Part # | Link |
|---|------|-----|-------------|-----|-------|-----------|------|
| 13 | 6V 5W solar panel | FIT0600 | DFRobot | 1 | $17.50 | — | [DigiKey](https://www.digikey.com/en/products/detail/dfrobot/FIT0600/9608221) |
| 14 | 18650 Li-ion cell (3.7V 2.5Ah) | ASR00050 | TinyCircuits | 1 | $6.95 | — | [DigiKey](https://www.digikey.com/en/products/detail/tinycircuits/ASR00050/9808766) |
| 15 | 18650 battery holder (wire leads) | BH-18650-W | MPD | 1 | $3.64 | — | [DigiKey](https://www.digikey.com/en/products/detail/mpd-memory-protection-devices/BH-18650-W/3029217) |
| 16 | LDO 3.3V 250mA (TO-92) | MCP1700-3302E/TO | Microchip | 1 | $0.50 | — | [DigiKey](https://www.digikey.com/en/products/detail/microchip-technology/MCP1700-3302E-TO/652680) |
| 17 | Boost converter (12V, adjustable) | DFR0123 | DFRobot | 1 | $7.50 | 1738-1144-ND | [DigiKey](https://www.digikey.com/en/products/detail/dfrobot/DFR0123/6588566) |
| 18 | Inrush buffer cap — 1000uF 16V | 860020375017 | Würth | 1 | $0.43 | 732-8804-1-ND | [DigiKey](https://www.digikey.com/en/products/detail/würth-elektronik/860020375017/5727041) |

### Cost Breakdown

| Line | Calculation |
|------|------------|
| Nucleo-F411RE | 1 x $18.82 = $18.82 |
| Soil sensors | 2 x $5.90 = $11.80 |
| Green LED | 1 x $0.15 = $0.15 |
| Red LED | 1 x $0.17 = $0.17 |
| IRLZ44NPBF | 1 x $1.96 = $1.96 |
| 1N4007 | 1 x $0.14 = $0.14 |
| Solenoid valve | 1 x $4.62 = $4.62 |
| 220 ohm | 1 x $0.10 = $0.10 |
| 10K ohm | 1 x $0.10 = $0.10 |
| 330 ohm | 2 x $0.10 = $0.20 |
| 100K ohm | 2 x $0.10 = $0.20 |
| 1uF ceramic cap | 2 x $1.02 = $2.04 |
| Solar panel | 1 x $17.50 = $17.50 |
| 18650 cell | 1 x $6.95 = $6.95 |
| Battery holder | 1 x $3.64 = $3.64 |
| LDO 3.3V | 1 x $0.50 = $0.50 |
| Boost converter | 1 x $7.50 = $7.50 |
| 1000uF electrolytic cap | 1 x $0.43 = $0.43 |
| **DigiKey Subtotal** | **$76.82** |

### Packaging Guide

All parts available in single-unit quantities:
- **Bulk** (resistors, caps, LED, Nucleo, battery, holder) — loose in bag, order qty 1–2
- **Cut Tape** (220 ohm, 1N4007) — snipped from tape reel, min qty 1
- **Tube** (IRLZ44N) — anti-static tube, min qty 1
- Avoid **Tape & Reel** and **Tape & Box** — production quantities (1000+)

---

## Amazon / General Supplier Components

| # | Part | Description | Qty | Est. Price | Link |
|---|------|-------------|-----|------------|------|
| 19 | TP4056 Li-ion charger module (6-pack) | USB-C input, 5V 1A, charges 18650, built-in protection | 1 | ~$8 | [Amazon](https://www.amazon.com/Charging-Lithium-Battery-Charger-Protection/dp/B08FSRV7GS) |
| 20 | Breadboard kit (4-pack) | 2x 830-point + 2x 400-point solderless breadboards | 1 | ~$8 | [Amazon](https://www.amazon.com/Breadboards-Solderless-Breadboard-Distribution-Connecting/dp/B07DL13RZH) |
| 21 | Jumper wire kit | U-shape breadboard jumpers + flexible DuPont wires (M-M, M-F, F-F) | 1 | ~$10 | [Amazon](https://www.amazon.com/dp/B0BFX5TWZK) |
| 22 | Alligator clip test leads (10-pack) | 10x 20.5" leads, 5 colors, 22AWG soldered clips | 1 | ~$7 | [Amazon](https://www.amazon.com/dp/B06XX25HFX) |

**Amazon Subtotal: ~$33**

---

## Total Estimated Cost: ~$110

---

## Power Budget

**Solenoid:** 540mA @ 12V = 6.5W (confirmed from Olimex specs). At 80% boost efficiency (3.7V → 12V), the battery supplies 8.1W = **2.2A peak from the 18650**. This is a 0.88C rate — within spec for most 18650 cells.

**Boost converter quiescent:** The DFR0123 has no shutdown pin — it draws ~3–5mA from the 3.7V rail whenever connected, even with the solenoid off. This dominates the idle power budget.

| Load | Power | Daily duration | Daily Wh (from battery) |
|------|-------|---------------|------------------------|
| Solenoid (540mA @ 12V, boost η=80%) | 8.1W from battery | 90 sec | 0.20 Wh |
| Boost converter quiescent | ~18mW | ~23.97 hr | 0.44 Wh |
| MCU active + sensors | 0.33W | ~10 min | 0.06 Wh |
| MCU sleep (STOP mode) | 33µW | ~23.8 hr | 0.001 Wh |
| **Total** | | | **~0.70 Wh/day** |

**Autonomy without sun:** 9.25 Wh / 0.70 Wh = **~13 days** (boost always connected)

**Solar recharge:** 5W panel × 6 peak sun hours × ~80% TP4056 efficiency = ~24 Wh/day into battery — 34× daily consumption. Battery stays topped off even with quiescent draw.

**Boost converter (DFR0123):** XL6009-based, rated 15W output (1.25A @ 12V), Vin 3.7–34V. Output voltage set via onboard potentiometer — adjust to 12V with a multimeter before use. The 1000µF/16V electrolytic cap across the boost output buffers solenoid inrush current (~1A for 10–20ms).

---

## Wiring

### Solar Charging

```
6V Solar Panel (+/-) ──> TP4056 Module (IN+/IN-)
18650 in holder      ──> TP4056 Module (BAT+/BAT-)
TP4056 OUT+          ──> 3.7V rail (feeds LDO and boost converter)
```

The TP4056 handles charging, overcharge protection, and over-discharge protection.

### Battery Voltage Monitoring

A resistor divider (2x 100K) on the 3.7V rail feeds an ADC input so firmware can read battery state-of-charge and drive the red low-battery LED.

```
3.7V rail ──> [100K] ──┬──> ADC pin (A2)
                        |
                       [100K]
                        |
                       GND
```

Divider output: Vbat / 2 (4.2V → 2.1V, 3.3V → 1.65V, 2.5V → 1.25V). All within the 3.3V ADC range. Divider draws 21µA — negligible.

**Thresholds (firmware):**
- Vbat < 3.3V → low battery, turn on red LED
- Vbat < 3.0V → critical, skip watering to preserve cell

### Nucleo-F411RE GPIO / ADC Mapping

| Function | Pin | Notes |
|----------|-----|-------|
| Soil sensor 1 (analog) | A0 (PA0, ADC1_IN0) | 12-bit ADC, 0–3.3V range |
| Soil sensor 2 (analog) | A1 (PA1, ADC1_IN1) | 12-bit ADC, 0–3.3V range |
| Battery voltage (analog) | A2 (PA4, ADC1_IN4) | Via 100K/100K divider, reads Vbat/2 |
| MOSFET gate (solenoid) | D2 (PA10) | 3.3V drives IRLZ44N fully on (Vgs(th) max 2V) |
| Status LED (green) | D3 | Through 330 ohm resistor |
| Low-battery LED (red) | D4 | Through 330 ohm resistor |

---

## Watering Failsafe

If a soil sensor breaks (reads "always dry"), firmware could run the solenoid indefinitely — flooding the garden and draining the battery. Three firmware safeguards prevent this:

**1. Max watering duration** — hard cap per activation (e.g., 5 minutes). If soil still reads dry after the limit, stop watering and flag an error (blink red LED).

**2. Sensor sanity check** — reject readings at ADC rail values (0 or 4095). A disconnected or shorted sensor reads at the rail. If either sensor returns a rail value, treat it as a fault — don't water.

**3. Hardware watchdog (IWDG)** — the STM32F411's Independent Watchdog Timer resets the MCU if firmware hangs. On reset, all GPIOs default to floating input mode, and the 10K pull-down on the MOSFET gate turns the solenoid off. This is already built into the circuit — just needs to be enabled in firmware.

Together: a firmware hang kills the solenoid via watchdog reset. A stuck sensor gets caught by the max duration timer and sanity checks. No additional hardware needed.

---

## Spare Parts Worth Having

- Extra 1N4007 diodes (pennies)
- Extra resistors — buy 5–10 of each value, they're $0.10
- A spare 18650 cell
