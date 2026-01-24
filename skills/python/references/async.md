# Concurrency & Asynchronous Systems

Deep reference for async Python and concurrency patterns.

## Choosing the Right Model

| Workload | Model | Why |
|----------|-------|-----|
| I/O-bound, many connections | `asyncio` | Non-blocking, single thread |
| CPU-bound, parallelizable | `multiprocessing` | Bypasses GIL |
| CPU-bound, shared state | `threading` + locks | GIL serializes, but state shared |
| Mixed I/O + CPU | `ProcessPoolExecutor` from async | Best of both |

## Structured Concurrency with TaskGroup

```python
import asyncio

async def fetch_all(urls: list[str]) -> list[bytes]:
    results: list[bytes] = []

    async with asyncio.TaskGroup() as tg:
        for url in urls:
            tg.create_task(fetch_one(url, results))

    return results  # Only reached if all tasks succeed

async def fetch_one(url: str, results: list[bytes]) -> None:
    async with httpx.AsyncClient(timeout=30.0) as client:
        response = await client.get(url)
        results.append(response.content)
```

## Cancellation Handling

```python
async def cancellable_operation() -> None:
    try:
        await long_running_task()
    except asyncio.CancelledError:
        # Cleanup before re-raising
        await cleanup_resources()
        raise  # Always re-raise CancelledError
```

## Timeouts Pattern

```python
async def with_timeout[T](coro: Awaitable[T], seconds: float) -> T:
    """Wrap any coroutine with a timeout."""
    try:
        return await asyncio.wait_for(coro, timeout=seconds)
    except asyncio.TimeoutError:
        raise TimeoutError(f"Operation timed out after {seconds}s")
```

## Bounded Concurrency

```python
async def process_with_limit(
    items: list[str],
    max_concurrent: int = 10
) -> list[Result]:
    semaphore = asyncio.Semaphore(max_concurrent)

    async def bounded_process(item: str) -> Result:
        async with semaphore:
            return await process_item(item)

    return await asyncio.gather(*[bounded_process(i) for i in items])
```

## Context Variables for Async

```python
from contextvars import ContextVar

request_id: ContextVar[str] = ContextVar("request_id", default="unknown")

async def handle_request(req_id: str) -> None:
    token = request_id.set(req_id)
    try:
        await process_request()
    finally:
        request_id.reset(token)

async def process_request() -> None:
    # Access context anywhere in the call chain
    print(f"Processing {request_id.get()}")
```

## GIL and Threading

```python
import threading
from concurrent.futures import ThreadPoolExecutor

# Threading for I/O-bound tasks (GIL released during I/O)
def download_files(urls: list[str]) -> list[bytes]:
    with ThreadPoolExecutor(max_workers=10) as executor:
        return list(executor.map(download_one, urls))

# For CPU-bound, use multiprocessing
from concurrent.futures import ProcessPoolExecutor

def compute_parallel(data: list[int]) -> list[int]:
    with ProcessPoolExecutor() as executor:
        return list(executor.map(heavy_computation, data))
```

## Async Debugging

```python
import asyncio
import logging

# Enable asyncio debug mode
asyncio.get_event_loop().set_debug(True)
logging.getLogger("asyncio").setLevel(logging.DEBUG)

# Inspect running tasks
for task in asyncio.all_tasks():
    print(f"Task: {task.get_name()}, State: {task.done()}")
    if not task.done():
        task.print_stack()
```

## Signals of Mastery

- Async is used where it provides measurable benefit, not as default style
- Deadlocks and race conditions are prevented through architecture
- Cancellation and shutdown behavior is defined and tested
- Concurrency limits are explicit (semaphores, pools, bounded queues)
- Production incidents are explainable in scheduling/contention terms
