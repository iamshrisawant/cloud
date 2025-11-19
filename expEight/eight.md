# **Experiment No. 8 — Simplest Working Procedure**

## **Title**

Implement AI-based auto-scaling using ML-based predictions.

---

## **Prerequisites**

1. **Python 3 installed** (preferably Python 3.8+).
2. **Libraries installed:**

   ```bash
   pip install pandas scikit-learn matplotlib
   ```
3. **A dataset of CPU usage or request rate over time** (CSV). Example format:

   ```
   timestamp, cpu_usage
   2024-01-01 10:00, 45
   2024-01-01 10:05, 50
   2024-01-01 10:10, 60
   ```
4. **Basic auto-scaling rule:**

   * If predicted CPU > 70 → **Scale Up**
   * If predicted CPU < 30 → **Scale Down**

---

# **Simplest Procedure (Runnable, Minimal, Working)**

## **Step 1 — Load historical workload data**

Create a file `autoscale.py` and include:

```python
import pandas as pd
from sklearn.linear_model import LinearRegression

# Load CPU usage data
df = pd.read_csv("cpu_data.csv")

X = df.index.values.reshape(-1, 1)     # time index as input
y = df["cpu_usage"].values             # CPU usage as output
```

---

## **Step 2 — Train a simple ML model (Linear Regression)**

```python
model = LinearRegression()
model.fit(X, y)
```

---

## **Step 3 — Predict next time interval workload**

```python
next_time = [[len(df)]]               # next future step
predicted_cpu = model.predict(next_time)[0]

print("Predicted CPU:", predicted_cpu)
```

---

## **Step 4 — Apply auto-scaling decision**

```python
if predicted_cpu > 70:
    print("ACTION: Scale Up (add VM/container)")
elif predicted_cpu < 30:
    print("ACTION: Scale Down (remove VM/container)")
else:
    print("ACTION: No scaling needed")
```

---

## **Step 5 — Run the script**

```bash
python autoscale.py
```

Expected output example:

```
Predicted CPU: 76.4
ACTION: Scale Up (add VM/container)
```

This **demonstrates AI-based predictive auto-scaling** in simplest form.

---

# **Verification**

1. Change the last few values in `cpu_data.csv` to simulate rising or falling load.
2. Run the script again.
3. Check if the scaling decision changes accordingly.

---

# **Output**

* Predicted future CPU usage
* Scaling decision (Scale Up / Scale Down / No Action)

Example:

```
Predicted CPU: 25.3
ACTION: Scale Down (remove VM)
```