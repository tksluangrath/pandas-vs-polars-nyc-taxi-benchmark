# Pandas vs Polars: NYC Taxi Data Performance Benchmark

## 🚀 Overview

This project provides a comprehensive performance comparison between **Pandas** and **Polars**, two leading Python DataFrame libraries, using real-world NYC Yellow Taxi trip data.

### What We Compare

- **CSV Loading Performance** – How quickly each library ingests data
- **Data Cleaning Workflows** – Transformation and filtering operations
- **Quality Checks (QC)** – Data validation and integrity checks
- **Feature Engineering** – Creating derived columns and metrics
- **Aggregations** – Grouping and statistical computations
- **Memory Usage** – RAM consumption patterns
- **Visualization Outputs** – End-to-end analytical results

The goal is to provide clear, practical insights into how these libraries perform under identical workloads and offer guidance on when to use each.

---

## 📦 Dataset

**NYC 2023 Yellow Taxi Trip Records**

- **Source**: [NYC Open Data API](https://data.cityofnewyork.us/resource/4b4i-vvec.csv)
- **Sample Size**: 50,000 rows
- **Access Method**: Direct API fetch via SODA API

### Dataset Features

The dataset includes the following attributes:

- Pickup and dropoff timestamps
- Trip distance (miles)
- Fare amounts and taxes
- Passenger counts
- Pickup/dropoff location IDs
- Payment types and tip amounts

---

## 📚 Dependencies

### Installation

Install all required packages using:

```bash
pip install -r requirements.txt
```

### Key Packages

- **pandas** – Traditional DataFrame library
- **polars** – High-performance DataFrame library
- **matplotlib** – Data visualization
- **requests** – HTTP requests
- **python-dotenv** – Environment variable management
- **sodapy** – NYC Open Data API client

---

## ⚙️ Methodology

### 1️⃣ Data Loading

CSV data is fetched once from the NYC Open Data API and parsed using:

```python
# Pandas
df_pandas = pd.read_csv(...)

# Polars
df_polars = pl.read_csv(...)
```

**Result**: Polars loads data ~15× faster due to multithreaded, Rust-based parsing.

---

### 2️⃣ Data Cleaning

Identical transformations applied in both libraries:

- Parse datetime columns (`tpep_pickup_datetime`, `tpep_dropoff_datetime`)
- Cast numeric values to appropriate types
- Filter invalid records:
  - `trip_distance ≤ 0`
  - `fare_amount < 0`

**Result**: Both libraries produced identical cleaned datasets with **48,572 rows × 19 columns**.

---

### 3️⃣ Quality Checks

Data integrity verification:

- ✅ **Missing values**: None detected
- ✅ **Outliers**: 1 long-distance trip (~105 miles) identified
- ✅ **Negative values**: No negative fares or totals

**Result**: Both libraries flagged the same anomalies, confirming analytical correctness.

---

### 4️⃣ Feature Engineering

Created derived features:

- `trip_duration_minutes` – Time between pickup and dropoff
- `average_speed_mph` – Distance divided by duration
- `pickup_hour` – Hour of day extracted from pickup timestamp

Aggregated by hour:

- Total number of trips
- Average trip distance
- Average speed

**Result**: Pandas and Polars produced identical aggregated results.

---

### 5️⃣ Visualization

Generated visualizations using Matplotlib:

- Trips per hour (bar chart)
- Average distance per hour (line chart)
- Average speed per hour (line chart)

**Result**: Pandas and Polars outputs overlap perfectly, confirming equal analytical outcomes.

---

### 6️⃣ Performance Benchmarking

Average execution times measured over **100 runs** for each operation:

| Task                              | Library | Avg Time (sec) | Speedup    |
| --------------------------------- | ------- | -------------- | ---------- |
| CSV Load                          | Pandas  | 0.0463         | —          |
| CSV Load                          | Polars  | 0.0032         | **14.5×**  |
| Cleaning                          | Pandas  | 0.0004         | —          |
| Cleaning                          | Polars  | 0.0133         | 0.03×      |
| Quality Check                     | Pandas  | 0.0003         | —          |
| Quality Check                     | Polars  | 0.0018         | 0.17×      |
| Feature Engineering + Aggregation | Pandas  | 0.0030         | —          |
| Feature Engineering + Aggregation | Polars  | 0.0034         | 0.88×      |

**Key Takeaways**:
- Polars excels at I/O-bound operations (CSV loading)
- Pandas is faster for small-scale in-memory transformations
- Performance differences narrow for compute-bound operations at this scale

---

### 7️⃣ Memory Usage

| Library | Memory Usage (MB) | Efficiency |
| ------- | ----------------- | ---------- |
| Pandas  | 9.2508            | —          |
| Polars  | 8.3447            | **~10% less** |

**Result**: Polars demonstrates superior memory efficiency due to Apache Arrow's columnar storage format.

---

## 📊 Results Summary

### Performance Insights

✅ **Polars** is dramatically faster at reading CSV files (15× speedup)  
✅ **Pandas** is faster for small-scale cleaning and QC tasks on this dataset size  
✅ Both libraries produce **identical analytical results**  
✅ **Polars** is ~10% more memory efficient  
✅ For mid-sized workloads (~50K rows), performance differences are minimal  
✅ For larger datasets (millions of rows), Polars' advantages would compound significantly

---

## 🧠 Conclusion

Both Pandas and Polars are excellent tools—neither is universally "better," but each excels in different contexts.

### Choose Pandas if:

- ✅ You want familiar, mature syntax
- ✅ You need broad ecosystem compatibility
- ✅ Your dataset comfortably fits in memory
- ✅ You prioritize ease of use and rapid prototyping
- ✅ You're working with legacy codebases

### Choose Polars if:

- ✅ You need maximum performance at scale
- ✅ Your dataset has millions+ rows
- ✅ You want lower memory usage
- ✅ You prefer expressive, functional APIs
- ✅ You're building production data pipelines

### Final Thought

> **Pandas is great for productivity. Polars is great for performance.**  
> Knowing when to use each is a superpower for data scientists.

---

## 📈 Future Enhancements

Potential extensions to this project:

- [ ] Scale testing to millions of rows
- [ ] Add Dask and DuckDB comparisons
- [ ] Include write performance benchmarks
- [ ] Test join operations
- [ ] Evaluate GPU acceleration (cuDF)
- [ ] Add streaming data processing scenarios

---

## 📬 Contact

**Author**: Terrance Luangrath

**📧 Email:** [tksluangrath@gmail.com](mailto:tksluangrath@gmail.com)  
**💼 LinkedIn:** [![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/terranceluangrath/)  
**👨‍💻 GitHub:** [![GitHub](https://img.shields.io/badge/GitHub-181717.svg?logo=github&logoColor=white)](https://github.com/tksluangrath)

---

**⭐ If you find this project helpful, please consider giving it a star on GitHub!**
