---
title: "Multithreading 1"
description: "Multithreading 1"
---

Let's start with the basics of multithreading in Python.

### Multithreading in Python:

#### 1. **Threading Module:**

Python's `threading` module is used for creating and working with threads. Threads are separate flows of execution within a process. They can run concurrently and are useful for parallel execution of tasks.

#### 2. **Creating Threads:**

You can create a thread by instantiating the `Thread` class and providing a target function to be executed in the new thread.

```python
import threading

# Function to be executed in a thread
def print_numbers():
    for i in range(5):
        print(i)

# Create a thread
thread = threading.Thread(target=print_numbers)

# Start the thread
thread.start()
```

#### 3. **Joining Threads:**

The `join()` method allows the main thread to wait until the thread being joined has completed its execution.

```python
# Wait for the thread to finish its execution
thread.join()
print("Thread execution completed")
```

#### 4. **Thread Safety:**

When multiple threads share resources (like variables, data structures, etc.), thread safety becomes crucial. Synchronization mechanisms like locks (`Lock`, `RLock`), semaphores, and conditions are used to prevent race conditions and ensure data integrity.

#### Example with Lock:

```python
# Function with shared resource
counter = 0
lock = threading.Lock()

def increment():
    global counter
    for _ in range(100000):
        with lock:
            counter += 1

# Create multiple threads
threads = [threading.Thread(target=increment) for _ in range(5)]

# Start the threads
for thread in threads:
    thread.start()

# Wait for all threads to finish
for thread in threads:
    thread.join()

print(f"Counter value: {counter}")  # Expected output: 500000
```

#### Important Points:

- Python's Global Interpreter Lock (GIL) allows only one thread to execute Python bytecode at a time in a single process, limiting true parallelism in CPU-bound tasks in CPython.
- Multithreading in Python is effective for I/O-bound tasks where threads can wait for I/O operations without blocking other threads.

### Summary:

Multithreading in Python allows concurrent execution of tasks, useful for I/O-bound operations and tasks requiring parallelism without being CPU-bound. It's important to manage shared resources carefully using synchronization mechanisms to avoid data corruption in multithreaded applications.