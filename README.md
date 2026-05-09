# 📈 Demand Forecasting Project
## 📌 Project Overview
This project applies **six different time-series forecasting techniques** to a real demand dataset. The goal is to compare model accuracy using standard forecasting KPIs and identify the best-performing method.

Each model is evaluated across five metrics:

| Metric | Description |
|--------|-------------|
| **MAPE** | Mean Absolute Percentage Error |
| **MAD** | Mean Absolute Deviation |
| **RMSE** | Root Mean Square Error |
| **Bias** | Systematic over/under forecasting tendency |
| **Accuracy** | Overall forecast accuracy (%) |

---

## 📊 Dataset
The dataset contains **historical actual demand values** used as the baseline for all forecasting models. Each forecasting method generates predictions that are compared against these actuals to compute error metrics.

---

## 🔢 Forecasting Methods

### 1. Naïve Forecast
**Concept:**
The simplest possible forecasting method — the forecast for the next period is simply the actual value from the current period. It assumes nothing changes between periods.

**Formula:**
F(t+1) = A(t)

| Symbol | Meaning |
|--------|---------|
| `F(t+1)` | Forecast for the next period |
| `A(t)` | Actual demand in the current period t |

**When to use:**
Good as a **baseline benchmark**. Works well for stable, random-walk data with no trend or seasonality.

> 📊 **Result:** MAPE = 31.27% | Accuracy = 70.89% *(weakest performer)*

---

### 2. 3-Period Moving Average
**Concept:**
Averages the last **3 actual demand values** to produce the next forecast. Smooths out short-term fluctuations.

**Formula:**
F(t+1) = [ A(t) + A(t-1) + A(t-2) ] / 3

| Symbol | Meaning |
|--------|---------|
| `A(t)` | Actual demand in the most recent period |
| `A(t-1)` | Actual demand 2 periods ago |
| `A(t-2)` | Actual demand 3 periods ago |

**When to use:**
Best for relatively stable demand with no strong trend. A smaller window (3) **reacts faster** to recent changes.

> 📊 **Result:** MAPE = 18.22% | Accuracy = 83.74%

---

### 3. 4-Period Moving Average
**Concept:**
Same as the 3-period moving average but uses the last **4 periods**, producing a smoother but slower-reacting forecast.

**Formula:**
F(t+1) = [ A(t) + A(t-1) + A(t-2) + A(t-3) ] / 4

| Symbol | Meaning |
|--------|---------|
| `A(t)` | Actual demand in the most recent period |
| `A(t-1), A(t-2), A(t-3)` | Actual demand in the 3 prior periods |

**When to use:**
Slightly more smoothing than 3-MA. Useful when demand is noisier and a longer average reduces volatility.

> 📊 **Result:** MAPE = 24.13% | Accuracy = 78.56%

> 💡 *The 3-MA outperformed the 4-MA, suggesting the data benefits from a shorter, more responsive window.*

---

### 4. Simple Exponential Smoothing (SES)
**Concept:**
Unlike moving averages (which weight all past observations equally), SES assigns **exponentially decreasing weights** to older observations — recent data matters more.

**Formula:**
F(t+1) = α × A(t) + (1 - α) × F(t)

**Equivalent error-correction form:**
F(t+1) = F(t) + α × [ A(t) - F(t) ]

| Symbol | Meaning |
|--------|---------|
| `α` (alpha) | Smoothing parameter, where `0 < α < 1` |
| `A(t)` | Actual demand at period t |
| `F(t)` | Forecast for the current period |

**Choosing α:**

| α Value | Behavior |
|---------|----------|
| Close to **1** | Reacts quickly — more weight on recent data |
| Close to **0** | Slower reaction — relies heavily on historical averages |

**When to use:**
Works well for data with **no strong trend or seasonality**. The α value is typically optimized by minimizing the Sum of Squared Errors (SSE).

> 📊 **Result:** MAPE = 17.59% | Accuracy = **84.38% ✅** *(Best performer)*

---

### 5. Linear Trend Forecast
**Concept:**
Fits a **straight line** through the historical demand data using linear regression, then extrapolates that line forward for future forecasts.

**Forecast formula:**
F(t) = a + b × t

**Slope (b):**
b = [ n × Σ(t × A(t)) − Σt × ΣA(t) ] / [ n × Σ(t²) − (Σt)² ]

