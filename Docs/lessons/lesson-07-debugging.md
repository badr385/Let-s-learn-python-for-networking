## Lesson 7 — Reading Errors & Debugging

## What you are about to do

In this lesson, you will:
- understand what error messages mean
- recognize the most common beginner errors
- debug calmly (like a network engineer)
- learn a simple debugging process

---

## Errors are not failures

Errors are feedback.

They tell you:
- what went wrong
- where it went wrong
- what Python expected

Your job is not to panic.  
Your job is to read.

---

## The 5 most common errors

### 1) SyntaxError (Python cannot parse the code)
Example:
```py
print("hello"
```

### 2) IndentationError (blocks not aligned)
Example:
```py
if True:
print("x")
```

### 3) NameError (variable not defined)
Example:
```py
print(hostname)  # hostname not created yet
```

### 4) TypeError (wrong type for an operation)
Example:
```py
print("10" + 1)
```

### 5) KeyError (dictionary key missing)
Example:
```py
d = {"a": 1}
print(d["b"])
```

---

## Read stack traces calmly

A stack trace usually shows:
- file
- line number
- error type
- message

Rule:
- start from the bottom line (the error)
- then go to the line number in your code

---

## Debugging mindset (network-style)

In networking, you do:
- observe
- isolate
- test
- validate

In Python, do the same.

---

## A simple debugging process

When code fails:

1. Read the error type (SyntaxError? NameError?)
2. Go to the exact line
3. Print variables around it
4. Change one thing
5. Run again

---

## Debug tool #1: print

Yes, print is a debugging tool.

```py
status = "UP"
print("DEBUG status =", status)
```

---

## Exercise

This code is broken. Fix it.

```py
iface = {"name": "eth0", "up": "True"}
if iface["up"] == True:
    print(iface["name"], "is UP")
```

Hints:
- `"True"` is text, not boolean
- make the condition correct
