📈 Demand Forecasting Project
A comprehensive time-series demand forecasting analysis implementing six classical forecasting methods, complete with KPI evaluation and visual performance comparison.

📋 Table of Contents

Project Overview
Dataset
Forecasting Methods

1. Naïve Forecast
2. 3-Period Moving Average
3. 4-Period Moving Average
4. Simple Exponential Smoothing (SES)
5. Linear Trend Forecast
6. Exponential Smoothing with Seasonality (ETS)


KPI Metrics
Model Performance Results
Key Findings
How to Use
Project Structure
Technologies Used


📌 Project Overview
This project applies six different time-series forecasting techniques to a real demand dataset. The goal is to compare model accuracy using standard forecasting KPIs and identify the best-performing method.
Each model is evaluated across five metrics:

MAPE – Mean Absolute Percentage Error
MAD – Mean Absolute Deviation
RMSE – Root Mean Square Error
Bias – Systematic over/under forecasting tendency
Accuracy – Overall forecast accuracy (%)


📊 Dataset
The dataset contains historical actual demand values used as the baseline for all forecasting models. Each forecasting method generates predictions that are compared against these actuals to compute error metrics.

🔢 Forecasting Methods
1. Naïve Forecast
Concept: The simplest possible forecasting method — the forecast for the next period is simply the actual value from the current period. It assumes nothing changes between periods.
Formula:
F^t+1=At\hat{F}_{t+1} = A_tF^t+1​=At​
Where:

F^t+1\hat{F}_{t+1}
F^t+1​ = Forecast for next period
AtA_t
At​ = Actual demand in current period tt
t

When to use: Good as a baseline benchmark. Works well for stable, random-walk data with no trend or seasonality.
Result: MAPE = 31.27%, Accuracy = 70.89% (weakest performer)

2. 3-Period Moving Average
Concept: Averages the last 3 actual demand values to produce the next forecast. Smooths out short-term fluctuations.
Formula:
F^t+1=At+At−1+At−23\hat{F}_{t+1} = \frac{A_t + A_{t-1} + A_{t-2}}{3}F^t+1​=3At​+At−1​+At−2​​
Where:

At,At−1,At−2A_t, A_{t-1}, A_{t-2}
At​,At−1​,At−2​ = Actual demand in the last 3 periods

When to use: Best for relatively stable demand with no strong trend. A smaller window (3) reacts faster to recent changes.
Result: MAPE = 18.22%, Accuracy = 83.74%

3. 4-Period Moving Average
Concept: Same as the 3-period moving average but uses the last 4 periods, producing a smoother (but slower-reacting) forecast.
Formula:
F^t+1=At+At−1+At−2+At−34\hat{F}_{t+1} = \frac{A_t + A_{t-1} + A_{t-2} + A_{t-3}}{4}F^t+1​=4At​+At−1​+At−2​+At−3​​
When to use: Slightly more smoothing than 3-MA. Useful when demand is noisier and a longer average reduces volatility.
Result: MAPE = 24.13%, Accuracy = 78.56%

💡 Note: The 3-MA outperformed the 4-MA, suggesting the data benefits from a shorter, more responsive window.


4. Simple Exponential Smoothing (SES)
Concept: Unlike moving averages (which weight all past observations equally), SES assigns exponentially decreasing weights to older observations. Recent data matters more than older data.
Formula:
F^t+1=α⋅At+(1−α)⋅F^t\hat{F}_{t+1} = \alpha \cdot A_t + (1 - \alpha) \cdot \hat{F}_tF^t+1​=α⋅At​+(1−α)⋅F^t​
Or equivalently in error-correction form:
F^t+1=F^t+α⋅(At−F^t)\hat{F}_{t+1} = \hat{F}_t + \alpha \cdot (A_t - \hat{F}_t)F^t+1​=F^t​+α⋅(At​−F^t​)
Where:

α\alpha
α = Smoothing parameter (0<α<1)(0 < \alpha < 1)
(0<α<1)
AtA_t
At​ = Actual demand at period tt
t
F^t\hat{F}_t
F^t​ = Previous forecast

Choosing α:
α valueBehaviorClose to 1Reacts quickly to changes (more weight on recent data)Close to 0Slower reaction, heavily relies on historical averages
When to use: Works well for data with no trend or seasonality. The α parameter is usually optimized by minimizing SSE (Sum of Squared Errors).
Result: MAPE = 17.59%, Accuracy = 84.38% (Best performer)

5. Linear Trend Forecast
Concept: Fits a straight line through the historical demand data using linear regression, then extrapolates that line forward for future forecasts.
Formula:
F^t=a+b⋅t\hat{F}_t = a + b \cdot tF^t​=a+b⋅t
Where the intercept aa
a and slope bb
b are estimated by:
b=n∑(t⋅At)−∑t⋅∑Atn∑t2−(∑t)2b = \frac{n \sum(t \cdot A_t) - \sum t \cdot \sum A_t}{n \sum t^2 - (\sum t)^2}b=n∑t2−(∑t)2n∑(t⋅At​)−∑t⋅∑At​​
a=Aˉ−b⋅tˉa = \bar{A} - b \cdot \bar{t}a=Aˉ−b⋅tˉ
Where:

