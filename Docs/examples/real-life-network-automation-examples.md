## 🧠 Practical Automation Scripts (Best Practices Version)

This page contains **copy/paste-ready scripts** that follow good practices:

- ✅ Input inventory from **TXT / CSV / Excel**
- ✅ Credentials entered securely with **getpass** (no hardcoded passwords)
- ✅ Clean **TXT logs** + **Excel report** output
- ✅ Error handling (timeouts/auth)
- ✅ Optional **concurrency** for scale

> ⚠️ Safety
> - Test in a lab first.
> - For production changes: pre-check → change → post-check → rollback plan.

---

## 0) Install requirements

```bash
pip install netmiko pandas openpyxl requests
```

---

## 1) Inventory loaders (TXT / CSV / Excel)

```python
import csv
import pandas as pd

def load_devices_txt(path: str) -> list[dict]:
    """
    devices.txt format (one per line):
    ip,device_type,site,hostname

    Example:
    10.0.0.1,cisco_ios,Rabat,EDGE-RBA-01
    10.0.0.2,cisco_ios,Casa,EDGE-CAS-01
    """
    devices = []
    with open(path, "r", encoding="utf-8") as f:
        for line in f:
            line = line.strip()
            if not line or line.startswith("#"):
                continue
            parts = [p.strip() for p in line.split(",")]
            if len(parts) < 2:
                raise ValueError(f"Bad line in {path}: {line}")
            ip = parts[0]
            device_type = parts[1]
            site = parts[2] if len(parts) > 2 else ""
            hostname = parts[3] if len(parts) > 3 else ip
            devices.append({"ip": ip, "device_type": device_type, "site": site, "hostname": hostname})
    return devices

def load_devices_csv(path: str) -> list[dict]:
    """
    devices.csv columns:
    hostname,ip,device_type,site
    """
    with open(path, newline="", encoding="utf-8") as f:
        return list(csv.DictReader(f))

def load_devices_excel(path: str) -> list[dict]:
    """
    devices.xlsx columns:
    hostname | ip | device_type | site
    """
    df = pd.read_excel(path)
    return df.to_dict(orient="records")
```

---

## 2) Output writers (TXT log + Excel report)

```python
from datetime import datetime
import pandas as pd

def write_txt_log(path: str, lines: list[str]) -> None:
    with open(path, "w", encoding="utf-8") as f:
        f.write(f"Generated: {datetime.now().isoformat()}\n")
        f.write("\n".join(lines) + "\n")

def write_excel_report(path: str, rows: list[dict]) -> None:
    df = pd.DataFrame(rows)
    df.to_excel(path, index=False)
```

---

## 3) Script A — Cisco: check static routes starting with `10.` (with report)

### What it does
- Reads an inventory file
- Prompts for credentials securely
- Connects to each Cisco router
- Extracts static routes that start with `10.`
- Writes a TXT log + Excel report

```python
import getpass
import re
from datetime import datetime
from netmiko import ConnectHandler
from netmiko.exceptions import NetmikoTimeoutException, NetmikoAuthenticationException

# Choose ONE inventory method:
# devices = load_devices_txt("devices.txt")
devices = load_devices_csv("devices.csv")
# devices = load_devices_excel("devices.xlsx")

username = input("Username: ")
password = getpass.getpass("Password (hidden): ")

rows = []
log_lines = []

for d in devices:
    host = d["ip"]
    device_type = d.get("device_type", "cisco_ios")

    device = {
        "device_type": device_type,
        "host": host,
        "username": username,
        "password": password,
    }

    try:
        conn = ConnectHandler(**device)

        # Retrieve static routes from running config (IOS style)
        raw = conn.send_command("show running-config | section ^ip route")
        conn.disconnect()

        routes_10 = []
        for line in raw.splitlines():
            # Handles: ip route 10.x.x.x <mask> <nexthop>
            m = re.match(r"^ip route\s+(10\.\d+\.\d+\.\d+)\s+(\S+)\s+(\S+)", line.strip())
            if m:
                prefix, mask, nh = m.groups()
                routes_10.append(f"{prefix} {mask} -> {nh}")

        rows.append({
            "hostname": d.get("hostname", host),
            "ip": host,
            "success": True,
            "routes_10_count": len(routes_10),
            "routes_10": " ; ".join(routes_10),
            "error": "",
        })

        log_lines.append(f"{host} | OK | routes_10={len(routes_10)}")

    except (NetmikoTimeoutException, NetmikoAuthenticationException) as e:
        rows.append({
            "hostname": d.get("hostname", host),
            "ip": host,
            "success": False,
            "routes_10_count": 0,
            "routes_10": "",
            "error": str(e),
        })
        log_lines.append(f"{host} | FAIL | {e}")

ts = datetime.now().strftime("%Y%m%d_%H%M%S")
write_txt_log(f"routes10_{ts}.txt", log_lines)
write_excel_report(f"routes10_{ts}.xlsx", rows)

print("Done. Outputs created.")
```

