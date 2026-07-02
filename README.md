# Dynamic Inventory Optimization & Logistics Stress Testing Engine
An industrial-grade quantitative inventory control system built on multi-relational Brazilian e-commerce logistics operations (Olist dataset). This project models asymmetric supply chain latencies, dynamically calibrates Safety Stock & Reorder Points (ROP) under joint uncertainty, and evaluates catastrophic stockout risks using Monte Carlo simulation.

---

## Key Architectural Highlights
- **Parametric Latency Modeling:** Rejected naive normal distribution assumptions for logistics lead times; applied rigorous **Lognormal fitting** ($\mu = 2.30, \sigma = 0.68$) to accurately isolate the right-skewed, long-tailed tail risks of real-world delivery delays.
- **Joint-Uncertainty Propagation:** Formulated an advanced safety stock calculation engine based on the **Central Limit Theorem (CLT)**, combining concurrent fluctuations from both frontend consumer demand and backend shipping latencies.
- **Monte Carlo Stress Testing:** Engineered a stochastic simulation framework simulating black-swan supply chain disruptions, allowing executives to stress-test stockout probabilities under varying service level thresholds (95% vs. 99%).

---

## Analytical Pipeline & Core Framework

### 1. Data Alignment & Dimensional Extraction
- Synthesized heterogeneous database schemas (Orders, Items, Products, and Logistics timelines) into a unified analytical panel.
- Extracted exact physical metrics per SKU, converting raw dimension fields into **CBM (Cubic Meters)** to capture warehousing bulk characteristics.

### 2. Parametric Lead-Time Fitting
Real-world supply chains exhibit long-tailed distribution profiles due to customs delays, courier backlogs, and infrastructure anomalies. This engine fits the empirical `actual_lead_time_days` series into a continuous Lognormal distribution:

```python
# Core snippet mapping empirical data to lognormal spaces
shape, loc, scale = stats.lognorm.fit(valid_lead_times, floc=0)
