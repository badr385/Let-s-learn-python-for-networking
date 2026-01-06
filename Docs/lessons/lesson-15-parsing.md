## Lesson 15 — Reading Network Output

## Parsing (basic)

Parsing means extracting information from text.

---

## Searching for patterns

```py
output = "Gi0/0 is up\nGi0/1 is down\n"
if "down" in output:
    print("Some interface is down")
```

---

## Extracting with split (simple)

```py
line = "Gi0/1 is down"
parts = line.split()
iface = parts[0]
status = parts[-1]
print(iface, status)
```

---

## Pitfall

Text is messy:
- extra spaces
- different cases
- different formats

Normalize first:
```py
status = status.strip().lower()
```

---

## Exercise

Given this output:

```py
output = "Gi0/0 up\nGi0/1 down\nGi0/2 up\n"
```

Print only the down interface name.
