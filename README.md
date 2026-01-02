# Pump-Gated Gyro Control

This project implements a real-time, embedded state-estimation and control system using an Arduino microcontroller, an IMU (GY-521 / MPU-6050), and multi-axis servo actuation. The system is designed to operate under noisy measurements and dynamic instability, using sensor fusion and probabilistic state inference to maintain orientation and control in real time.

The core motivation is to study **control under uncertainty** in a fully observable, hardware-constrained environment. Gyroscope and accelerometer data are treated as a time-domain data stream, continuously integrated to estimate system state and drive closed-loop actuation. The design philosophy mirrors approaches used in time-domain astronomy and observational inference: sparse measurements, non-ideal sensors, and feedback decisions made without hindsight.

The system is being extended to interface with external event streams (e.g. Pump.fun API), allowing physical control logic and statistical inference to coexist with on-chain data in a unified real-time pipeline.

---

## Hardware Overview

### IMU: GY-521 (MPU-6050)

I2C communication is used for low-latency sensor access.

**Wiring**

* `SCL` → `A5`
* `SDA` → `A4`
* `VCC` → `5V`
* `GND` → `GND`

The IMU provides 3-axis gyroscope and accelerometer data, which are fused to estimate orientation and angular velocity.

---

### Servo Actuation

Four independent servo channels are used for multi-axis control.

**Pin Configuration**

```cpp
int servoPin1 = 9;
int servoPin2 = 10;
int servoPin3 = 11;
int servoPin4 = 12;
```

Servos receive control signals derived from the inferred system state. Actuation logic is intentionally conservative and bounded to prevent instability amplification.

---

## System Architecture

1. **Sensor Acquisition**
   Raw IMU data is sampled via I2C at a fixed cadence.

2. **State Estimation**
   Orientation and angular velocity are inferred using sensor fusion techniques (e.g. complementary or Kalman-style filtering). The system explicitly models noise rather than smoothing it away.

3. **Control Loop**
   Estimated state is mapped to servo outputs using feedback control laws designed for stability, not optimality.

4. **Time-Domain Logic**
   All decisions are made causally. No buffering, no post-processing, no future information.

5. **External Data Integration (Planned)**
   On-chain events are treated as an additional observational stream, gated through confidence thresholds before influencing system behavior.

---

## Design Principles

* **Inference over prediction**
* **Causality over reactivity**
* **Hardware constraints as first-class citizens**
* **Noise is signal until proven otherwise**

This is not a demo project. It is a controlled experiment in real-time inference, feedback, and stability, with all assumptions exposed at the hardware level.

---

## Status

Active development. Core control loop operational.