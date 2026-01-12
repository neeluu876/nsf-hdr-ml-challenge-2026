# Neural Activity Forecasting under Cross-Day Drift

This project investigates **multi-step forecasting of neural population activity**
from μECoG recordings as part of the **NSF HDR ML Challenge (Jan 2026)**.

The core challenge is **generalizing predictions across recording days**, where
neural signals exhibit strong non-stationarity due to electrode drift and
session-to-session variability.

---

## Problem Overview

- **Input:** Multivariate neural time series  
  Shape: `(samples, 20 time steps, channels, features)`
- **Task:** Predict the next 10 time steps given the first 10
- **Target:** Feature 0 (neural activity)
- **Key Constraint:** Models must generalize to **unseen recording sessions**

---

## Approach

Key modeling decisions:

- **Per-sample normalization** using only the observed context window
- **Delta-based prediction**, forecasting changes relative to the last observed step
- **Direct multi-step forecasting** over a 10-step horizon
- **Temporal Convolutional Network (TCN)** for stable long-horizon prediction
- **Cross-day validation**, holding out entire recording sessions

This setup explicitly addresses **distribution shift** caused by day-to-day
electrode drift.

---

## Model

- **Architecture:** Temporal Convolutional Network (TCN)
- **Input:** `(10, channels × features)`
- **Output:** `(10 future steps)`
- **Loss:** Mean Squared Error (future steps only)
- **Optimizer:** AdamW
- **Validation:** Held-out recording days (session-level split)

---

## Results

The model successfully learns neural dynamics and produces stable multi-step
forecasts across unseen recording days.

Key observations:
- Validation error stabilizes early, emphasizing robustness over model capacity
- Prediction error increases with forecast horizon, consistent with uncertainty
  accumulation in neural signals
- Predicted signals qualitatively align with ground truth across multiple channels

These results demonstrate the feasibility of **drift-aware neural forecasting**
under realistic recording conditions.

---

## Why This Matters

Accurate neural activity forecasting is critical for applications such as
**brain–computer interfaces** and **neuroprosthetics**, where models must operate
reliably across days and sessions. This project moves beyond same-session
assumptions toward models better aligned with real-world deployment.

---

## Repository Contents

- `Monkeys.ipynb` — End-to-end training, evaluation, and visualization (Google Colab)
- `README.md` — Project overview and methodology  

> Datasets are not included due to access and licensing restrictions.
> See the NSF HDR ML Challenge documentation for data access.

---

## Future Work

- Session-invariant representations
- GRU-based and hybrid temporal architectures
- Uncertainty-aware forecasting methods

---

## Acknowledgements

NSF HDR ML Challenge organizers and dataset contributors.
