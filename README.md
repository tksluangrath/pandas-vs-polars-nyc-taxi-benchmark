# Pandas vs Polars: Performance Showdown with NYC Taxi Data

## What This Is

A head-to-head performance comparison between Pandas and Polars using real NYC Yellow Taxi trip data. I wanted to see how these two popular DataFrame libraries actually stack up when doing the kind of work you'd do in a typical data analysis workflow.

## What I Tested

I put both libraries through the same workload:

- Loading CSV data from an API
- Cleaning and transforming the data
- Running quality checks
- Creating new features
- Aggregating results
- Memory usage
- Generating visualizations

The goal was to get a practical sense of how they perform on identical tasks and figure out when you'd actually want to use one over the other.

## The Data

I pulled 50,000 records from NYC's 2023 Yellow Taxi Trip Records using their [Open Data API](https://data.cityofnewyork.us/resource/4b4i-vvec.csv). The dataset includes things like:

- Pickup and dropoff times
- Trip distances
- Fares and tips
- Passenger counts
- Location IDs
- Payment types

## Setup

Install everything you need:

```bash
pip install -r requirements.txt
```

Main packages used:
- **pandas** and **polars** (obviously)
- **matplotlib** for charts
- **requests** for HTTP calls
- **sodapy** for the NYC API
- **python-dotenv** for environment variables

## What I Did

### Loading the Data

Both libraries read from the same CSV:

```python
# Pandas
df_pandas = pd.read_csv(...)

# Polars
df_polars = pl.read_csv(...)
```

**Finding**: Polars was about 15× faster here. It uses Rust under the hood with multithreading, which really shows during I/O operations.

### Cleaning

Applied the same transformations in both:
- Parsed datetime columns
- Cast everything to the right types
- Filtered out bad data (negative fares, zero distances)

**Finding**: Both ended up with the exact same cleaned dataset - 48,572 rows × 19 columns.

### Quality Checks

Ran basic validation:
- Checked for missing values (none found)
- Looked for outliers (found one 105-mile trip)
- Made sure no negative fares slipped through

**Finding**: Both libraries caught the same issues, so they're equally reliable for data validation.

### Feature Engineering

Created some new columns:
- `trip_duration_minutes` - how long each trip took
- `average_speed_mph` - distance divided by time
- `pickup_hour` - what hour the trip started

Then aggregated by hour to see patterns throughout the day.

**Finding**: The results matched perfectly between Pandas and Polars.

### Visualization

Made some charts to show trips, distances, and speeds by hour using Matplotlib.

**Finding**: When you overlay the Pandas and Polars outputs, they're identical. Both produce the same analytical insights.

## Performance Results

I ran each operation 100 times and averaged the results:

| Task | Pandas (sec) | Polars (sec) | Winner |
|------|--------------|--------------|--------|
| CSV Load | 0.0463 | 0.0032 | Polars (14.5× faster) |
| Cleaning | 0.0004 | 0.0133 | Pandas |
| Quality Check | 0.0003 | 0.0018 | Pandas |
| Feature Engineering + Aggregation | 0.0030 | 0.0034 | Pandas (slightly) |

**What this tells me:**
- Polars crushes it at loading data
- For small-scale operations on this dataset, Pandas is actually faster
- At 50K rows, the differences aren't huge except for I/O
- These gaps would widen significantly with larger datasets

## Memory Usage

| Library | Memory (MB) | 
|---------|-------------|
| Pandas | 9.25 |
| Polars | 8.34 |

Polars used about 10% less memory thanks to Apache Arrow's columnar format.

## What I Learned

Both libraries are solid - there's no clear winner across the board. It really depends on what you're doing.

**Use Pandas if:**
- You want familiar syntax that's well-documented
- You need compatibility with tons of other libraries
- Your data fits comfortably in memory
- You're prototyping or doing exploratory work
- You're maintaining existing code

**Use Polars if:**
- You're working with millions of rows
- Performance and memory efficiency matter
- You're building production pipelines
- You like functional, expressive APIs
- You need to scale up

At 50K rows, honestly either one works fine. But scale this up to 5 million rows and Polars' advantages would become way more obvious.

## Future Ideas

Things I might add later:
- Test with much larger datasets (millions of rows)
- Compare against Dask and DuckDB
- Benchmark write operations
- Test join performance
- Try GPU acceleration with cuDF
- Add streaming scenarios

## Contact

**Author**: Terrance Luangrath

**📧 Email:** [tksluangrath@gmail.com](mailto:tksluangrath@gmail.com)  
**💼 LinkedIn:** [![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/terranceluangrath/)  
**👨‍💻 GitHub:** [![GitHub](https://img.shields.io/badge/GitHub-181717.svg?logo=github&logoColor=white)](https://github.com/tksluangrath)

---

*Star this repo if you found it useful!*
