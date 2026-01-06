## Lesson 8 — What Is a Function

## Why functions exist

Functions help you:
- avoid repetition
- reuse logic safely
- make code readable
- build automation that scales

---

## A function is a tool

You give it input.  
It returns output.

---

## The simplest function

```py
def say_hello():
    print("Hello")
```

Call it:
```py
say_hello()
```

---

## Inputs and outputs

```py
def add(a, b):
    return a + b

result = add(2, 3)
print(result)
```

Key idea:
- `return` gives a value back to the caller
- `print` only displays text

---

## Network example: normalize interface status

```py
def normalize_status(status):
    return status.strip().lower()

print(normalize_status(" UP "))
```

---

## Exercise

Write a function `is_up(status)` that returns True if status is "up" (any case).

Starter:
```py
def is_up(status):
    # your code
    pass
```
