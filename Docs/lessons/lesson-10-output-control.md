## Lesson 10 — Functions and Output Control

## Returning vs printing

- `print()` shows output to humans
- `return` gives output to code

Good automation:
- returns data
- prints only clean summaries

---

## Bad pattern (hard to reuse)

```py
def add(a, b):
    print(a + b)
```

---

## Good pattern (reusable)

```py
def add(a, b):
    return a + b

print(add(2, 3))
```

---

## Network example: validation function

```py
def is_mtu_ok(mtu):
    return mtu == 1500

mtu = 1500
if is_mtu_ok(mtu):
    print("MTU OK")
else:
    print("MTU WRONG")
```

---

## Exercise

Write a function `format_alert(host, message)` that returns:

`ALERT | <host> | <message>`

Then print it.
