## 🧪 Code Examples — From Simple to Automation

This section shows how Python code evolves **step by step**.
Nothing fancy at the beginning. Just logic.

---

## 1️⃣ Very simple: see output

```py
print("Hello, Network Engineer")
```

Goal:
- understand that code runs
- see immediate output

---

## 2️⃣ Variables + meaning

```py
hostname = "EDGE-RBA-01"
status = "UP"

print(hostname, "status is", status)
```

Goal:
- store information
- read code like a sentence

---

## 3️⃣ Conditions (decisions)

```py
status = "down"

if status.lower() == "up":
    print("Interface is UP")
else:
    print("Interface is DOWN")
```

Goal:
- make decisions
- understand execution paths

---

## 4️⃣ Loop over multiple elements

```py
interfaces = ["Gi0/0", "Gi0/1", "Gi0/2"]

for iface in interfaces:
    print("Checking", iface)
```

Goal:
- stop repeating yourself
- apply the same logic everywhere

---

## 5️⃣ Structured data (network-style)

```py
interfaces = [
    {"name": "Gi0/0", "status": "up"},
    {"name": "Gi0/1", "status": "down"},
]

for iface in interfaces:
    if iface["status"] != "up":
        print("ALERT:", iface["name"])
```

Goal:
- treat the network as data
- prepare for automation

---

## 6️⃣ Functions (reusable logic)

```py
def is_down(iface):
    return iface["status"] != "up"

for iface in interfaces:
    if is_down(iface):
        print("ALERT:", iface["name"])
```

Goal:
- reuse logic
- make code readable and testable

---

## 7️⃣ Real automation mindset

```py
devices = [
    {"hostname": "EDGE-RBA-01", "status": "up"},
    {"hostname": "EDGE-CAS-01", "status": "down"},
]

alerts = []

for d in devices:
    if d["status"] != "up":
        alerts.append(d["hostname"])

print("Total alerts:", len(alerts))
for host in alerts:
    print("ALERT |", host)
```

Goal:
- input → process → output
- clean, trustworthy automation

---

## Final thought

Automation is not about writing complex code.

It’s about:
- starting simple
- understanding each step
- stacking small pieces of logic

That’s how Python becomes useful for networking.
