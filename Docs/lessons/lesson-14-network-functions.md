## Lesson 14 — Functions for Network Tasks

## One function = one task

Good automation uses small functions:
- easy to test
- easy to reuse
- easy to trust

---

## Example: normalize text

```py
def norm(text):
    return text.strip().lower()
```

---

## Example: one function for one check

```py
def is_interface_up(iface):
    return iface["status"].lower() == "up"

iface = {"name": "Gi0/0", "status": "up"}
print(is_interface_up(iface))
```

---

## Clean separation of concerns

- parsing: extract data
- logic: decide
- output: display

---

## Exercise

Write:
- `check_mtu(mtu)` → returns True if mtu == 1500
- then use it in an if/else to print OK/WRONG
