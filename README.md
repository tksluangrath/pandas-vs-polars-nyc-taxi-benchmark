🚀 Overview

This project evaluates Pandas and Polars, two major Python DataFrame libraries, by applying them to real-world NYC Yellow Taxi trip data.

We compare both libraries across:

- CSV loading performance
- Data cleaning workflows
- Quality checks (QC)
- Feature engineering
- Aggregations
- Memory usage
- Visualization outputs

The goal is to provide a clear, practical demonstration of how these libraries behave under identical workloads—and offer guidance on when to use each.

📦 Dataset

NYC 2023 Yellow Taxi Trip Records
API Source: https://data.cityofnewyork.us/resource/4b4i-vvec.csv

A sample of 50,000 rows is fetched directly using the NYC Open Data API.

The dataset includes:

- Pickup/dropoff timestamps
- Trip distance
- Fares and taxes
- Passenger counts
- Location IDs


📚 Dependencies

Install everything via:

```bash
pip install -r requirements.txt
```


Key packages:

- `pandas`
- `polars`
- `matplotlib`
- `requests`
- `python-dotenv`
- `sodapy`

⚙️ Methods
1️⃣ Data Loading

CSV fetched once from the API → parsed using:

```python
pd.read_csv(...)
pl.read_csv(...)
```

Polars loads ~15× faster thanks to multithreaded Rust-based parsing.

2️⃣ Data Cleaning

Applied identical transformations in both libraries:

parse datetime columns

cast numeric values

filter invalid rows (`trip_distance ≤ 0`, `fare_amount < 0`)

Both libraries produced the same cleaned dataset shape:
48,572 rows × 19 columns

3️⃣ Quality Checks

Verified dataset integrity:

- Missing values → none
- Outliers → exactly 1 long trip (distance ~105 miles)
- No negative fares or totals

Both libraries flagged the same row, confirming correctness.

4️⃣ Feature Engineering

Added new computed fields:

- `trip_duration_minutes`
- `average_speed_mph`
- `pickup_hour`

Then aggregated by hour:

- number of trips
- average distance
- average speed

Pandas and Polars produced identical aggregated results.

5️⃣ Visualization

Used Matplotlib to create:

trips per hour

average distance per hour

average speed per hour

The Pandas and Polars lines overlap perfectly, confirming equal outputs.

6️⃣ Benchmarking

Measured average execution times over 100 runs for:

- CSV loading
- Cleaning
- QC
- Feature engineering + aggregation

| Task                              | Library | Avg Time (sec) |
| --------------------------------- | ------- | -------------- |
| CSV Load                          | Pandas  | 0.0463         |
| CSV Load                          | Polars  | 0.0032         |
| Cleaning                          | Pandas  | 0.0004         |
| Cleaning                          | Polars  | 0.0133         |
| Quality Check                     | Pandas  | 0.0003         |
| Quality Check                     | Polars  | 0.0018         |
| Feature Engineering + Aggregation | Pandas  | 0.0030         |
| Feature Engineering + Aggregation | Polars  | 0.0034         |

7️⃣ Memory Usage

| Library | Memory Usage (MB) |
| ------- | ----------------- |
| Pandas  | 9.2508            |
| Polars  | 8.3447            |


Polars used ~10% less memory, aligning with expectations for Arrow-backed columnar storage.

📊 Results

- Polars is dramatically faster at reading CSV files.
- Pandas is faster for small-scale cleaning and QC tasks.
- Both libraries produce identical analytical results.
- Polars is more memory efficient due to Arrow-native columnar storage.
- For mid-sized workloads (~50k rows), performance differences shrink.
- For larger datasets, Polars’ advantages would grow significantly.

🧠 Conclusion

Both Pandas and Polars are excellent tools—neither is “better,” but each excels in different contexts.

Choose Pandas if:

- You want familiar syntax and ecosystem compatibility
- Your dataset fits comfortably in memory
- You prioritize ease of use

Choose Polars if:

- You need maximum performance
- Your dataset is large (millions+ rows)
- You want lower memory usage
- You prefer expressive, Rust-inspired APIs

Final Thought

Pandas is great for productivity. Polars is great for performance. Knowing when to use each is a superpower for data scientists.

📬 Contact

If you use or extend this project, feel free to connect:

Author: Terrance Luangrath

GitHub: https://github.com/tksluangrath

LinkedIn: https://www.linkedin.com/in/terrance-luangrath/
