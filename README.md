# UAV Pitch Attitude Autopilot — PD Control in a PID Framework

## Overview

This project presents the design and validation in simulation of a pitch attitude autopilot for an Unmanned Aerial Vehicle (UAV) using a PD controller implemented within a PID framework. The integral term was evaluated during controller development but set to zero in the final tuned design because acceptable steady-state behaviour was achieved without it.

A complementary filter is used to fuse gyroscope and accelerometer measurements for pitch estimation, while Monte Carlo analysis is used to assess robustness under noisy sensing conditions. This project replicates a typical aerospace control system workflow including modelling, control design, estimation, and validation.

## Key Features

- **PD Controller Design in a PID Framework:** Pitch attitude control designed using proportional and derivative action, with the integral channel disabled in the final tuned controller.
- **Complementary Filter Sensor Fusion:** Combines accelerometer and gyroscope measurements to estimate pitch angle with improved stability and transient behaviour.
- **Monte Carlo Robustness Analysis:** Evaluates closed-loop performance across repeated noisy sensor realizations.
- **Simulation-Based Validation:** Control-system behaviour assessed in Google Colab under actuator constraints, noisy sensing, and disturbance-oriented testing.

## Project Structure

```text
uav-pid-autopilot/
├── UAV_2.ipynb        # Main Jupyter notebook with all code and results
├── README.md          # Project documentation
└── .gitignore         # Python gitignore
```

## Results Summary

The final tuned controller successfully stabilizes UAV pitch attitude to a 10-degree reference angle. The filtered feedback case achieved:

| Metric | Value | Requirement | Status |
|---|---:|---:|---|
| Overshoot | 0.96% | < 5% | PASS |
| Settling Time | 3.44 s | < 5.0 s | PASS |
| Steady-State Error | 0.0016 rad | < 0.01 rad | PASS |
| Rise Time | 2.18 s | < 2.0 s | FAIL |

### Requirement Interpretation

The final filtered feedback case satisfied the overshoot, settling-time, and steady-state-error requirements, but narrowly missed the rise-time requirement. This reflects a deliberate control-design trade-off: reducing rise time further would typically require a more aggressive controller, which would likely increase overshoot and reduce robustness under noisy sensing conditions.

### Feedback Configuration Comparison

| Case | Overshoot (%) | Settling Time (s) | SSE (rad) | Rise Time (s) |
|---|---:|---:|---:|---:|
| Ideal | 1.35 | 3.35 | 2.46e-05 | 2.14 |
| Raw Accelerometer | 4.60 | 8.91 | 1.81e-03 | 1.90 |
| Gyro Only | 0.00 | -- | 1.16e-02 | 2.21 |
| Filtered | 0.96 | 3.44 | 1.62e-03 | 2.18 |

### Monte Carlo Robustness (100 trials)

| Metric | Mean | Std Dev | Min | Max |
|---|---:|---:|---:|---:|
| Overshoot (%) | 0.83 | 0.53 | 0.00 | 2.30 |
| Settling Time (s) | 3.58 | 0.59 | 3.28 | 7.94 |
| SSE (rad) | 0.0012 | 0.0009 | 1.02e-05 | 0.0035 |
| Rise Time (s) | 2.16 | 0.04 | 2.09 | 2.28 |

### Monte Carlo Interpretation

Monte Carlo analysis indicates that average closed-loop performance remains stable under noisy sensing conditions. Although the mean settling time remains moderate, the worst-case settling time reached approximately 7.94 s in high-noise realizations, showing that noise primarily affects transient recovery rather than destabilizing the closed-loop system.

## Tuning Justification

Controller gains were selected through iterative simulation and a proportional-gain sweep. The final tuned controller uses:

- **Kp = 1.2**, **Ki = 0.0**, **Kd = 0.7**, **N_filter = 10.0**

A Kp sweep from 0.8 to 1.6 showed that Kp = 1.2 provides the best trade-off between overshoot, settling time, and rise time while respecting actuator saturation limits (`u_max = 0.5`, `du_max = 1.0`).

### Why Ki = 0 in the Final Controller

The final tuned controller uses Ki = 0.0. This choice was intentional because the plant already contains natural integration from pitch rate to pitch angle, so acceptable steady-state performance could be achieved without adding integral action.

In practice, introducing integral gain increased the risk of slower transients and windup effects under actuator constraints, while the Kp-Kd structure already provided a better trade-off between overshoot, settling time, and steady-state error.

## Engineering Insights

- Accelerometer-only feedback produced faster initial motion but significantly worse noise sensitivity and longer settling.
- Gyro-only estimation reduced short-term noise effects but suffered from drift and steady-state bias.
- The complementary filter provided the best practical balance by combining gyro responsiveness with accelerometer long-term correction.
- The derivative term was important for damping the pitch response and limiting overshoot under noisy feedback.
- Setting Ki = 0 avoided unnecessary integral action because acceptable steady-state behaviour was already achieved without it in the final tuned design.

## Key Design Decisions

1. **Complementary Filter (alpha = 0.99):** Fuses gyroscope and accelerometer data to provide a stable pitch estimate with good transient response.
2. **Derivative Filter (N_filter = 10.0):** Prevents excessive noise amplification in the derivative term.
3. **Anti-Windup Infrastructure:** Conditional integration was included to prevent integral windup during actuator saturation, although the final tuned controller used Ki = 0.0.
4. **Actuator Rate Limiting:** `du_max = 1.0` limits the rate of change of control input for more realistic actuator behaviour.

## Limitations

This project uses a simplified pitch-attitude control model rather than a full nonlinear UAV flight-dynamics model. The study focuses on longitudinal pitch regulation, noisy sensing, actuator constraints, and simulation-based validation.

The current work does not yet include full 6-DOF dynamics, aerodynamic coupling, hardware implementation, or experimental flight validation. The results should therefore be interpreted as a control-design and estimation study rather than a complete flight-certified autopilot.

## Requirements

- Python 3.x
- NumPy
- Matplotlib
- Pandas
- Google Colab (recommended) or any Jupyter environment

## Installation and Usage

1. Open `UAV_2.ipynb` in Google Colab.
2. Upload `UAV_2.ipynb`.
3. Run all cells sequentially.
4. No additional setup is required because the core dependencies are pre-installed in Colab.

## Author

**Saad Wajih**

- GitHub: @SaadWajih99

## License

This project is open-source and available for educational purposes.
