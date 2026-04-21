# Motion Alert System
### Finals Quiz Equivalent — Activity Documentation

---

## What It's For

This project is a **wearable inactivity alert system** designed to detect whether a person is walking or moving. It uses an accelerometer to monitor physical motion and warns the user (via a red LED) if they have been inactive for more than 4 seconds. It is useful for scenarios like:

- Reminding elderly or sedentary individuals to keep moving
- A basic step/activity monitor prototype
- A safety device that alerts if a person has stopped moving unexpectedly

---

## How It Works

### Components Used
| Component | Role |
|---|---|
| Arduino (Uno/Nano) | Main microcontroller |
| MPU9250 (Accelerometer) | Detects physical motion |
| Yellow LED (Pin 8) | Indicates active movement |
| Red LED (Pin 9) | Warns of inactivity |
| Push Button (Pin 2) | Activates/deactivates the system |

---

### System Logic

The system runs in a **button-gated loop**:

1. **Button Held DOWN** → System is active and monitors motion.
2. **Button Released** → All LEDs turn off and states reset.

#### Motion Detection
- The MPU9250 reads acceleration on the X, Y, and Z axes every loop cycle.
- A **magnitude** is computed:

  ```
  magnitude = √(accelX² + accelY² + accelZ²)
  ```

- If `magnitude > 1.05` → Movement is detected → **Yellow LED turns ON**
- If `magnitude < 0.95` → No movement → **Yellow LED turns OFF**

#### Inactivity Warning
- A timestamp (`lastMoveTime`) is updated every time motion is detected.
- If **4 seconds pass** with no detected motion:
  - **Red LED turns ON**
  - A serial warning is printed: `⚠ WARNING: THERE IS NO MOTION DETECTED!`
- Once motion resumes, the red LED turns off and the warning resets.

---

### State Flow

```
[Button Pressed]
      │
      ▼
[Read Accelerometer] ──► magnitude > 1.05 ──► Yellow LED ON, reset timer
      │
      ▼ magnitude < 0.95
[Yellow LED OFF]
      │
      ▼ 4 seconds pass with no motion
[Red LED ON + Serial Warning]
      │
      ▼ Motion detected again
[Red LED OFF, warning reset]
```

---

## Pin Summary

```
Pin 8  → Yellow LED (Movement Indicator)
Pin 9  → Red LED    (Inactivity Warning)
Pin 2  → Push Button (System Enable, external pull-up)
SDA/SCL → MPU9250 via I2C (Wire library)
```

---

## Libraries Required

- `Wire.h` — I2C communication
- `MPU9250_asukiaaa.h` — MPU9250 accelerometer driver
- `math.h` — For `sqrt()` in magnitude calculation

---

## Notes / Possible Improvements

- Thresholds (`highThreshold`, `lowThreshold`) can be tuned depending on sensor sensitivity and use case.
- The 4-second inactivity delay (`notWalkingDelay`) is adjustable.
- A buzzer could replace or supplement the red LED for a more noticeable alert.
- Future version could log step counts or transmit data via Bluetooth.

---

*Documented by: [Your Name]*  
*Date: April 22, 2026*
