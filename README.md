# Neural Activity Forecasting under Cross-Day Drift

This project explores multi-step forecasting of neural population activity from μECoG recordings as part of the NSF HDR ML Challenge (Jan 2026).

The primary challenge is **generalizing predictions across recording days**, where neural signals exhibit significant non-stationarity and electrode drift.

---

## Problem Overview

- Input: Multivariate neural time series  
  Shape: `(samples, 20 time steps, channels, features)`
- Task: Predict the **next 10 time steps** given the first 10
- Target: Feature 0 (neural activity)
- Constraint: Models must generalize to **unseen recording sessions**

---

## Approach

Key design choices:
- **Per-sample normalization** using the first 10 observed steps
- **Delta-based prediction**, forecasting changes relative to the last observed time step
- **Direct multi-step forecasting** (10 steps ahead)
- **Temporal Convolutional Network (TCN)** for stable long-horizon prediction
- **Cross-day validation** using held-out recording sessions

This setup explicitly addresses distribution shift caused by day-to-day electrode drift.

---

## Model

- Architecture: Temporal Convolutional Network (TCN)
- Input: `(10, channels × features)`
- Output: `(10 future steps)`
- Loss: Mean Squared Error (evaluated on future steps only)
- Optimizer: AdamW
- Validation: Held-out recording days

---

## Results

Models successfully learn neural dynamics and generalize across days.

Validation performance stabilizes early, highlighting the importance of early stopping and robustness over capacity.

Further improvements focus on:
- Robust loss functions (Huber)
- Noise-based regularization
- Drift-aware augmentation

---

## Repository Contents

- `Monkeys.ipynb`: End-to-end training, evaluation, and visualization (Google Colab)
- `README.md`: Project overview and methodology

> Datasets are not included due to size and access restrictions. See the NSF HDR ML Challenge documentation for data access.

---

## Future Work

- Incorporate session-invariant representations
- Explore GRU-based and hybrid architectures
- Evaluate uncertainty-aware forecasting

---

## Acknowledgements

NSF HDR ML Challenge organizers and dataset contributors.
