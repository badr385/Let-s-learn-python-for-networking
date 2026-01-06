## Lesson 12 — Python for Networking: Libraries & the Big Picture

## What you are about to do

In this part, you will learn **why Python matters** for networking:
- how Python talks to devices (SSH, APIs)
- which libraries are common in network automation
- how “network = data” becomes real work (inventory, reports, config pushes)
- why you should always start from a **real need** and build step by step

---

## Python is interpreted (why that’s good)

Python is an **interpreted language**:
- you write code
- the Python interpreter runs it immediately

Why it helps:
- fast feedback (run → see output → adjust)
- easy debugging
- perfect for iterative automation

---

## The mindset: start from a need

The best scripts are not “cool scripts”.  
They are scripts that solve a real pain.

Example needs:
- “I need to check BGP status on 50 routers every morning.”
- “I need to push a safe config snippet on 80 sites.”
- “I need a report in Excel for management.”

Rule:
> Need → small steps → reliable output → scale

---

## Common networking libraries (the ones you’ll see in real jobs)

### SSH / CLI automation
- **Netmiko**: simple SSH automation to network devices (popular, practical)
- **Scrapli**: modern alternative (fast, clean)
- **Paramiko**: low-level SSH building block (more manual)

### Multi-vendor configuration / state
- **NAPALM**: vendor abstraction (get facts, interfaces, config, etc.)

### Network protocols
- **ncclient**: NETCONF (structured configs, automation workflows)
- **pygnmi**: gNMI (telemetry / config on supported platforms)

### APIs
- **requests**: talk to REST APIs (controllers, SD-WAN managers, cloud)

---

## Input / Output libraries (inventory + reports)

Automation is not just connecting.  
It is also reading/writing data.

Common ones:
- `csv` (built-in): read/write CSV device inventories
- `json` (built-in): structured data and APIs
- `pathlib` (built-in): safe file paths
- `openpyxl` (Excel): read/write `.xlsx`
- `pandas` (dataframes): reports + Excel + CSV fast
- `yaml` (PyYAML): config files and inventories (common in automation)

---

## Your first “real” automation architecture

A good network script usually looks like this:

1. **Input**: load devices from CSV/Excel/YAML
2. **Connect**: SSH/API
3. **Collect**: run commands / get facts
4. **Decide**: logic checks (OK/ALERT)
5. **Output**: clean summary + save report

Keep it simple. Then scale.

---

## Mini exercise (no device required)

Make a tiny inventory and print it cleanly:

```py
devices = [
    {"hostname": "EDGE-RBA-01", "ip": "10.0.0.1", "site": "Rabat"},
    {"hostname": "EDGE-CAS-01", "ip": "10.0.0.2", "site": "Casablanca"},
]

for d in devices:
    print(d["hostname"], d["ip"], d["site"], sep=" | ")
```

Expected output:
```
EDGE-RBA-01 | 10.0.0.1 | Rabat
EDGE-CAS-01 | 10.0.0.2 | Casablanca
```
