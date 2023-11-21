---
title: "Patterns Implementation"
description: "Patterns Implementation"
---

### 1. Singleton Pattern:

```python
class Singleton:
    _instance = None

    def __new__(cls):
        if not cls._instance:
            cls._instance = super(Singleton, cls).__new__(cls)
        return cls._instance

# Usage
singleton_instance_1 = Singleton()
singleton_instance_2 = Singleton()

print(singleton_instance_1 is singleton_instance_2)  # Output: True (Both variables reference the same instance)
```

### 2. Factory Method Pattern:

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def sound(self):
        pass

class Dog(Animal):
    def sound(self):
        return "Woof"

class Cat(Animal):
    def sound(self):
        return "Meow"

class AnimalFactory:
    def create_animal(self, animal_type):
        if animal_type == 'dog':
            return Dog()
        elif animal_type == 'cat':
            return Cat()
        else:
            raise ValueError("Invalid animal type")

# Usage
factory = AnimalFactory()
dog = factory.create_animal('dog')
cat = factory.create_animal('cat')

print(dog.sound())  # Output: Woof
print(cat.sound())  # Output: Meow
```

### 3. Observer Pattern:

```python
class Subject:
    def __init__(self):
        self._observers = []

    def attach(self, observer):
        self._observers.append(observer)

    def detach(self, observer):
        self._observers.remove(observer)

    def notify(self, value):
        for observer in self._observers:
            observer.update(value)

class Observer:
    def update(self, value):
        print(f"Received value: {value}")

# Usage
subject = Subject()
observer1 = Observer()
observer2 = Observer()

subject.attach(observer1)
subject.attach(observer2)

subject.notify("Hello Observers!")
```

These examples provide a basic understanding and implementation of some design patterns in Python. The implementations may vary based on specific requirements and contexts. Additionally, Python's dynamic nature allows for different ways to implement these patterns based on the problem at hand.