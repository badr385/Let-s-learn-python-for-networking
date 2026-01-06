## Lesson 3 — Operators & Logic

## What you are about to do

In this lesson, you will:
- use math operators (useful for counters, timers, sizing)
- compare values (the core of “is this OK?”)
- use boolean logic (and / or / not)
- read conditions like English (no panic)

---

## Arithmetic operators (quick + useful)

```py
packets_in = 1200
packets_out = 950
total = packets_in + packets_out
print("Total packets:", total)
```

Common operators:
- `+` add
- `-` subtract
- `*` multiply
- `/` divide (float)
- `//` floor divide (int)
- `%` modulo (remainder)

---

## Network example: check packet loss percent

```py
sent = 1000
received = 970
loss = sent - received
loss_pct = (loss / sent) * 100
print("Loss %:", loss_pct)
```

---

## Comparison operators (the “truth” operators)

They return **True** or **False**.

```py
mtu = 1500
print(mtu == 1500)  # True
print(mtu != 1500)  # False
print(mtu > 1400)   # True
```

Common comparisons:
- `==` equal
- `!=` not equal
- `>` greater
- `<` less
- `>=` greater or equal
- `<=` less or equal

---

## Boolean logic (and / or / not)

### `and` (both must be True)
```py
bgp_up = True
ping_ok = True
print(bgp_up and ping_ok)  # True
```

### `or` (at least one True)
```py
primary_ok = False
backup_ok = True
print(primary_ok or backup_ok)  # True
```

### `not` (flip)
```py
is_shutdown = False
print(not is_shutdown)  # True
```

---

## The big pitfall: mixing strings and numbers

```py
vlan = "100"
# print(vlan + 1)  # TypeError
print(int(vlan) + 1)     # 101
```

If you parse text from device output, you often need conversions.

---

## Read conditions like English

Example:

```py
cpu = 78
mem = 65

if cpu > 80 or mem > 80:
    print("Alert")
```

Read it as:
> “If CPU is above 80 OR memory is above 80, print Alert.”

---

## Exercise

You are given:

```py
latency_ms = 42
loss_pct = 1.8
```

Goal: print **OK** only if:
- latency is <= 50
- and loss is < 2

Starter:

```py
latency_ms = 42
loss_pct = 1.8

# your if here
```

Expected output (with given values):
```
OK
```
