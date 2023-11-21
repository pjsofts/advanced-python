---
title: "Generators Exercises"
description: "Generators Exercises"
---



### Exercise 1: Create a generator that generates a Fibonacci sequence up to a given limit.

```python
def fibonacci_generator(limit):
    a, b = 0, 1
    while a < limit:
        yield a
        a, b = b, a + b

# Using the generator to print Fibonacci numbers up to a limit
limit = 50
fib_gen = fibonacci_generator(limit)
for num in fib_gen:
    print(num)
```

### Exercise 2: Create a generator that generates even numbers up to a given limit.

```python
def even_number_generator(limit):
    for i in range(0, limit, 2):
        yield i

# Using the generator to print even numbers up to a limit
limit = 20
even_gen = even_number_generator(limit)
for num in even_gen:
    print(num)
```

### Exercise 3: Create a generator that generates the squares of numbers up to a given limit.

```python
def square_generator(limit):
    for i in range(limit):
        yield i ** 2

# Using the generator to print squares of numbers up to a limit
limit = 7
square_gen = square_generator(limit)
for num in square_gen:
    print(num)
```

### Exercise 4: Create a generator that generates a custom countdown sequence.

```python
def countdown_generator(start):
    while start >= 0:
        yield start
        start -= 1

# Using the generator to print a countdown sequence
start_count = 5
countdown_gen = countdown_generator(start_count)
for num in countdown_gen:
    print(num)
```

These exercises demonstrate different use cases for generators in Python. You can run each code snippet to see how the generators produce sequences based on the specified criteria.