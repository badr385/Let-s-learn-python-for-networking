## Lesson 16 — Mini Automation Scripts

## The full flow

Automation is always:
> input → process → output

---

## Mini script (no libraries yet)

```py
devices = [
    {"hostname": "EDGE-RBA-01", "status": "up"},
    {"hostname": "EDGE-CAS-01", "status": "down"},
]

for d in devices:
    if d["status"].lower() != "up":
        print("ALERT |", d["hostname"], "| DOWN")
```

---

## Trust your automation

Trust comes from:
- clean output
- predictable behavior
- small functions

---

## Exercise

Add a counter that prints:
`Total DOWN: X`
