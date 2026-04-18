# UAV PID Autopilot

## Overview

This project implements a **pitch attitude autopilot** for an Unmanned Aerial Vehicle (UAV) using PID control. The system features complementary filter sensor fusion and Monte Carlo robustness analysis to evaluate controller performance under uncertain conditions.

## Key Features

- **PID Controller Design**: Full PID-based pitch attitude control with tuned gains for stable flight response.
- **Complementary Filter Sensor Fusion**: Combines accelerometer and gyroscope data to estimate pitch angle accurately.
- **Monte Carlo Robustness Analysis**: Tests controller stability and performance across a range of uncertain system parameters.
- **Simulation-Based Validation**: All algorithms implemented and validated in Python using Google Colab.

## Project Structure

```
uav-pid-autopilot/
|-- UAV_2.ipynb      # Main Jupyter notebook with all code and results
|-- README.md        # Project documentation
`-- .gitignore       # Python .gitignore
```

## Results Summary

The PID controller successfully stabilizes UAV pitch attitude to a 10-degree reference angle. The final filtered feedback case achieved:

| Metric | Value | Requirement | Status |
|--------|-------|-------------|--------|
| Overshoot | 0.96% | < 5% | PASS |
| Settling Time | 3.44 s | < 5.0 s | PASS |
| Steady-State Error | 0.0016 rad | < 0.01 rad | PASS |
| Rise Time | 2.18 s | < 2.0 s | FAIL |

### Feedback Configuration Comparison

| Case | Overshoot (%) | Settling Time (s) | SSE (rad) | Rise Time (s) |
|------|---------------|-------------------|-----------|---------------|
| Ideal | 1.35 | 3.35 | 2.46e-05 | 2.14 |
| Raw Accelerometer | 4.60 | 8.91 | 1.81e-03 | 1.90 |
| Gyro Only | 0.00 | -- | 1.16e-02 | 2.21 |
| Filtered | 0.96 | 3.44 | 1.62e-03 | 2.18 |

### Monte Carlo Robustness (100 trials)

| Metric | Mean | Std Dev | Min | Max |
|--------|------|---------|-----|-----|
| Overshoot (%) | 0.83 | 0.53 | 0.00 | 2.30 |
| Settling Time (s) | 3.58 | 0.59 | 3.28 | 7.94 |
| SSE (rad) | 0.0012 | 0.0009 | 1.02e-05 | 0.0035 |
| Rise Time (s) | 2.16 | 0.04 | 2.09 | 2.28 |

## Tuning Justification

Controller gains were selected through iterative simulation and a proportional-gain sweep. The final tuned controller uses:

- **Kp = 1.2**, **Ki = 0.0**, **Kd = 0.7**, **N_filter = 10.0**

A Kp sweep from 0.8 to 1.6 showed that Kp = 1.2 provides the best trade-off between overshoot, settling time, and rise time while respecting actuator saturation limits (u_max = 0.5, du_max = 1.0).

## Key Design Decisions

1. **Complementary Filter (alpha = 0.99)**: Fuses gyroscope and accelerometer data to provide a stable pitch estimate with good transient response.
2. **Derivative Filter (N_filter = 10.0)**: Prevents noise amplification in the derivative term.
3. **Anti-Windup**: Conditional integration prevents integral windup during actuator saturation.
4. **Actuator Rate Limiting**: du_max = 1.0 limits the rate of change of control input for realistic actuator behavior.

## Requirements

- Python 3.x
- NumPy
- Matplotlib
- Pandas
- Google Colab (recommended) or any Jupyter environment

## Installation and Usage

1. Open `UAV_2.ipynb` in Google Colab
2. Upload `UAV_2.ipynb`
3. Run all cells sequentially
4. No additional setup is required - all dependencies are pre-installed in Colab.

## Author

**Saad Wajih**
- GitHub: @SaadWajih99

## License

This project is open-source and available for educational purposes.
