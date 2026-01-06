## Lesson 6 — Lists & Dictionaries

## What you are about to do

In this lesson, you will:
- understand lists (ordered data)
- understand dictionaries (key/value data)
- access and update values safely
- see why dictionaries are everywhere in networking

---

## Lists: ordered data

A list is an ordered collection.

```py
ports = [22, 80, 443]
print(ports[0])  # 22
```

Pitfall:
- index starts at 0 (not 1)

---

## Dictionaries: key/value logic

A dictionary maps a key to a value.

```py
iface = {"name": "eth0", "status": "UP", "mtu": 1500}
print(iface["status"])
```

Pitfall:
- wrong key → KeyError

Safer access:
```py
print(iface.get("speed", "UNKNOWN"))
```

---

## Update values

```py
iface["status"] = "DOWN"
print(iface)
```

---

## Why dictionaries are everywhere in networking

Networking data is naturally key/value:
- interface name → status
- device → IP
- tunnel → metrics
- VLAN → subnet

Example (very real automation shape):

```py
device = {
    "hostname": "EDGE-RBA-01",
    "mgmt_ip": "10.10.10.10",
    "site": "Rabat",
    "roles": ["sdwan", "wan-edge"],
}
print(device["hostname"], device["mgmt_ip"])
```

---

## Exercise

You have interface data:

```py
iface = {"name": "GigabitEthernet0/0", "status": "down"}
```

Goal:
- print `GigabitEthernet0/0 is DOWN` (uppercase status)
- without changing the original dictionary

Hint:
- use `.upper()` on the value
