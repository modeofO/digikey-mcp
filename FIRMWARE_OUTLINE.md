# Irrigation Controller — Firmware Outline

Target: STM32F411RE (Nucleo-64)

---

## Peripherals Used

| Peripheral | Purpose |
|------------|---------|
| ADC1_IN0 (A0) | Soil sensor 1 |
| ADC1_IN1 (A1) | Soil sensor 2 |
| ADC1_IN4 (A2) | Battery voltage (via 100K/100K divider, reads Vbat/2) |
| GPIO D2 (PA10) | MOSFET gate — solenoid on/off |
| GPIO D3 | Green LED — heartbeat / watering active |
| GPIO D4 | Red LED — low battery / fault |
| RTC | Wake-from-sleep alarm, daily cycle counter reset |
| IWDG | Independent watchdog — resets MCU if firmware hangs |

---

## Configuration Constants

```
SOIL_DRY_THRESHOLD       = 2000    // ADC counts — below this, soil needs water
SOIL_WET_THRESHOLD       = 2800    // ADC counts — above this, soil is wet enough
SENSOR_FAULT_LOW         = 50      // near 0 = sensor shorted
SENSOR_FAULT_HIGH        = 4045    // near 4095 = sensor disconnected
MAX_WATERING_SEC         = 300     // 5-minute hard cap per activation
CHECK_INTERVAL_MIN       = 15      // how often to wake and check sensors
MAX_DAILY_CYCLES         = 6       // max watering activations per day
BATTERY_LOW_MV           = 3300    // red LED on
BATTERY_CRITICAL_MV      = 3000    // skip watering entirely
WATCHDOG_TIMEOUT_SEC     = 30      // IWDG reset if not petted
```

Sensor thresholds will need calibration with actual soil — these are starting values.

---

## System States

```
SLEEPING        — MCU in STOP mode, RTC alarm pending
CHECKING        — awake, reading sensors and battery
WATERING        — solenoid open, monitoring soil + duration
FAULT           — sensor error or max cycles exceeded, red LED blinking
LOW_BATTERY     — red LED solid, watering skipped if critical
```

---

## Boot Sequence

```
1. Initialize clocks, GPIO, ADC, RTC, IWDG
2. Configure GPIO:
     D2 (solenoid gate) → output, drive LOW
     D3 (green LED)     → output, drive LOW
     D4 (red LED)       → output, drive LOW
     A0, A1, A2         → analog input
3. Enable IWDG (30-second timeout)
4. If RTC not already configured:
     Set RTC to a default time
     Reset daily_cycles counter
5. Blink green LED 3x (boot indicator)
6. Enter main loop
```

The solenoid gate is driven LOW immediately at boot — combined with the 10K hardware pull-down, this guarantees the solenoid is off after any reset (including watchdog reset).

---

## Main Loop

```
loop:
    pet_watchdog()

    // --- Read battery ---
    vbat = read_battery_mv()

    if vbat < BATTERY_CRITICAL_MV:
        set_red_led(SOLID)
        set_green_led(OFF)
        goto sleep

    if vbat < BATTERY_LOW_MV:
        set_red_led(SOLID)
    else:
        set_red_led(OFF)

    // --- Read sensors ---
    s1 = read_adc(SOIL_SENSOR_1)
    s2 = read_adc(SOIL_SENSOR_2)

    if is_sensor_fault(s1) or is_sensor_fault(s2):
        set_red_led(FAST_BLINK)
        set_green_led(OFF)
        goto sleep

    // --- Decide whether to water ---
    soil_dry = (s1 < SOIL_DRY_THRESHOLD) or (s2 < SOIL_DRY_THRESHOLD)

    if soil_dry and (daily_cycles < MAX_DAILY_CYCLES) and (vbat >= BATTERY_CRITICAL_MV):
        water()
        daily_cycles += 1

    // --- Heartbeat ---
    pulse_green_led()          // single short blink = "I'm alive"

sleep:
    pet_watchdog()
    configure_rtc_alarm(CHECK_INTERVAL_MIN)
    enter_stop_mode()          // ~10µA until RTC alarm fires
    // ... MCU wakes here ...

    if rtc_midnight_crossed():
        daily_cycles = 0       // reset daily counter
```

---

## Watering Routine

