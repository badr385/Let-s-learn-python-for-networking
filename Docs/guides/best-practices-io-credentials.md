## ✅ Best Practices — Inputs/Outputs + Secure Credentials (Network Automation)

This page fixes the **most common beginner mistakes**:
- hardcoding passwords ❌
- messy inventories ❌
- outputs you can’t trust ❌
- no separation between *input → process → output* ❌

It also gives you **clean patterns** you can reuse everywhere.

---

## 0) Golden rule (read this)

Automation is always:

> **Input → Process → Output**

If you don’t control the input and output, your script becomes dangerous.

---

## 1) ✅ Good input formats (inventory)

### ✅ TXT (simple)
Best for: quick lists (IPs, hostnames)

Example `devices.txt` (one per line):
```
10.0.0.1
10.0.0.2
10.0.0.3
```

Read it:
```python
def load_devices_txt(path: str) -> list[str]:
    devices = []
    with open(path, "r", encoding="utf-8") as f:
        for line in f:
            line = line.strip()
            if line and not line.startswith("#"):
                devices.append(line)
    return devices
```

Best practices:
- allow comments with `#`
- strip spaces
- ignore empty lines

---

### ✅ CSV (recommended standard)
Best for: structured inventory without Excel dependency

Example `devices.csv`:
```csv
hostname,ip,platform,site
EDGE-RBA-01,10.0.0.1,cisco_ios,Rabat
EDGE-CAS-01,10.0.0.2,cisco_ios,Casa
```

Read it (built-in `csv`):
```python
import csv

def load_devices_csv(path: str) -> list[dict]:
    with open(path, newline="", encoding="utf-8") as f:
        return list(csv.DictReader(f))
```

Best practices:
- keep column names consistent
- treat everything as strings
- validate required columns

---

### ✅ Excel (when business wants .xlsx)
Best for: business teams, managers, shared inventories

Read with pandas:
```python
import pandas as pd

def load_devices_excel(path: str) -> list[dict]:
    df = pd.read_excel(path)
    return df.to_dict(orient="records")
```

Best practices:
- avoid merged cells
- one header row only
- keep data types consistent (especially IPs)

---

### ❌ PDF as input (avoid)
PDF is for **humans**, not automation.
- difficult to parse reliably
- formatting breaks scripts
- low trust

✅ If you receive a PDF inventory: convert to CSV/Excel first.

---

## 2) ✅ Good output formats

### ✅ TXT output (human-friendly logs)
Best for: quick audits, troubleshooting, trace

Write a log file:
```python
from datetime import datetime

def write_txt_report(path: str, lines: list[str]) -> None:
    with open(path, "w", encoding="utf-8") as f:
        f.write(f"Report generated: {datetime.now().isoformat()}\n")
        f.write("\n".join(lines) + "\n")
```

Best practices:
- add a timestamp header
- keep one clean line per device
- never dump walls of text unless needed

---

### ✅ Excel output (management-friendly)
Best for: reports shared with managers/teams

Write with pandas:
```python
import pandas as pd

def write_excel_report(path: str, rows: list[dict]) -> None:
    df = pd.DataFrame(rows)
    df.to_excel(path, index=False)
```

Best practices:
- one row per device
- columns = facts (hostname, ip, version, uptime, status, error)
- no multi-line garbage inside cells

---

## 3) ✅ Secure credentials (no hardcoded passwords)

### ✅ Option A — Ask once with `getpass` (simple + secure)
Best for: ad-hoc scripts, lab, manual run

```python
import getpass

username = input("Username: ")
password = getpass.getpass("Password (hidden): ")
```

Why this is good:
- password not visible in terminal
- not saved in code
- not stored in git

---

### ✅ Option B — Environment variables (better for repeated runs)
Best for: CI/CD, scheduled scripts, automation servers

Set env vars (PowerShell):
```powershell
setx NET_USER "admin"
setx NET_PASS "YOUR_SECRET"
```

Use in Python:
```python
import os

username = os.environ.get("NET_USER")
password = os.environ.get("NET_PASS")

if not username or not password:
    raise RuntimeError("Missing NET_USER or NET_PASS environment variables")
```

Best practices:
- never commit `.env` files
- use secrets in GitHub Actions / Jenkins / vault

---

## 4) ✅ Clean structure template (copy/paste)

This is the pattern you should follow for every automation script.

```python
import getpass
from datetime import datetime

def load_devices_txt(path: str) -> list[str]:
    devices = []
    with open(path, "r", encoding="utf-8") as f:
        for line in f:
            line = line.strip()
            if line and not line.startswith("#"):
                devices.append(line)
    return devices

def main() -> None:
    # 1) INPUT
    devices = load_devices_txt("devices.txt")

    # 2) SECURE CREDS
    username = input("Username: ")
    password = getpass.getpass("Password: ")

    # 3) PROCESS (placeholder)
    results = []
    for ip in devices:
        # Here you'd connect to the device and collect info
        results.append(f"{ip} | OK")

    # 4) OUTPUT
    out_file = f"report_{datetime.now().strftime('%Y%m%d_%H%M%S')}.txt"
    with open(out_file, "w", encoding="utf-8") as f:
        f.write("\n".join(results) + "\n")

    print("Saved:", out_file)

if __name__ == "__main__":
    main()
```

---

## 5) ✅ Operational best practices (real-world)

- Use a **test lab** first.
- Do not run config changes without:
  - pre-check
  - post-check
  - rollback plan
- Keep output clean:
  - one-line summaries
  - clear FAIL reasons
- Handle failures:
  - timeouts
  - auth errors
  - command not supported
- Limit concurrency:
  - too many threads can overload devices and jump hosts
- Never store secrets in git.
- Add logging when scripts grow (file logs + timestamps).

---

## 6) Recommended “starter stack”

For network engineers learning automation:

- Netmiko (SSH)
- requests (REST APIs)
- csv/json (built-in)
- pandas + openpyxl (Excel reports)
- concurrent.futures (scale)
- getpass / env vars (secrets)


