## Lesson 16 — Push Config on Multiple Devices (Safely) ✅

## The goal

You want to:
- push a small config snippet
- validate with checks
- do it across multiple devices
- keep output clean and trustworthy

---

## The rule: never “blind push”

Always do:
1. pre-check
2. push
3. post-check
4. report

---

## Example: push a description on an interface (Netmiko)

```py
from netmiko import ConnectHandler

device = {
    "device_type": "cisco_ios",
    "host": "10.0.0.1",
    "username": "admin",
    "password": "PWD",
}

commands = [
    "interface GigabitEthernet0/0",
    "description Managed by Python",
]

conn = ConnectHandler(**device)

pre = conn.send_command("show run interface GigabitEthernet0/0 | i description")
out = conn.send_config_set(commands)
post = conn.send_command("show run interface GigabitEthernet0/0 | i description")

conn.save_config()  # for platforms that support it (IOS: write mem / etc. netmiko handles per platform sometimes)
conn.disconnect()

print("PRE:", pre.strip())
print("POST:", post.strip())
```

Pitfalls:
- wrong interface name
- command syntax differs per platform
- saving config differs across platforms

---

## Multi-device safe push (with concurrency + result report)

```py
from concurrent.futures import ThreadPoolExecutor, as_completed
from netmiko import ConnectHandler

COMMANDS = [
    "interface GigabitEthernet0/0",
    "description Managed by Python",
]

def push_and_check(device):
    host = device["host"]
    conn = ConnectHandler(**device)

    pre = conn.send_command("show run interface GigabitEthernet0/0 | i description")
    conn.send_config_set(COMMANDS)
    post = conn.send_command("show run interface GigabitEthernet0/0 | i description")

    conn.disconnect()

    ok = "Managed by Python" in post
    return {
        "host": host,
        "pre": pre.strip(),
        "post": post.strip(),
        "ok": ok,
    }

devices = [
    {"device_type": "cisco_ios", "host": "10.0.0.1", "username": "admin", "password": "PWD"},
    {"device_type": "cisco_ios", "host": "10.0.0.2", "username": "admin", "password": "PWD"},
]

results = []
with ThreadPoolExecutor(max_workers=10) as ex:
    futures = [ex.submit(push_and_check, d) for d in devices]
    for f in as_completed(futures):
        results.append(f.result())

print("=== REPORT ===")
for r in results:
    status = "OK" if r["ok"] else "FAIL"
    print(r["host"], "|", status, "|", r["post"])
```

---

## Why people need Python (the real value)

With Python you can:
- do the same work on 100 devices in minutes
- reduce human errors
- produce consistent reports
- build repeatable, testable procedures

That’s why Python fits networking so well.

---

## Exercise

Modify the script:
- change the interface name to a variable `iface`
- change the description to include the hostname
- print a final summary:
  - total devices
  - number OK
  - number FAIL
