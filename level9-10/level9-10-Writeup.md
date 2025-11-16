````markdown
# Bandit Level 9 → Level 10 – Writeup

## Level Goal

The password for the next level is stored in `data.txt` in **one of the few human-readable strings**, and that string is **preceded by several `=` characters**.

In other words: `data.txt` is mostly binary noise, with a few readable fragments, and exactly one of those fragments contains the password.

---

## Connect to the Level

From your local machine:

```bash
ssh bandit9@bandit.labs.overthewire.org -p 2220
```
````

Use the password from Level 8 → 9.

---

## Initial Recon

Once logged in:

```bash
ls -lah
```

Sample output:

```text
total 40K
drwxr-xr-x   2 root     root    4.0K Oct 14 09:26 .
drwxr-xr-x 150 root     root    4.0K Oct 14 09:29 ..
-rw-r--r--   1 root     root     220 Mar 31  2024 .bash_logout
-rw-r--r--   1 root     root    3.8K Oct 14 09:19 .bashrc
-rw-r-----   1 bandit10 bandit9  19K Oct 14 09:26 data.txt
-rw-r--r--   1 root     root     807 Mar 31  2024 .profile
```

Check the file type:

```bash
file data.txt
```

This will report something along the lines of “data” / generic binary, not a clean text file. That’s your indicator that traditional tools like `cat` or `sort` will produce a lot of garbage output.

---

## Strategy

The problem statement gives two critical hints:

1. **“human-readable strings”** → use `strings` to extract only printable text from the binary.
2. **“preceded by several ‘=’ characters”** → use `grep` to filter on `=` sequences.

So the game plan is:

1. Extract human-readable segments from `data.txt`.
2. Filter those segments for lines containing multiple `=` characters.
3. Identify which of those lines is the password (the random-looking token).

---

## Step-by-Step Solution

### 1. Extract human-readable strings

From the home directory of `bandit9`:

```bash
strings data.txt
```

This prints out all printable ASCII sequences found in the binary. The output is still long, but now it’s all readable text.

You can already see the “signal” in this output, but we can tighten it further.

---

### 2. Filter on the `=` hint

The level text explicitly mentions: _“preceded by several ‘=’ characters.”_
We’ll pipeline `strings` through `grep`:

```bash
strings data.txt | grep '===='
```

Typical output structure:

```text
========== the
========== password
========== is
========== <REDACTED_PASSWORD>
```

- The first three lines form the sentence: `the password is`.
- The last line contains the **actual password** (a long random-looking string) after the `==========`.

> 🔒 **Important for GitHub:**
> Do **not** commit the real password value into your repository. Replace it with a placeholder like `<REDACTED_PASSWORD>` or `<PASSWORD_HERE>` in your markdown.

---

## 3. Use the Password to Move On

From your local machine, use the extracted password:

```bash
ssh bandit10@bandit.labs.overthewire.org -p 2220
```

When prompted, paste the long random string you saw after the last `==========`.

---

## Conceptual Takeaways

- When a file is mostly binary but contains “human-readable strings”, `strings` is your go-to tool.
- Hints like “preceded by several `=` characters” are effectively **search filters** — `grep` is perfect for this.
- This level is about **tool chaining and pattern filtering**, not cryptography:

  - `strings` → extract readable data
  - `grep` → narrow down based on the clue

- This pattern is reusable for log analysis, basic forensics, and quick triage of unknown binary artifacts.

```

```
