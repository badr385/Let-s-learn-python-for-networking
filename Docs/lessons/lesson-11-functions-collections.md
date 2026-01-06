## Lesson 11 — Functions with Lists & Dictionaries

## Passing data to functions

Automation often uses lists of devices and dicts of attributes.

---

## Example: count down interfaces

```py
def count_down(interfaces):
    down = 0
    for iface in interfaces:
        if iface["status"].lower() != "up":
            down += 1
    return down

interfaces = [
    {"name": "eth0", "status": "UP"},
    {"name": "eth1", "status": "down"},
]

print(count_down(interfaces))
```

---

## Return structured results

```py
def summarize_device(device):
    return {
        "hostname": device["hostname"],
        "site": device["site"],
        "ok": device["status"].lower() == "up",
    }
```

---

## Exercise

Given a list of devices, return a list of hostnames that are down.
