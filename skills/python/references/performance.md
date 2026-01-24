# Performance Engineering

Deep reference for profiling and optimizing Python.

## Methodology

1. **Measure** — Profile before optimizing
2. **Identify** — Find the real bottleneck
3. **Optimize** — Fix the constraint
4. **Verify** — Confirm improvement with benchmarks
5. **Prevent** — Add regression tests

## Profiling Tools

### CPU Profiling with cProfile

```python
import cProfile
import pstats

def profile_function(func, *args, **kwargs):
    profiler = cProfile.Profile()
    profiler.enable()
    result = func(*args, **kwargs)
    profiler.disable()

    stats = pstats.Stats(profiler)
    stats.sort_stats("cumulative")
    stats.print_stats(20)  # Top 20 functions

    return result
```

### Line-by-Line with line_profiler

```python
# Install: pip install line_profiler

# Decorate functions to profile
@profile
def slow_function():
    total = 0
    for i in range(1000000):
        total += i ** 2
    return total

# Run: kernprof -l -v script.py
```

### Production Profiling with py-spy

```bash
# Attach to running process
py-spy top --pid 12345

# Record flame graph
py-spy record -o profile.svg --pid 12345

# Profile specific script
py-spy record -o profile.svg -- python script.py
```

### Memory Profiling

```python
from memory_profiler import profile

@profile
def memory_intensive():
    data = [i ** 2 for i in range(1000000)]
    return sum(data)

# Run: python -m memory_profiler script.py
```

## Optimization Patterns

### Vectorization with NumPy

```python
import numpy as np

# Bad - Python loop
def slow_sum_squares(data: list[float]) -> float:
    return sum(x ** 2 for x in data)

# Good - vectorized
def fast_sum_squares(data: np.ndarray) -> float:
    return np.sum(data ** 2)

# Speedup: 10-100x for large arrays
```

### Avoiding Object Churn

```python
# Bad - creates intermediate objects
def slow_process(items: list[str]) -> list[str]:
    result = []
    for item in items:
        result.append(item.upper().strip())
    return result

# Good - generator expression
def fast_process(items: list[str]) -> list[str]:
    return [item.upper().strip() for item in items]

# Better for streaming - generator
def stream_process(items: Iterable[str]) -> Iterator[str]:
    for item in items:
        yield item.upper().strip()
```

### Batching for I/O

```python
async def slow_fetch(urls: list[str]) -> list[bytes]:
    results = []
    for url in urls:
        results.append(await fetch(url))  # Sequential
    return results

async def fast_fetch(urls: list[str], batch_size: int = 10) -> list[bytes]:
    results = []
    for i in range(0, len(urls), batch_size):
        batch = urls[i:i + batch_size]
        results.extend(await asyncio.gather(*[fetch(u) for u in batch]))
    return results
```

## Native Extensions

### PyO3 / Rust

```rust
// src/lib.rs
use pyo3::prelude::*;

#[pyfunction]
fn sum_squares(values: Vec<f64>) -> f64 {
    values.iter().map(|x| x * x).sum()
}

#[pymodule]
fn my_extension(_py: Python, m: &PyModule) -> PyResult<()> {
    m.add_function(wrap_pyfunction!(sum_squares, m)?)?;
    Ok(())
}
```

```toml
# pyproject.toml
[build-system]
requires = ["maturin>=1.0,<2.0"]
build-backend = "maturin"

[tool.maturin]
features = ["pyo3/extension-module"]
```

### Cython (Legacy/Numeric)

```python
# fast_math.pyx
cimport cython

@cython.boundscheck(False)
@cython.wraparound(False)
def sum_squares(double[:] values):
    cdef double total = 0.0
    cdef Py_ssize_t i
    for i in range(values.shape[0]):
        total += values[i] * values[i]
    return total
```

## Benchmarking

```python
import pytest

def test_performance(benchmark):
    result = benchmark(process_data, large_dataset)
    assert result is not None

# Run: pytest --benchmark-only
# Output includes mean, stddev, min, max, iterations
```

### CI Performance Budget

```yaml
# .github/workflows/benchmark.yml
- name: Run benchmarks
  run: pytest tests/benchmarks --benchmark-json=benchmark.json

- name: Check performance regression
  run: |
    python scripts/check_benchmark.py benchmark.json \
      --max-regression=10%
```

## Signals of Mastery

- Optimization is driven by measurements, validated by before/after
- Improvements come from eliminating real constraints
- Native extensions are used surgically with clear boundaries
- Latency and throughput goals are met consistently
- Performance stays stable because regressions are detected automatically
