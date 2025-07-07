# Capstone_Project_CAC_IITG
Here is your **README.md** rephrased without emojis for clean addition to your GitHub repository:

---

# Smart Parking Dynamic Pricing System

A real-time, intelligent pricing engine for smart city parking lots that dynamically adjusts rates based on demand, location-based competition, and environmental context (traffic, vehicle type, special days, etc.). This project simulates urban-scale parking lot optimization using streaming data and edge-level intelligence.

---

## Overview

Urban congestion and inefficient parking pricing lead to traffic delays, lost revenue, and poor utilization. This system demonstrates how cities can leverage data for dynamic real-time pricing, optimizing:

* Space utilization
* Revenue generation
* Customer fairness

---

## Tech Stack

| Layer             | Tools/Frameworks                                              |
| ----------------- | ------------------------------------------------------------- |
| Language          | Python 3.11                                                   |
| Stream Engine     | [Pathway](https://pathway.com) for real-time table processing |
| Visualization     | Panel + Bokeh                                                 |
| IDE/Notebook      | Jupyter Notebook / Google Colab                               |
| Data Source       | Simulated CSV stream (`parking_stream.csv`)                   |
| Architecture Tool | Mermaid.js                                                    |

---

## Architecture Diagram

flowchart TD
A[parking_stream.csv (simulated real-time feed)]
A --> B[Pathway Streaming Ingest]
B --> C[Model 1: Timestamp + Schema Parsing]
C --> D[Model 2: Demand-Based Pricing]
D --> E[Model 3: Competitive Adjustment]
E --> F[Final Table: final_price]
F --> G[Live Price Visualization (Panel + Bokeh)]
```

---

## Models Overview & Algorithms

### Model 1 — Preprocessing & Timestamp

* Reads `parking_stream.csv` in simulated real-time
* Parses schema and converts string timestamps to datetime objects

Tools: `Pathway.replay_csv`, `datetime.strptime`

---

### Model 2 — Demand-Aware Pricing

Adjusts prices dynamically based on real-time indicators:

```
demand_raw = α × (Occupancy / Capacity) + β × QueueLength − γ × traffic_score + δ × IsSpecialDay + ε × vehicle_score

demand_norm = min(1, max(0, demand_raw / 5))

price = base_price × (1 + λ × demand_norm)
```

| Symbol | Factor             | Default |
| ------ | ------------------ | ------- |
| α      | Occupancy weight   | 1.0     |
| β      | Queue length       | 0.1     |
| γ      | Traffic penalty    | 0.5     |
| δ      | Special day bonus  | 0.3     |
| ε      | Vehicle type score | 1.0     |
| λ      | Demand scaler      | 0.5     |

Algorithm: Feature-weighted scoring → normalization → capped pricing

---

### Model 3 — Competition-Aware Adjustment

Incorporates spatial competition effects:

1. Cross join all parking lots → lot pairs
2. Compute Haversine distance between lots
3. Filter pairs where nearby competitor has a cheaper price
4. Calculate `avg_price_diff`
5. Adjust own price:

   ```
   final_price = price − φ × avg_price_diff
   ```

| Variable | Description             | Default |
| -------- | ----------------------- | ------- |
| φ        | Competition sensitivity | 0.5     |

Algorithm: Cross join → spatial filter → groupby-reduce → price correction

---

## Project Workflow

1. Ingest simulated data stream
2. Parse timestamps and schema (Model 1)
3. Compute demand-weighted prices (Model 2)
4. Adjust for nearby competition (Model 3)
5. Output `final_price` in real-time
6. Visualize using Bokeh + Panel

---

## Sample Output

* Time series plots of `final_price` per lot
* Responsive to spikes in demand, traffic, and competition
* Highly tunable via φ, λ, α, and other parameters

---

## Files Included

| File Name               | Description                     |
| ----------------------- | ------------------------------- |
| `Sample_Notebook.ipynb` | Base model development notebook |
| `dataset.csv`           | Sample CSV used for simulation  |
| `README.md`             | Project documentation           |

---

### Future Enhancements

* Integration with live IoT sensor feeds
* Extended competitive market simulations with external APIs
* Advanced fairness constraints using user personas

---

Built as a prototype to demonstrate dynamic, intelligent pricing for smart city parking optimization.
