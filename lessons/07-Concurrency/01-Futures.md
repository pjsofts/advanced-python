---
title: "Futures"
description: "Futures"
---

In Python, `Futures` are a high-level interface for asynchronously executing a callable (a function or method) and obtaining its eventual result. They are a part of the `concurrent.futures` module, which provides a high-level interface for asynchronously executing functions.

### Usage of Futures:

#### 1. Asynchronous Execution:

Futures allow you to execute a function asynchronously without blocking the main thread. This is particularly useful when dealing with I/O-bound tasks or running multiple functions concurrently.

#### 2. Obtaining Results:

They provide a way to retrieve the result of a function once it has finished executing asynchronously.

### Example Using `concurrent.futures.Future`:

```python
import concurrent.futures

# Define a function to be executed asynchronously
def square(n):
    return n * n

# Create a ThreadPoolExecutor (can also use ProcessPoolExecutor)
with concurrent.futures.ThreadPoolExecutor() as executor:
    # Submit the function for execution asynchronously
    future = executor.submit(square, 5)

    # Do other tasks while 'square' is running asynchronously
    # ...

    # Obtain the result once the function execution is finished
    result = future.result()
    print(result)  # Output: 25
```

In this example:

- `executor.submit(square, 5)` submits the `square` function with an argument `5` for asynchronous execution and returns a `Future` object.
- While the function is running asynchronously, you can perform other tasks.
- `future.result()` blocks until the function is complete and returns the result.

### Key Methods and Attributes of `concurrent.futures.Future`:

- **`result()`:** Waits for the function to complete and returns its result. It blocks until the result is available.
- **`done()`:** Returns `True` if the function has completed execution.
- **`cancel()`:** Attempts to cancel the execution of the function. Returns `True` if successful.
- **`exception()`:** Returns the exception raised by the function (if any) or `None` if no exception occurred.
- **`add_done_callback(fn)`:** Adds a callback function to be executed when the `Future` is completed.

### Types of Executors:

Python's `concurrent.futures` module provides two types of executors:

1. **ThreadPoolExecutor:** Executes tasks in a pool of threads.
2. **ProcessPoolExecutor:** Executes tasks in a pool of processes.

### Considerations:

- Futures are suitable for I/O-bound tasks where waiting for I/O operations is the main bottleneck.
- For CPU-bound tasks, the Global Interpreter Lock (GIL) in CPython limits the actual parallelism due to which `ProcessPoolExecutor` might be more effective than `ThreadPoolExecutor`.

Futures in Python offer a convenient way to perform tasks asynchronously, allowing for more efficient resource utilization and improved responsiveness in I/O-bound scenarios.