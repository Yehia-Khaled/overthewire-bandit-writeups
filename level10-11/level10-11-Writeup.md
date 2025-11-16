````markdown
# Bandit Level 10 → Level 11 – Writeup

## Level Goal

The password for the next level is stored in the file `data.txt`, which contains **base64-encoded** data.

Your objective: **decode the base64 payload**, extract the password token, and use it to pivot to the next level.

---

## Connect to the Level

From your local machine:

```bash
ssh bandit10@bandit.labs.overthewire.org -p 2220
````

Use the password obtained from Level 9 → 10.

---

## Initial Recon

Once authenticated:

```bash
ls
file data.txt
head data.txt
```

Sample output:

```bash
bandit10@bandit:~$ ls
data.txt
bandit10@bandit:~$ file data.txt
data.txt: ASCII text
bandit10@bandit:~$ head data.txt
VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg==
```

Observations:

* `file` reports **ASCII text**, not “binary data”.
* The content looks like a typical **base64 blob** (A–Z, a–z, 0–9, `+`, `/`, and `=` padding).

The level description explicitly says: *“contains base64 encoded data”* – so we treat the file as a base64 container.

---

## Execution Plan

1. **Confirm / trust the encoding hint**
   The challenge states base64; no need to over-engineer detection.

2. **Decode the file using the `base64` utility**
   Use `base64 -d` (decode mode) against `data.txt`.

3. **Parse the decoded sentence**
   You expect a sentence like:

   ```text
   The password is <some-long-string>
   ```

   The token after `is` is the **actual password**.

---

## Step-by-Step Solution

### 1. View the raw encoded data (optional)

```bash
cat data.txt
```

Example:

```bash
bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg==
```

This confirms a single-line base64 string.

---

### 2. Decode the base64 content

You can decode either directly from the file or via a pipe. Both are functionally equivalent for this use case.

**Option A – Direct decode:**

```bash
base64 -d data.txt
```

**Option B – Using `cat` + pipe:**

```bash
cat data.txt | base64 -d
```

Output will be a human-readable sentence:

```text
The password is <REDACTED_PASSWORD>
```

* Replace `<REDACTED_PASSWORD>` with the actual token shown in your terminal.
* For version control hygiene, **do not commit the real password** in this file.

---

### 3. Use the password to log in to the next level

From your local machine:

```bash
ssh bandit11@bandit.labs.overthewire.org -p 2220
```

When prompted, paste **only** the password token (without `The password is`, and without extra spaces or newlines).

---

## Common Pitfalls / Gotchas

* **Trailing whitespace / newline**
  When copying the password, ensure you don’t include extra spaces or line breaks. If login fails, re-copy the token carefully.

* **Wrong user**
  You should authenticate as `bandit11` (the **next** level), not `bandit10`.

* **Overcomplicating the challenge**
  This level is purely about base64 decoding. No need for `grep`, `strings`, or crypto tooling.

---

## Reusable Pattern for Base64 Levels

Whenever a Bandit level (or any CTF task) states “data is base64 encoded”:

```bash
# 1) Inspect (optional)
file data.txt
head data.txt

# 2) Decode
base64 -d data.txt

# 3) Extract the “The password is XXXXX” token
# 4) Use XXXXX as the password or flag
```

This creates a repeatable decoding workflow you can reapply across future challenges and everyday troubleshooting.

```
```