```
fn water():
    start_time = now()
    set_green_led(SOLID)       // solid green = solenoid is open

    solenoid_on()              // drive D2 HIGH

    loop:
        pet_watchdog()
        delay(5_seconds)

        elapsed = now() - start_time
        if elapsed >= MAX_WATERING_SEC:
            // failsafe: hit max duration, flag it
            set_red_led(FAST_BLINK)
            break

        s1 = read_adc(SOIL_SENSOR_1)
        s2 = read_adc(SOIL_SENSOR_2)

        if is_sensor_fault(s1) or is_sensor_fault(s2):
            // sensor died mid-watering — stop immediately
            set_red_led(FAST_BLINK)
            break

        soil_wet = (s1 > SOIL_WET_THRESHOLD) and (s2 > SOIL_WET_THRESHOLD)
        if soil_wet:
            break              // soil is saturated, done

    solenoid_off()             // drive D2 LOW
    set_green_led(OFF)
```

The solenoid_off() call is outside the loop — every exit path (normal, timeout, fault) hits it. There is no code path where the solenoid stays on after water() returns.

---

## Helper Functions

```
fn read_battery_mv() -> u16:
    raw = read_adc(BATTERY_ADC)          // 0–4095
    vdivider_mv = raw * 3300 / 4096      // voltage at divider midpoint
    return vdivider_mv * 2               // undo the 1:2 divider

fn is_sensor_fault(reading: u16) -> bool:
    return reading < SENSOR_FAULT_LOW or reading > SENSOR_FAULT_HIGH

fn solenoid_on():
    gpio_set(D2, HIGH)

fn solenoid_off():
    gpio_set(D2, LOW)

fn pet_watchdog():
    iwdg_reload()
```

---

## LED Behavior Summary

| System state | Green LED | Red LED |
|-------------|-----------|---------|
| Sleeping (healthy) | Off | Off |
| Waking to check | Single short pulse | Off |
| Watering active | Solid on | Off |
| Low battery (still watering) | Normal | Solid on |
| Critical battery (no watering) | Off | Solid on |
| Sensor fault | Off | Fast blink (~2Hz) |
| Max duration failsafe hit | Off | Fast blink (~2Hz) |

---

## Interrupt / Wake Sources

| Source | Purpose |
|--------|---------|
| RTC alarm | Wake from STOP mode every 15 minutes |
| IWDG timeout | Reset MCU if firmware hangs (solenoid turns off via hardware pull-down) |

No other interrupts needed. The system is polling-based during the brief awake window.

---

## Power Modes

| Mode | Duration | Current draw |
|------|----------|-------------|
| STOP mode | ~14 min 55 sec per cycle | ~10µA (MCU) + ~5mA (boost quiescent) |
| Active (checking) | ~1–2 sec | ~50mA (MCU + ADC) |
| Watering | up to 5 min | ~50mA (MCU) + 2.2A (boost driving solenoid) |

The boost converter quiescent draw (~5mA) dominates idle power consumption. If battery life becomes a concern, add a high-side P-channel MOSFET between the 3.7V rail and the boost converter input, controlled by a spare GPIO. The MCU would power up the boost only during watering.

---

## Implementation Notes

**Sensor calibration:** The ADC thresholds above are placeholders. To calibrate:
1. Read sensor in completely dry soil → that's your "dry" baseline
2. Read sensor in freshly watered soil → that's your "wet" baseline
3. Set SOIL_DRY_THRESHOLD ~20% above dry baseline
4. Set SOIL_WET_THRESHOLD ~20% below wet baseline

**Two-sensor logic:** The outline waters if EITHER sensor reads dry (OR logic), and stops when BOTH read wet (AND logic). This means: any dry zone triggers watering, but watering continues until all zones are wet. Adjust if your two sensors cover independent zones.

**RTC accuracy:** The Nucleo-F411RE's LSE crystal position (X2) may or may not be populated. If not, the RTC runs from the internal RC oscillator, which drifts ~1–2% (up to 30 min/day). For soil-moisture-triggered watering this doesn't matter — you're checking conditions, not scheduling by clock.

**Watchdog and sleep:** The IWDG runs from an independent 32kHz RC oscillator. Its timeout (30s) must be longer than any single operation but shorter than a runaway watering cycle. The main loop pets it every iteration. During STOP mode, the IWDG keeps running — configure the RTC alarm to wake BEFORE the watchdog expires, or freeze the watchdog during STOP (set DBGMCU_APB1FZ.DBG_IWDG_STOP).

**Language:** This outline is language-agnostic. For Rust/Embassy, the main loop becomes an async task with `Timer::after()` for delays and embassy-stm32 HAL for peripherals. For C, use STM32 HAL or LL drivers with `HAL_PWR_EnterSTOPMode()`.
