## Lesson 15 — Run on Multiple Devices (Concurrency) 🚀

## Why concurrency matters

If you have:
- 5 devices → sequential is fine
- 50 devices → sequential is slow
- 500 devices → sequential is pain

Python can run tasks in parallel using **concurrent.futures**.

---

## The safe mental model

One function does one device.  
Then we run that function across a list of devices.

---

## Example: collect `show clock` from many devices (Netmiko + threads)

```py
from concurrent.futures import ThreadPoolExecutor, as_completed
from netmiko import ConnectHandler

def collect_clock(device):
    conn = ConnectHandler(**device)
    out = conn.send_command("show clock")
    conn.disconnect()
    return device["host"], out.strip()

devices = [
    {"device_type": "cisco_ios", "host": "10.0.0.1", "username": "admin", "password": "PWD"},
    {"device_type": "cisco_ios", "host": "10.0.0.2", "username": "admin", "password": "PWD"},
]

results = []
with ThreadPoolExecutor(max_workers=10) as ex:
    futures = [ex.submit(collect_clock, d) for d in devices]
    for f in as_completed(futures):
        results.append(f.result())

for host, clock in results:
    print(host, "=>", clock)
```

Pitfalls:
- too many threads can overload your jump host or devices
- handle exceptions (timeouts/auth failures)

---

## Exercise

Change the function to run:
- `show ip interface brief`
Then print only the number of lines returned per device.
