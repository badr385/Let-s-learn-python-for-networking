## Lesson 14 — Connect to a Device & Retrieve Common Information

## What you are about to do

You will learn:
- how a script connects to a device using a library (example: Netmiko)
- how to run common show commands
- how to store results cleanly

---

## Library example: Netmiko (SSH to network devices)

Install:
```bash
pip install netmiko
```

Basic pattern:
- define device parameters
- connect
- run commands
- disconnect

---

## Example: collect hostname, version, interfaces (Cisco IOS-like)

```py
from netmiko import ConnectHandler

device = {
    "device_type": "cisco_ios",
    "host": "10.0.0.1",
    "username": "admin",
    "password": "YOUR_PASSWORD",
}

conn = ConnectHandler(**device)

hostname = conn.send_command("show run | i hostname")
version = conn.send_command("show version | i Version")
interfaces = conn.send_command("show ip interface brief")

conn.disconnect()

print(hostname)
print(version)
print(interfaces)
```

Pitfalls:
- wrong device_type
- SSH blocked / ACL
- credentials wrong
- command differs by platform

---

## Make output readable (important)

Instead of printing huge walls, print a summary:

```py
print("=== SUMMARY ===")
print(hostname.strip())
print(version.strip())
print("Interfaces lines:", len(interfaces.splitlines()))
```

---

## Exercise (even without a device)

Take a long text output (multi-line string) and:
- count lines
- check if a keyword exists (like "down")

Starter:

```py
output = \"\"\"Gi0/0 up
Gi0/1 down
Gi0/2 up
\"\"\"

# your checks
```
