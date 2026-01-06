## Lesson 12 — Network Data as Python Data

## Network = data

Automation becomes easy when you treat networks as data:
- interfaces → dictionaries
- devices → dictionaries
- inventory → list of dictionaries
- configs → text

---

## Interfaces as dictionaries

```py
iface = {"name": "Gi0/0", "admin": "up", "oper": "down", "mtu": 1500}
print(iface["name"], iface["oper"])
```

---

## Device lists

```py
devices = [
    {"hostname": "EDGE-RBA-01", "mgmt_ip": "10.0.0.1"},
    {"hostname": "EDGE-CAS-01", "mgmt_ip": "10.0.0.2"},
]
```

---

## Config as text

```py
config = "interface Gi0/0\n description WAN\n ip address 10.0.0.1 255.255.255.0\n"
print(config)
```

---

## Exercise

Create a dictionary for a device with:
- hostname
- mgmt_ip
- site
- role

Then print one clean summary line.