**Intercept (a):**
a = mean(A) − b × mean(t)

| Symbol | Meaning |
|--------|---------|
| `t` | Time period index (1, 2, 3, …) |
| `A(t)` | Actual demand at period t |
| `mean(A)` | Mean of all actual demand values |
| `mean(t)` | Mean of all time period indices |
| `n` | Total number of periods |

**When to use:**
Effective when demand shows a **consistent upward or downward trend** over time.

> 📊 **Result:** MAPE = 23.14% | Accuracy = 79.52%

---

### 6. Exponential Smoothing with Seasonality (ETS)
**Concept:**
ETS (**E**rror, **T**rend, **S**easonality) extends simple exponential smoothing to handle both **trend and seasonality** simultaneously using three smoothing equations.

**Holt-Winters Additive Model:**

**Level equation:**
L(t) = α × (A(t) − S(t−s)) + (1 − α) × (L(t−1) + T(t−1))

**Trend equation:**
T(t) = β × (L(t) − L(t−1)) + (1 − β) × T(t−1)

**Seasonal index equation:**
S(t) = γ × (A(t) − L(t)) + (1 − γ) × S(t−s)

**Forecast (m periods ahead):**
F(t+m) = L(t) + m × T(t) + S(t − s + m)

| Symbol | Meaning |
|--------|---------|
| `α` (alpha) | Level smoothing parameter |
| `β` (beta) | Trend smoothing parameter |
| `γ` (gamma) | Seasonal smoothing parameter |
| `s` | Length of seasonal cycle (e.g., 4 = quarterly, 12 = monthly) |
| `m` | Number of periods ahead being forecast |
| `L(t)` | Estimated level at period t |
| `T(t)` | Estimated trend at period t |
| `S(t)` | Seasonal index at period t |

**When to use:**
When data contains **both trend and seasonal patterns**. More powerful but requires more historical data and parameter tuning.

> 📊 **Result:** MAPE = 25.91% | Accuracy = 78.59%

---

## 📐 KPI Metrics
All models are evaluated using the following five metrics:

| Metric | Formula | Interpretation |
|--------|---------|----------------|
| **MAPE** | `(1/n) × Σ( |A(t) − F(t)| / A(t) ) × 100` | % error relative to actual — lower is better |
| **MAD** | `(1/n) × Σ |A(t) − F(t)|` | Average absolute error in demand units — lower is better |
| **RMSE** | `sqrt( (1/n) × Σ(A(t) − F(t))² )` | Penalizes large errors more heavily — lower is better |
| **Bias** | `(1/n) × Σ(F(t) − A(t))` | Positive = over-forecast; Negative = under-forecast; closer to 0 is better |
| **Accuracy** | `100 − MAPE` | Direct readability of model performance — higher is better |

---

## 📊 Model Performance Results

| Method | MAPE | MAD | RMSE | Bias | Accuracy |
|--------|------|-----|------|------|----------|
| Naïve | 31.27 | 39.83 | 48.09 | 0.030 | 70.89% |
| 3-MA | 18.22 | 22.32 | 27.89 | 0.077 | 83.74% |
| 4-MA | 24.13 | 29.43 | 35.49 | 0.322 | 78.56% |
| **SES** | **17.59** | **21.44** | **35.49** | **0.754** | **84.38% ✅** |
| Linear Trend | 23.14 | 29.39 | 35.49 | ~0.000 | 79.52% |
| ETS | 25.91 | 28.11 | 35.49 | 11.075 | 78.59% |

---

## 🔍 Key Findings
- ✅ **Best Model: SES** — Lowest MAPE (17.59%) and highest accuracy (84.38%), outperforming all other methods.
- 📉 **Naïve Forecast** was the weakest model with only 70.89% accuracy, as expected for a baseline.
- 🔁 **3-MA outperformed 4-MA**, suggesting the dataset benefits from a shorter, more reactive smoothing window.
- ⚖️ **Linear Trend** had near-zero bias (~5.68E-16), making it the most unbiased model — ideal when systematic error is a critical concern.
- 🌊 **ETS** had the highest bias (11.08), indicating it tends to over-forecast for this particular dataset.
- 📏 All models except Naïve share the same RMSE (35.49), indicating similar performance on penalized large-error metrics.






</div>
