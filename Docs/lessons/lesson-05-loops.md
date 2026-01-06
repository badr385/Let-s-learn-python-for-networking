## Lesson 5 — Loops (for / while)

## What you are about to do

In this lesson, you will:
- repeat actions with loops (automation mindset)
- loop over lists of interfaces/devices
- count things safely
- understand when loops stop

---

## `for` loop (repeat for each item)

```py
interfaces = ["eth0", "eth1", "eth2"]

for iface in interfaces:
    print("Checking", iface)
```

---

## Counting with conditions

```py
interfaces = [
    {"name": "eth0", "up": True},
    {"name": "eth1", "up": False},
    {"name": "eth2", "up": True},
]

down = 0
for iface in interfaces:
    if not iface["up"]:
        down += 1

print("Down interfaces:", down)
```

---

## The biggest pitfall: infinite `while` loops

A `while` loop repeats while the condition is True.

```py
x = 0
while x < 3:
    print("x =", x)
    x += 1
```

If you forget `x += 1`, it can run forever.

---

## Network example: retry logic (simple)

```py
attempts = 0
max_attempts = 3
ping_ok = False

while attempts < max_attempts and not ping_ok:
    attempts += 1
    print("Attempt", attempts, "- ping...")

    # pretend we succeed on the 3rd try
    if attempts == 3:
        ping_ok = True

print("Ping OK:", ping_ok)
```

---

## Exercise

You have a list of devices. Print only the ones in France.

```py
devices = [
    {"name": "RBA-EDGE-01", "country": "MA"},
    {"name": "PAR-EDGE-01", "country": "FR"},
    {"name": "LYO-EDGE-02", "country": "FR"},
]
```

Expected output:
```
PAR-EDGE-01
LYO-EDGE-02
```
