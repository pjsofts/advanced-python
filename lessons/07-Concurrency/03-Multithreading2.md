---
title: "Multithreading 2"
description: "Multithreading 2"
---

Let's delve deeper into multithreading in Python and explore more concepts and functionalities.

### Thread Synchronization:

#### Locks:

Locks, represented by `threading.Lock()`, are used to prevent multiple threads from accessing shared resources concurrently, ensuring data integrity. Acquire a lock using `lock.acquire()` and release it using `lock.release()`.

```python
import threading

shared_resource = 0
lock = threading.Lock()

def modify_shared_resource():
    global shared_resource
    for _ in range(100000):
        lock.acquire()
        shared_resource += 1
        lock.release()

threads = [threading.Thread(target=modify_shared_resource) for _ in range(5)]

for thread in threads:
    thread.start()

for thread in threads:
    thread.join()

print(f"Final shared resource value: {shared_resource}")  # Expected output: 500000
```

#### RLocks (Reentrant Locks):

`threading.RLock()` is a reentrant lock that allows the same thread to acquire the lock multiple times, preventing deadlocks.

```python
import threading

lock = threading.RLock()

def foo():
    with lock:
        print("Acquired lock in foo")
        bar()

def bar():
    with lock:
        print("Acquired lock in bar")

foo()
```

### Thread Communication:

#### Condition Variables:

Condition variables (`threading.Condition()`) are used to coordinate between multiple threads. They allow threads to wait until notified by another thread.

```python
import threading

condition = threading.Condition()
shared_queue = []

def producer():
    for i in range(10):
        with condition:
            shared_queue.append(i)
            condition.notify()  # Notify the consumer
            condition.wait()  # Wait for consumer signal

def consumer():
    for _ in range(10):
        with condition:
            while not shared_queue:
                condition.wait()  # Wait for producer signal
            print(f"Consumed: {shared_queue.pop()}")
            condition.notify()  # Notify the producer

producer_thread = threading.Thread(target=producer)
consumer_thread = threading.Thread(target=consumer)

producer_thread.start()
consumer_thread.start()

producer_thread.join()
consumer_thread.join()
```

#### Event Objects:

Event objects (`threading.Event()`) allow threads to wait for an event to be set or cleared by other threads.

```python
import threading
import time

event = threading.Event()

def wait_for_event():
    print("Waiting for event to be set")
    event.wait()
    print("Event set!")

def set_event():
    time.sleep(3)
    print("Event is set")
    event.set()

thread1 = threading.Thread(target=wait_for_event)
thread2 = threading.Thread(target=set_event)

thread1.start()
thread2.start()

thread1.join()
thread2.join()
```

### Thread-local Data:

Thread-local data (`threading.local()`) allows data to be stored and accessed on a per-thread basis, avoiding interference between threads.

```python
import threading

local_data = threading.local()

def set_data(value):
    local_data.value = value

def print_data():
    print(f"Data in thread: {threading.current_thread().name} - {local_data.value}")

thread1 = threading.Thread(target=set_data, args=(5,))
thread2 = threading.Thread(target=set_data, args=(10,))
thread3 = threading.Thread(target=print_data)

thread1.start()
thread2.start()
thread3.start()

thread1.join()
thread2.join()
thread3.join()
```

### Summary:

- Thread synchronization mechanisms such as locks, RLocks, condition variables, and event objects help manage shared resources and coordinate between threads.
- Thread-local data (`threading.local()`) allows data to be stored separately for each thread.
- Understanding these concepts is crucial for building concurrent and thread-safe applications in Python.