---
title: "Multiprocessing"
description: "Multiprocessing"
---

Multiprocessing in Python involves using multiple processes to perform tasks concurrently, providing a way to utilize multiple CPU cores. Unlike multithreading, multiprocessing can bypass the Global Interpreter Lock (GIL) in CPython, allowing true parallelism by running separate interpreters in different processes.

### Key Aspects of Multiprocessing:

#### 1. `multiprocessing` Module:

The `multiprocessing` module in Python allows the creation and management of processes.

#### 2. Creating Processes:

You can create a process by instantiating the `Process` class and providing a target function to be executed in the new process.

```python
import multiprocessing

def print_numbers():
    for i in range(5):
        print(i)

# Create a process
process = multiprocessing.Process(target=print_numbers)

# Start the process
process.start()

# Wait for the process to finish
process.join()
```

#### 3. Communication between Processes:

Processes can communicate and share data using various methods like `Queue`, `Pipe`, and shared memory.

```python
import multiprocessing

def square(n, result_queue):
    result_queue.put(n * n)

# Create a Queue to store results
result_queue = multiprocessing.Queue()

# Create multiple processes
processes = [
    multiprocessing.Process(target=square, args=(i, result_queue)) for i in range(1, 6)
]

# Start the processes
for process in processes:
    process.start()

# Wait for all processes to finish
for process in processes:
    process.join()

# Retrieve results from the Queue
while not result_queue.empty():
    print(result_queue.get())  # Prints squares of numbers 1 to 5
```

#### 4. Pooling of Processes:

The `Pool` class in `multiprocessing` allows creating a pool of worker processes to execute functions asynchronously.

```python
import multiprocessing

def square(n):
    return n * n

# Create a Pool of worker processes
with multiprocessing.Pool(processes=3) as pool:
    result = pool.map(square, [1, 2, 3, 4, 5])

print(result)  # Output: [1, 4, 9, 16, 25]
```

#### 5. Shared Memory:

The `multiprocessing` module provides objects like `Value` and `Array` for sharing data between processes using shared memory.

```python
import multiprocessing

def modify_shared_data(shared_value):
    shared_value.value += 1

shared_value = multiprocessing.Value('i', 0)

process = multiprocessing.Process(target=modify_shared_data, args=(shared_value,))
process.start()
process.join()

print(shared_value.value)  # Output: 1
```

### Advantages of Multiprocessing:

- Utilizes multiple CPU cores effectively.
- Avoids the limitations of the Global Interpreter Lock (GIL), allowing true parallelism.
- Ideal for CPU-bound tasks.

### Considerations:

- Overhead in creating and managing processes might be higher compared to threads due to the larger memory footprint.
- Inter-process communication has overhead and might need synchronization mechanisms like `Queue`, `Pipe`, or shared memory.

Multiprocessing in Python provides a robust way to achieve parallelism, particularly for CPU-bound tasks, by leveraging multiple processes running concurrently. Understanding these concepts enables effective utilization of multiprocessing for performance-intensive tasks.