tt
t = Time period index
AtA_t
At​ = Actual demand at period tt
t
Aˉ\bar{A}
Aˉ = Mean of actual demand values
tˉ\bar{t}
tˉ = Mean of time indices
nn
n = Number of periods

When to use: Effective when demand shows a consistent upward or downward trend over time.
Result: MAPE = 23.14%, Accuracy = 79.52%

6. Exponential Smoothing with Seasonality (ETS)
Concept: ETS (Error, Trend, Seasonality) extends simple exponential smoothing to handle both trend and seasonality simultaneously. It uses three smoothing equations.
Holt-Winters Additive ETS Formula:
Level:
Lt=α(At−St−s)+(1−α)(Lt−1+Tt−1)L_t = \alpha(A_t - S_{t-s}) + (1 - \alpha)(L_{t-1} + T_{t-1})Lt​=α(At​−St−s​)+(1−α)(Lt−1​+Tt−1​)
Trend:
Tt=β(Lt−Lt−1)+(1−β)Tt−1T_t = \beta(L_t - L_{t-1}) + (1 - \beta)T_{t-1}Tt​=β(Lt​−Lt−1​)+(1−β)Tt−1​
Seasonal Index:
St=γ(At−Lt)+(1−γ)St−sS_t = \gamma(A_t - L_t) + (1 - \gamma)S_{t-s}St​=γ(At​−Lt​)+(1−γ)St−s​
**Forecast:**

F^t+m=Lt+m⋅Tt+St−s+m\hat{F}_{t+m} = L_t + m \cdot T_t + S_{t-s+m}F^t+m​=Lt​+m⋅Tt​+St−s+m​
Where:

α\alpha
α = Level smoothing parameter
β\beta
β = Trend smoothing parameter
γ\gamma
γ = Seasonal smoothing parameter
ss
s = Length of seasonal cycle (e.g., 4 for quarterly, 12 for monthly)
mm
m = Number of periods ahead being forecast

When to use: When data contains both trend and seasonal patterns. More powerful but requires more data and tuning.
Result: MAPE = 25.91%, Accuracy = 78.59%

📐 KPI Metrics
All models are evaluated using the following five metrics:
MetricFormulaInterpretationMAPE$\frac{1}{n}\sum\left\frac{A_t - F_t}{A_t}\rightMAD1n∑∥At−Ft∥\frac{1}{n}\sum\|A_t - F_t\|
n1​∑∥At​−Ft​∥Average absolute error in demand units; lower is betterRMSE1n∑(At−Ft)2\sqrt{\frac{1}{n}\sum(A_t - F_t)^2}
n1​∑(At​−Ft​)2​Penalizes large errors more; lower is betterBias1n∑(Ft−At)\frac{1}{n}\sum(F_t - A_t)
n1​∑(Ft​−At​)Positive = over-forecast; Negative = under-forecast; closer to 0 is betterAccuracy100−MAPE100 - \text{MAPE}
100−MAPEHigher is better; direct readability of model performance

📊 Model Performance Results
MethodMAPEMADRMSEBiasAccuracyNaïve31.2739.8348.090.03070.89%3-MA18.2222.3227.890.07783.74%4-MA24.1329.4335.490.32278.56%SES17.5921.4435.490.75484.38% ✅Linear Trend23.1429.3935.49~0.00079.52%ETS25.9128.1135.4911.07578.59%

🔍 Key Findings

✅ Best Model: Simple Exponential Smoothing (SES) — Achieved the lowest MAPE (17.59%) and highest accuracy (84.38%), outperforming all other methods.
📉 Naïve Forecast was the weakest model with only 70.89% accuracy, as expected for a baseline method.
🔁 3-MA outperformed 4-MA, suggesting the dataset benefits from a shorter, more reactive smoothing window.
⚖️ Linear Trend had near-zero bias (~5.68E-16), making it the most unbiased model — useful when bias is a critical concern.
🌊 ETS had the highest bias (11.08), indicating it tends to over-forecast for this particular dataset.
All models except Naïve share the same RMSE (35.49), indicating similar performance on penalized large-error metrics.


🚀 How to Use

Clone the repository:

bash   git clone https://github.com/your-username/demand-forecasting.git
   cd demand-forecasting

Open the Excel workbook:

All forecasting models and KPI calculations are built directly in the Excel file.
Navigate to each sheet tab to explore individual model outputs.


Review the KPI dashboard:

The KPI Summary sheet contains the comparison table and charts for all models.


Modify the data:

Replace the actual demand values in the Data sheet with your own dataset.
All forecasts and KPIs will automatically recalculate.




📁 Project Structure
demand-forecasting/
│
├── data/
│   └── demand_data.xlsx          # Raw actual demand data
│
├── forecasting/
│   └── forecast_models.xlsx      # All 6 forecasting models with formulas
│
├── results/
│   ├── kpi_summary.png           # KPI comparison table screenshot
│   └── forecast_chart.png        # Forecast vs Actual chart
│
└── README.md

🛠 Technologies Used

Microsoft Excel — Forecasting formulas, KPI calculations, and charting
Excel Functions Used:

AVERAGE() — Moving averages
SLOPE(), INTERCEPT() — Linear trend regression
Custom ETS smoothing formulas
ABS(), SQRT(), SUMPRODUCT() — KPI metric calculations
