## Lesson 2 — Variables & Types

## What you are about to do

In this lesson, you will:
- understand what a variable is
- learn the 3 core types you will use all the time (numbers, strings, booleans)
- learn the biggest beginner traps (and how to avoid them)
- do a mini network-style exercise

You are expected to:
- run every code block
- change values and observe output
- never guess: print it

---

## What is a variable (simple definition)

A variable is a **label** that points to a value.

```py
site = "Rabat"
print(site)
```

Think of it like a sticky note on a box.

---

## Core types you will use everywhere

### Numbers (int / float)
```py
mtu = 1500
delay_ms = 12.5
print(mtu, delay_ms)
```

### Strings (text)
```py
hostname = "EDGE-RBA-01"
print(hostname)
```

### Booleans (True / False)
```py
is_up = True
is_shutdown = False
print(is_up, is_shutdown)
```

---

## The 3 beginner traps (important)

### Trap 1 — Quotes change everything
These are NOT the same:

```py
print(22)      # number
print("22")    # text
```

Why it matters in networking:
- numbers are used for math (counters, timers, VLAN IDs)
- strings are used for labels and parsing (hostnames, interface names)

### Trap 2 — `=` is assignment, `==` is comparison
```py
x = 10      # assign
print(x == 10)  # compare (True)
```

### Trap 3 — Python is case-sensitive
```py
Status = "UP"
# print(status)  # NameError (different name)
print(Status)
```

---

## Reassigning values (normal and useful)

Variables can change over time:

```py
status = "DOWN"
print("Before:", status)

status = "UP"
print("After:", status)
```

This is how you model “state” (like interface status).

---

## Reading variables mentally

Train this habit:

- read from right to left on assignment
- say it out loud in your head

Example:
```py
vlan = 100
iface = "GigabitEthernet0/0"
```
Mental reading:
- “vlan equals 100”
- “iface equals GigabitEthernet0/0”

---

## Network mini-lab: build a clean status line

Run this:

```py
hostname = "EDGE-RBA-01"
iface = "GigabitEthernet0/0"
is_up = True

print("Host:", hostname)
print("Interface:", iface)
print("Up:", is_up)
```

Now change:
- hostname
- iface
- is_up

Observe how your output changes.

---

## Exercise (do it now)

Goal:
Print this exact output (with your own values):

```
EDGE-RBA-01 | GigabitEthernet0/0 | UP
```

Rules:
- create 3 variables: hostname, iface, status
- use ONE `print()` call
- use `sep=" | "`

Starter:

```py
hostname = "EDGE-RBA-01"
iface = "GigabitEthernet0/0"
status = "UP"

# your print here
```

Expected solution pattern:
```py
print(hostname, iface, status, sep=" | ")
```
