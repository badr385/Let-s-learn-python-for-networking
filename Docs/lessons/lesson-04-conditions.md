## Lesson 4 — Conditions (if / elif / else)

## What you are about to do

In this lesson, you will:
- make decisions with `if`
- learn how Python chooses a branch
- avoid the 3 classic mistakes
- write a network-style decision (UP/DOWN)

---

## The simplest decision

```py
temperature = 31

if temperature > 30:
    print("Hot")
```

If the condition is True, the block runs.

---

## Condition flow (only ONE branch runs)

```py
score = 75

if score >= 90:
    print("A")
elif score >= 70:
    print("B")
else:
    print("C")
```

Python checks from top to bottom and stops at the first match.

---

## The 3 classic mistakes

### Mistake 1 — forgetting indentation
Indentation is not style in Python. It is logic.

### Mistake 2 — using `=` instead of `==`
```py
x = 5
if x == 5:
    print("OK")
```

### Mistake 3 — comparing text with wrong case
```py
status = "up"
print(status == "UP")  # False
print(status.upper() == "UP")  # True
```

---

## Network example: interpret interface status

```py
iface = "GigabitEthernet0/0"
status = "up"

if status.lower() == "up":
    print(iface, "is UP")
else:
    print(iface, "is DOWN")
```

---

## Network example: multi-level decision

```py
cpu = 83
if cpu >= 90:
    print("CRITICAL")
elif cpu >= 80:
    print("WARNING")
else:
    print("OK")
```

---

## Exercise

Goal: print the correct message for this device.

Rules:
- if `site` is "Rabat" and `link_up` is True → print "Rabat link OK"
- else → print "Check link"

Starter:

```py
site = "Rabat"
link_up = True

# your if here
```
