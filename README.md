# 🚗 Dynamic Parking Pricing System — Real-Time Stream Processing

This project implements a real-time dynamic parking pricing engine built on **Pathway**, a high-throughput stream processing framework. Using live-streamed parking sensor data, the system continuously computes optimal parking prices based on real-world demand signals — occupancy rate, vehicle type, queue length, traffic density, and special event flags.  

Rather than relying on fixed static rates, the pricing engine adapts in real time to actual ground conditions, simulating how a modern smart city parking infrastructure would behave in production. All price outputs are streamed to CSV and visualized through an interactive **Bokeh + Panel dashboard**.  

The dataset spans **October to December 2016 (Birmingham Parking Dataset)**, covering multiple lots sampled at regular intervals. Data is preprocessed, streamed via Pathway's replay mechanism, and aggregated in **30-minute tumbling windows** — one pricing decision per lot per window. Two distinct pricing models are built and compared.

---

## 🔄 System Workflow

```text
Raw Dataset (CSV)
      │
      ▼
Preprocessing
(Timestamp parsing, vehicle weight mapping, feature engineering)
      │
      ▼
Pathway Stream Replay
(Simulated real-time ingestion at controlled input rate)
      │
      ▼
30-Min Tumbling Window Aggregation
(Per parking lot · Max/Min occupancy · Demand signal extraction)
      │
      ▼
 ┌────────────────┐        ┌──────────────────────-┐
 │   Model 1      │        │       Model 2         │
 │ Occupancy-based│        │ Multi-factor weighted │
 └───────┬────────┘        └──────────┬────────────┘
         │                            │
         ▼                            ▼
   price_output.csv          price_output_2.csv
         │                            │
         └──────────┬─────────────────┘
                    ▼
        Bokeh + Panel Dashboard
        (Real-time price visualization)
```

---

## 🔵 Model 1 — Occupancy-Based Linear Pricing

Model 1 is the foundational pricing model where price rises proportionally with occupancy. It uses occupancy rate and vehicle type to compute a normalized demand score, which feeds into a linear pricing function anchored to a base price.

The model extracts max and min occupancy per 30-minute window, normalizes them against total lot capacity, and weights them by vehicle type — trucks contribute higher weight, while bikes and cycles contribute less. Output prices are clipped within defined bounds to ensure stability.

**Result:** Smooth pricing behavior (~₹12–₹35) with predictable peaks during high-demand periods and dips during low-demand intervals.

---

## 🟣 Model 2 — Multi-Factor Competitive Pricing

Model 2 incorporates multiple demand signals beyond occupancy, including queue length, traffic density, vehicle type, and special event flags. Each factor is weighted and combined into a composite demand score scaled around a base price.

This creates a pricing mechanism that reacts more aggressively to real-world conditions. High traffic and events trigger price spikes, while low activity keeps prices near the minimum.

**Result:** More dynamic pricing (~₹4–₹20) with sharper fluctuations driven by multiple interacting variables.

---

## ⚙️ Tech Stack

| Layer             | Tools        |
| ----------------- | ------------ |
| Stream Processing | Pathway      |
| Data Handling     | Pandas       |
| Visualization     | Bokeh, Panel |
| Language          | Python 3     |
| Environment       | Google Colab |

---

## 📁 Project Structure

```text
├── Model_1_cleaned.ipynb
├── Model_2_cleaned.ipynb
├── dataset.csv
├── parking_stream.csv
├── price_output.csv
├── price_output_2.csv
├── model_1_plot.png
└── model_2_plot.png
```

---

📌 *Capstone Project · Birmingham Parking Dataset (Oct–Dec 2016) · 30-min tumbling windows · Multi-lot real-time simulation*
