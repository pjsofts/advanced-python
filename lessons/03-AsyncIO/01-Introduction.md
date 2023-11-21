---
title: "Async IO"
description: "Async IO"
---

In Python, `async` and `await` are used in asynchronous programming to define and work with coroutines, allowing for non-blocking execution of code. Asynchronous programming is beneficial when dealing with I/O-bound tasks like network operations, file I/O, or accessing databases, where waiting for the result can cause delays.

Here's a breakdown of `async` and `await`:

### `async`:
- `async` is used to declare that a function is a coroutine, which can run asynchronously.
- When a function is declared with `async`, it allows you to use `await` within that function, indicating where the coroutine should wait for the result of another coroutine or asynchronous operation.

### `await`:
- `await` is used within an `async` function to pause the execution of that coroutine until the awaited coroutine or asynchronous operation completes.
- It allows you to wait for the result of other coroutines, such as I/O operations, without blocking the execution of other code.

### Example:

Consider a simple example of asynchronous code using `async` and `await` in Python with `asyncio`, Python's built-in library for asynchronous programming:

```python
import asyncio

async def say_hello():
    print("Hello")
    await asyncio.sleep(1)  # Simulate a delay without blocking
    print("World!")

async def main():
    await say_hello()

# Run the coroutine
asyncio.run(main())
```

Explanation:
- The `say_hello()` function is a coroutine declared with `async`.
- Inside `say_hello()`, the `await asyncio.sleep(1)` line pauses the coroutine for 1 second without blocking other tasks. This simulates an asynchronous operation (in this case, a delay).
- The `main()` function is also a coroutine declared with `async`, which calls `say_hello()` using `await`.
- Finally, `asyncio.run(main())` is used to execute the `main()` coroutine.

When `await asyncio.sleep(1)` is encountered, the `say_hello()` coroutine is paused for 1 second, allowing other asynchronous operations or coroutines to run concurrently. This non-blocking behavior is fundamental in asynchronous programming to prevent waiting for I/O operations and to utilize resources efficiently.

Asynchronous programming with `async` and `await` in Python enables developers to write more responsive and efficient code, especially when dealing with I/O-bound tasks that would otherwise lead to unnecessary waiting.