---

## 4) Script B — Cisco: collect facts (hostname, IOS version, uptime)

### What it does
- Connects to every router
- Collects key facts from `show version`
- Writes TXT + Excel output

```python
import getpass
import re
from datetime import datetime
from netmiko import ConnectHandler
from netmiko.exceptions import NetmikoTimeoutException, NetmikoAuthenticationException

devices = load_devices_csv("devices.csv")

username = input("Username: ")
password = getpass.getpass("Password (hidden): ")

rows = []
log_lines = []

for d in devices:
    host = d["ip"]
    device = {
        "device_type": d.get("device_type", "cisco_ios"),
        "host": host,
        "username": username,
        "password": password,
    }

    try:
        conn = ConnectHandler(**device)

        hostname_out = conn.send_command("show run | i ^hostname")
        show_ver = conn.send_command("show version")

        conn.disconnect()

        hostname = hostname_out.replace("hostname", "").strip() if hostname_out else d.get("hostname", host)

        ver_match = re.search(r"Version\s+([\w\.\(\)\-]+)", show_ver)
        ios_version = ver_match.group(1) if ver_match else "UNKNOWN"

        uptime_match = re.search(r"uptime is (.+)", show_ver)
        uptime = uptime_match.group(1).strip() if uptime_match else "UNKNOWN"

        rows.append({
            "hostname": hostname,
            "ip": host,
            "ios_version": ios_version,
            "uptime": uptime,
            "success": True,
            "error": "",
        })
        log_lines.append(f"{host} | OK | {ios_version} | {uptime}")

    except (NetmikoTimeoutException, NetmikoAuthenticationException) as e:
        rows.append({
            "hostname": d.get("hostname", host),
            "ip": host,
            "ios_version": "",
            "uptime": "",
            "success": False,
            "error": str(e),
        })
        log_lines.append(f"{host} | FAIL | {e}")

ts = datetime.now().strftime("%Y%m%d_%H%M%S")
write_txt_log(f"facts_{ts}.txt", log_lines)
write_excel_report(f"facts_{ts}.xlsx", rows)

print("Done. Outputs created.")
```

---

## 5) Script C — Excel inventory → run ANY command per device → export report

### Inventory example (devices.xlsx)
Columns:
- hostname
- ip
- device_type
- site
- command

```python
import getpass
from datetime import datetime
from netmiko import ConnectHandler
from netmiko.exceptions import NetmikoTimeoutException, NetmikoAuthenticationException

devices = load_devices_excel("devices.xlsx")

username = input("Username: ")
password = getpass.getpass("Password (hidden): ")

rows = []
log_lines = []

for d in devices:
    host = d["ip"]
    cmd = d.get("command", "show ip interface brief")

    device = {
        "device_type": d.get("device_type", "cisco_ios"),
        "host": host,
        "username": username,
        "password": password,
    }

    try:
        conn = ConnectHandler(**device)
        out = conn.send_command(cmd)
        conn.disconnect()

        rows.append({
            "hostname": d.get("hostname", host),
            "ip": host,
            "command": cmd,
            "success": True,
            "lines": len(str(out).splitlines()),
            "error": "",
        })
        log_lines.append(f"{host} | OK | lines={len(str(out).splitlines())} | cmd={cmd}")

    except (NetmikoTimeoutException, NetmikoAuthenticationException) as e:
        rows.append({
            "hostname": d.get("hostname", host),
            "ip": host,
            "command": cmd,
            "success": False,
            "lines": 0,
            "error": str(e),
        })
        log_lines.append(f"{host} | FAIL | {e}")

ts = datetime.now().strftime("%Y%m%d_%H%M%S")
write_txt_log(f"command_{ts}.txt", log_lines)
write_excel_report(f"command_{ts}.xlsx", rows)

print("Done. Outputs created.")
```

