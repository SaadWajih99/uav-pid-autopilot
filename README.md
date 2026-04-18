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
├── UAV_2.ipynb          # Main Jupyter notebook with all code and results
├── README.md            # Project documentation
└── .gitignore           # Python .gitignore
```

## Results Summary

The PID controller successfully stabilizes UAV pitch attitude to the desired reference angle. Key findings include:

- **Settling time**: Achieved within expected bounds for the tuned PID gains.
- **Overshoot**: Minimized through careful gain selection.
- **Robustness**: Monte Carlo simulations confirm stable performance under parameter uncertainty.

## Installation & Usage

1. Open the notebook in **Google Colab**:
   - Go to [Google Colab](https://colab.research.google.com)
   - Upload `UAV_2.ipynb`
   - Run all cells sequentially

2. No additional setup is required — all dependencies (NumPy, Matplotlib, etc.) are pre-installed in Colab.

## Requirements

- Python 3.x
- NumPy
- Matplotlib
- Google Colab (recommended) or any Jupyter environment

## Author

**Saad Wajih**
- GitHub: [@SaadWajih99](https://github.com/SaadWajih99)

## License

This project is open-source and available for educational purposes.
