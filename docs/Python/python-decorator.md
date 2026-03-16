# Decorator

Decorator is a function that extends the behavior of another function without modifying the base fucntion. We pass the base function as an argument to the decorator.

1. We will calling `brew_tea` function as soon as we apply the `timer_dec` decorator:

```python
import time

def timer_dec(base_fn):
    start = time.time()
    base_fn()
    end = time.time()
    print(f"Task time: {end - start} seconds")

@timer_dec
def brew_tea():
    print(f"Brewing tea...")
    time.sleep(1)
    print("Tea is ready!")
```

2. We only want to apply the decorator when we call `brew_tea` function. So we need the `timer_dec` decorator to return `enhanced_fn` function with variable arguements:

```python
import time

def timer_dec(base_fn):
    def enhanced_fn(*args, **kwargs):
        start = time.time()
        base_fn(*args, **kwargs)
        end = time.time()
        print(f"Task time: {end - start} seconds")
    return enhanced_fn

@timer_dec
def brew_tea(tea_type, steep_time):
    print(f"Brewing {tea_type} tea...")
    time.sleep(steep_time)
    print("Tea is ready!")

if __name__ == '__main__':
    brew_tea("green", 1)
```

3. `timer_dec` decorator with return value:

```python
import time

def timer_dec(base_fn):
    def enhanced_fn(*args, **kwargs):
        start = time.time()
        result = base_fn(*args, **kwargs)
        end = time.time()
        print(f"Task time: {end - start} seconds")
        return result
    return enhanced_fn

@timer_dec
def make_matcha():
    print("Making matcha...")
    time.sleep(1)
    print("Matcha is ready!")
    return f"Drink matcha at {time.time()}"

if __name__ == '__main__':
    result = make_matcha()
    print(result)
```

#### Reference

- [Python Decorators - Visually Explained](https://www.youtube.com/watch?v=3tyaO-OE0K0)