---

## 6) Script D — Palo Alto: retrieve security rule names via API (secure input)

### What it does
- Asks for firewall host + API key securely
- Retrieves security rules (names) from vsys1
- Saves TXT log + Excel report

> Notes:
> - API key is a secret. Do not hardcode.
> - XPaths can vary between firewall/panorama and vsys.

```python
import getpass
import urllib.parse
import xml.etree.ElementTree as ET
from datetime import datetime
import requests

requests.packages.urllib3.disable_warnings()

fw_host = input("Firewall URL (ex: https://192.0.2.10): ").strip()
api_key = getpass.getpass("PAN-OS API key (hidden): ").strip()

xpath_rules = "/config/devices/entry/vsys/entry[@name='vsys1']/rulebase/security/rules"

params = {
    "type": "config",
    "action": "get",
    "key": api_key,
    "xpath": xpath_rules,
}

url = f"{fw_host}/api/?{urllib.parse.urlencode(params)}"
resp = requests.get(url, verify=False, timeout=30)
resp.raise_for_status()

root = ET.fromstring(resp.text)
rule_entries = root.findall(".//entry")
rule_names = [e.attrib.get("name", "UNKNOWN") for e in rule_entries]

rows = [{"rule_name": n} for n in rule_names]
log_lines = [f"RULE | {n}" for n in rule_names]

ts = datetime.now().strftime("%Y%m%d_%H%M%S")
write_txt_log(f"palo_rules_{ts}.txt", log_lines)
write_excel_report(f"palo_rules_{ts}.xlsx", rows)

print("Done. Outputs created.")
```

---

## 7) Script E — Concurrency: get facts from ALL routers fast (safe structure)

### What it does
- Reads inventory
- Prompts creds securely
- Runs `show version` on all routers using threads
- Extracts uptime
- Creates TXT + Excel outputs

```python
import getpass
import re
from datetime import datetime
from concurrent.futures import ThreadPoolExecutor, as_completed
from netmiko import ConnectHandler
from netmiko.exceptions import NetmikoTimeoutException, NetmikoAuthenticationException

devices = load_devices_csv("devices.csv")

username = input("Username: ")
password = getpass.getpass("Password (hidden): ")

def worker(d: dict) -> dict:
    host = d["ip"]
    device = {
        "device_type": d.get("device_type", "cisco_ios"),
        "host": host,
        "username": username,
        "password": password,
    }

    try:
        conn = ConnectHandler(**device)
        show_ver = conn.send_command("show version")
        conn.disconnect()

        m = re.search(r"uptime is (.+)", show_ver)
        uptime = m.group(1).strip() if m else "UNKNOWN"

        return {"hostname": d.get("hostname", host), "ip": host, "uptime": uptime, "success": True, "error": ""}

    except (NetmikoTimeoutException, NetmikoAuthenticationException) as e:
        return {"hostname": d.get("hostname", host), "ip": host, "uptime": "", "success": False, "error": str(e)}

results = []
max_workers = 10  # Tune carefully

with ThreadPoolExecutor(max_workers=max_workers) as pool:
    futures = [pool.submit(worker, d) for d in devices]
    for f in as_completed(futures):
        results.append(f.result())

log_lines = []
for r in results:
    if r["success"]:
        log_lines.append(f'{r["ip"]} | OK | uptime={r["uptime"]}')
    else:
        log_lines.append(f'{r["ip"]} | FAIL | {r["error"]}')

ts = datetime.now().strftime("%Y%m%d_%H%M%S")
write_txt_log(f"uptime_concurrent_{ts}.txt", log_lines)
write_excel_report(f"uptime_concurrent_{ts}.xlsx", results)

print("Done. Outputs created.")
```

---

## Read please the final thoughts, congrats if you arrived untill her and followed from the start.