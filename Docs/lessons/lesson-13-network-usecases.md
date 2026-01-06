## Lesson 13 — Simple Network Use Cases

## Use case: check interface status

```py
iface = {"name": "Gi0/0", "status": "down"}

if iface["status"].lower() != "up":
    print("ALERT:", iface["name"], "is DOWN")
```

---

## Use case: count down interfaces

```py
interfaces = [
    {"name": "Gi0/0", "status": "up"},
    {"name": "Gi0/1", "status": "down"},
    {"name": "Gi0/2", "status": "down"},
]

down = 0
for iface in interfaces:
    if iface["status"].lower() != "up":
        down += 1

print("Down:", down)
```

---

## Use case: validate values

```py
mtu = 1400
if mtu != 1500:
    print("MTU mismatch:", mtu)
```

---

## Exercise

Given a list of interfaces, print only the down ones in the format:
`DOWN | <name>`
