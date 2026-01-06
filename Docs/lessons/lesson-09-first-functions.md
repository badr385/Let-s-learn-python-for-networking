## Lesson 9 — Writing Your First Functions

## What you are about to do

In this lesson, you will:
- write functions with `def`
- use parameters
- return values
- call functions like building blocks

---

## Your first useful function

```py
def greet(name):
    print("Hello", name)

greet("Badr")
```

---

## Parameters

Parameters are inputs.

```py
def tag(site, device):
    return site + "-" + device

print(tag("RBA", "EDGE01"))
```

---

## Return values

Return is the “output” of the function.

```py
def double(x):
    return x * 2

print(double(10))
```

---

## Network example: build a status line

```py
def status_line(hostname, iface, status):
    return hostname + " | " + iface + " | " + status.upper()

print(status_line("EDGE-RBA-01", "Gi0/0", "up"))
```

---

## Exercise

Write a function `is_vlan_valid(vlan)` that returns True if vlan is between 1 and 4094.

Starter:
```py
def is_vlan_valid(vlan):
    # your code
    pass
```
