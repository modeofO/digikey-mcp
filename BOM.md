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

### Solenoid Drive Circuit

| # | Part | MPN | Manufacturer | Qty | Price | DK Part # | Link |
|---|------|-----|-------------|-----|-------|-----------|------|
| 4 | MOSFET (logic-level) | IRLZ44NPBF | Infineon | 1 | $1.96 | IRLZ44NPBF-ND | [DigiKey](https://www.digikey.com/en/products/detail/infineon-technologies/IRLZ44NPBF/811808) |
| 5 | Flyback diode | 1N4007 | Diotec | 1 | $0.14 | 4878-1N4007CT-ND | [DigiKey](https://www.digikey.com/en/products/detail/diotec-semiconductor/1N4007/18833652) |
| 6 | Solenoid valve (12V, 6.5mm barb) | WATER-VALVE-6.5MM-12VDC | Olimex | 1 | $4.62 | — | [DigiKey](https://www.digikey.com/en/products/detail/olimex-ltd/WATER-VALVE-6-5MM-12VDC/21661974) |

### Passives

| # | Part | MPN | Manufacturer | Qty | Price | DK Part # | Link |
|---|------|-----|-------------|-----|-------|-----------|------|
| 7 | Gate resistor — 220 ohm | CF14JT220R | Stackpole | 2 | $0.10 | CF14JT220RCT-ND | [DigiKey](https://www.digikey.com/en/products/detail/stackpole-electronics-inc/CF14JT220R/1741346) |
| 8 | Gate pull-down — 10K ohm | CFR-25JB-52-10K | YAGEO | 1 | $0.10 | 13-CFR-25JB-52-10K-ND | [DigiKey](https://www.digikey.com/en/products/detail/yageo/CFR-25JB-52-10K/338) |
| 9 | LED resistor — 330 ohm | CFR-25JB-52-330R | YAGEO | 1 | $0.10 | 330QBK-ND | [DigiKey](https://www.digikey.com/en/products/detail/yageo/CFR-25JB-52-330R/1636) |
| 10 | Filter cap — 1uF | K105M20X7RF53H5 | Vishay | 2 | $1.02 | K105M20X7RF53H5-ND | [DigiKey](https://www.digikey.com/en/products/detail/vishay-beyschlag-draloric-bc-components/K105M20X7RF53H5/2820555) |

### Power

| # | Part | MPN | Manufacturer | Qty | Price | DK Part # | Link |
|---|------|-----|-------------|-----|-------|-----------|------|
| 11 | 6V 5W solar panel | FIT0600 | DFRobot | 1 | $17.50 | — | [DigiKey](https://www.digikey.com/en/products/detail/dfrobot/FIT0600/9608221) |
| 12 | 18650 Li-ion cell (3.7V 2.5Ah) | ASR00050 | TinyCircuits | 1 | $6.95 | — | [DigiKey](https://www.digikey.com/en/products/detail/tinycircuits/ASR00050/9808766) |
| 13 | 18650 battery holder (wire leads) | BH-18650-W | MPD | 1 | $3.64 | — | [DigiKey](https://www.digikey.com/en/products/detail/mpd-memory-protection-devices/BH-18650-W/3029217) |
| 14 | LDO 3.3V 250mA (TO-92) | MCP1700-3302E/TO | Microchip | 1 | $0.50 | — | [DigiKey](https://www.digikey.com/en/products/detail/microchip-technology/MCP1700-3302E-TO/652680) |
| 15 | Boost converter (12V 500mA, 5-pack) | DFR0952 | DFRobot | 1 | $4.90 | 1738-DFR0952-ND | [DigiKey](https://www.digikey.com/en/products/detail/dfrobot/DFR0952/18069216) |

### Cost Breakdown

| Line | Calculation |
|------|------------|
| Nucleo-F411RE | 1 x $18.82 = $18.82 |
| Soil sensors | 2 x $5.90 = $11.80 |
| Green LED | 1 x $0.15 = $0.15 |
| IRLZ44NPBF | 1 x $1.96 = $1.96 |
| 1N4007 | 1 x $0.14 = $0.14 |
| Solenoid valve | 1 x $4.62 = $4.62 |
| 220 ohm | 2 x $0.10 = $0.20 |
| 10K ohm | 1 x $0.10 = $0.10 |
| 330 ohm | 1 x $0.10 = $0.10 |
| 1uF cap | 2 x $1.02 = $2.04 |
| Solar panel | 1 x $17.50 = $17.50 |
| 18650 cell | 1 x $6.95 = $6.95 |
| Battery holder | 1 x $3.64 = $3.64 |
| LDO 3.3V | 1 x $0.50 = $0.50 |
| Boost converter (5-pack) | 1 x $4.90 = $4.90 |
| **DigiKey Subtotal** | **$73.42** |

### Packaging Guide

All parts available in single-unit quantities:
- **Bulk** (resistors, caps, LED, Nucleo, battery, holder) — loose in bag, order qty 1–2
- **Cut Tape** (220 ohm, 1N4007) — snipped from tape reel, min qty 1
- **Tube** (IRLZ44N) — anti-static tube, min qty 1
- Avoid **Tape & Reel** and **Tape & Box** — production quantities (1000+)

---

## Amazon / General Supplier Components

| # | Part | Description | Qty | Est. Price | Notes |
|---|------|-------------|-----|------------|-------|
| 16 | TP4056 Li-ion charger module | USB/solar input, charges single Li-ion cell, built-in protection | 1 | ~$2–3 (pack of 5) | Search "TP4056 charging module". Accepts 4.5–8V input. |
| 17 | Full-size breadboard | 830 tie-point, standard | 1 | $5–8 | |
| 18 | Jumper wire kit | Male-male, male-female, female-female assortment | 1 | $6–9 | |

**Amazon Subtotal: ~$15–20**

---

## Total Estimated Cost: ~$88–93

---

## Power Budget

**Boost converter efficiency loss:** The solenoid draws 500mA @ 12V = 6W at the output. At 80% boost efficiency (3.7V → 12V), the battery supplies 7.5W = **2.0A peak from the 18650**. This is a 0.8C rate — within spec for most 18650 cells, with ~0.16V sag at 80mΩ internal resistance.

| Load | Power | Daily duration | Daily Wh (from battery) |
|------|-------|---------------|------------------------|
| Solenoid (via boost, η=80%) | 7.5W from battery | 90 sec | 0.19 Wh |
| MCU active + sensors | 0.33W | ~10 min | 0.055 Wh |
| MCU sleep (STOP mode) | 33µW | ~23.8 hr | 0.001 Wh |
| **Total** | | | **~0.25 Wh/day** |

**Autonomy without sun:** 9.25 Wh / 0.25 Wh = **~37 days**

**Solar recharge:** 5W panel × 6 peak sun hours × ~80% TP4056 efficiency = ~24 Wh/day into battery — 96× daily consumption. Battery stays topped off.

**Boost converter (DFR0952):** Rated 500mA @ 12V output from 2.5V–12V input. Matches solenoid draw at the upper limit. The pack of 5 provides spares. If the solenoid's inrush current exceeds 500mA, add a 1000µF/16V electrolytic across the boost output to buffer the initial surge.

---

## Wiring

### Solar Charging

```
6V Solar Panel (+/-) ──> TP4056 Module (IN+/IN-)
18650 in holder      ──> TP4056 Module (BAT+/BAT-)
TP4056 OUT+          ──> 3.7V rail (feeds LDO and boost converter)
```

The TP4056 handles charging, overcharge protection, and over-discharge protection.

### Nucleo-F411RE GPIO / ADC Mapping

| Function | Pin | Notes |
|----------|-----|-------|
| Soil sensor 1 (analog) | A0 (PA0, ADC1_IN0) | 12-bit ADC, 0–3.3V range |
| Soil sensor 2 (analog) | A1 (PA1, ADC1_IN1) | 12-bit ADC, 0–3.3V range |
| MOSFET gate (solenoid) | D2 (PA10) or any digital GPIO | 3.3V drives IRLZ44N fully on (Vgs(th) max 2V) |
| Status LED | D3 or any digital GPIO | Through 330 ohm resistor |

---

## Spare Parts Worth Having

- Extra 1N4007 diodes (pennies)
- Extra 220 ohm and 10K resistors (buy 5–10 of each, they're $0.10)
- A spare 18650 cell
