# Data Engineering & Analytics

Deep reference for data pipelines and analytics in Python.

## Library Selection

| Need | Library | Why |
|------|---------|-----|
| High-performance DataFrames | Polars | Lazy evaluation, parallelism |
| Legacy/compatibility | pandas | Ecosystem, familiarity |
| Local SQL analytics | DuckDB | Embedded OLAP, fast |
| Columnar interchange | Apache Arrow | Zero-copy, standard |
| Orchestration | Prefect | Modern, Pythonic |
| Numerical arrays | NumPy | Foundation layer |

## Polars Patterns

### Lazy Evaluation

```python
import polars as pl

# Lazy mode - query optimization, streaming
lf = (
    pl.scan_parquet("data/*.parquet")
    .filter(pl.col("date") >= "2024-01-01")
    .group_by("category")
    .agg(
        pl.col("value").sum().alias("total"),
        pl.col("value").mean().alias("average"),
    )
)

# Only executes when collected
result = lf.collect()
```

### Streaming Large Files

```python
# Process larger-than-memory files
lf = pl.scan_parquet("huge_file.parquet")
for batch in lf.collect(streaming=True).iter_slices(10_000):
    process_batch(batch)
```

### Expression Patterns

```python
# Prefer expressions over apply
df = df.with_columns(
    # Good - vectorized
    (pl.col("price") * pl.col("quantity")).alias("total"),

    # Good - conditional logic
    pl.when(pl.col("status") == "active")
      .then(pl.col("value"))
      .otherwise(0)
      .alias("active_value"),
)
```

## DuckDB Integration

```python
import duckdb

# Query Parquet directly
result = duckdb.sql("""
    SELECT category, SUM(value) as total
    FROM 'data/*.parquet'
    WHERE date >= '2024-01-01'
    GROUP BY category
    ORDER BY total DESC
""").fetchdf()

# Query Polars DataFrames
df = pl.DataFrame({"a": [1, 2, 3]})
duckdb.sql("SELECT * FROM df WHERE a > 1").fetchdf()
```

## Prefect Orchestration

### Basic Flow

```python
from prefect import flow, task
from prefect.tasks import task_input_hash
from datetime import timedelta

@task(
    retries=3,
    retry_delay_seconds=60,
    cache_key_fn=task_input_hash,
    cache_expiration=timedelta(hours=1),
)
def extract(source: str) -> pl.DataFrame:
    return pl.read_parquet(source)

@task
def transform(df: pl.DataFrame) -> pl.DataFrame:
    return df.filter(pl.col("valid") == True)

@task
def load(df: pl.DataFrame, destination: str) -> None:
    df.write_parquet(destination)

@flow(log_prints=True)
def etl_pipeline(source: str, destination: str) -> None:
    raw = extract(source)
    clean = transform(raw)
    load(clean, destination)
```

### Concurrent Tasks

```python
from prefect import flow, task
from prefect.futures import wait

@flow
def parallel_etl(sources: list[str]) -> None:
    # Submit tasks concurrently
    futures = [extract.submit(src) for src in sources]

    # Wait for all to complete
    results = [f.result() for f in futures]

    # Continue with merged data
    merged = pl.concat(results)
    load(merged, "output.parquet")
```

## Schema Management

```python
import polars as pl
from typing import TypedDict

class EventSchema(TypedDict):
    event_id: str
    timestamp: str
    user_id: int
    value: float

def validate_schema(df: pl.DataFrame) -> None:
    expected = {
        "event_id": pl.Utf8,
        "timestamp": pl.Datetime,
        "user_id": pl.Int64,
        "value": pl.Float64,
    }

    for col, dtype in expected.items():
        if col not in df.columns:
            raise ValueError(f"Missing column: {col}")
        if df[col].dtype != dtype:
            raise TypeError(f"Column {col}: expected {dtype}, got {df[col].dtype}")
```

## Data Quality Checks

```python
def quality_checks(df: pl.DataFrame) -> None:
    # Null checks
    null_counts = df.null_count()
    for col in ["event_id", "user_id"]:
        if null_counts[col][0] > 0:
            raise ValueError(f"Nulls found in required column: {col}")

    # Uniqueness
    if df["event_id"].n_unique() != len(df):
        raise ValueError("Duplicate event_ids found")

    # Range validation
    if (df["value"] < 0).any():
        raise ValueError("Negative values found")
```

## Signals of Mastery

- Minimizes unnecessary materialization (pushdown, lazy, streaming)
- Selects row vs column storage based on access patterns
- Pipelines are explainable with lineage and schemas
- Orchestration behavior is explicit (dependencies, retries, limits)
- Failures are diagnosable without manual archaeology
