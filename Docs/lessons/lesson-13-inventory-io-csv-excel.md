## Lesson 13 — Inventory & I/O: TXT / CSV / Excel

## Why I/O matters

Network automation always starts with **inventory**:
- device list
- credentials (never hardcode in files)
- platform / vendor
- site / role

You also need outputs:
- clean terminal output
- CSV/Excel report
- logs for troubleshooting

---

## Option 1: TXT inventory (simple)

`devices.txt` example (one IP per line):
```
10.0.0.1
10.0.0.2
10.0.0.3
```

Read it:

```py
with open("devices.txt", "r") as f:
    devices = [line.strip() for line in f if line.strip()]

print(devices)
```

Pitfall:
- empty lines
- spaces at the end → use `.strip()`

---

## Option 2: CSV inventory (best simple standard)

`devices.csv` example:
```csv
hostname,ip,platform,site
EDGE-RBA-01,10.0.0.1,cisco_ios,Rabat
EDGE-CAS-01,10.0.0.2,cisco_ios,Casablanca
```

Read it (built-in `csv`):

```py
import csv

devices = []
with open("devices.csv", newline="") as f:
    reader = csv.DictReader(f)
    for row in reader:
        devices.append(row)

print(devices[0])
```

Pitfalls:
- column names must match exactly
- everything is read as **string**

---

## Option 3: Excel inventory (when business wants .xlsx)

Use **openpyxl** or **pandas**.

Example with pandas (fast):

```py
import pandas as pd

df = pd.read_excel("devices.xlsx")
print(df.head())
```

Write a report:

```py
df.to_excel("report.xlsx", index=False)
```

Pitfalls:
- Excel file locked/open in another program
- mixed types in columns

---

## Exercise

Create a CSV `devices.csv` with at least 3 rows.  
Then write a script that prints:

```
<hostname> | <ip> | <site>
```

Hint:
- use `csv.DictReader`
- use `print(..., sep=" | ")`
