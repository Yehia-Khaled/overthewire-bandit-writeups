Here’s a streamlined, professional-style version you can drop into your repo:

````markdown
# Bandit Level 2 → Level 3 – Writeup

## Level Goal

The password for the next level is stored in a file named `--spaces in this filename--` in the home directory. The main challenge is reading this file without the shell misinterpreting its name as command-line options.

---

## Connection

From your local machine:

```bash
ssh bandit2@bandit.labs.overthewire.org -p 2220
```
````

Authenticate using the password from the previous level.

---

## Solution Overview

1. Verify location and list the home directory:

   ```bash
   pwd
   ls
   ```

   You should see the target file:

   ```text
   --spaces in this filename--
   ```

2. Confirm file metadata:

   ```bash
   ls -lah
   ```

   This shows the file is readable by `bandit2` and sized like a typical password file (33 bytes).

3. Handle the “tricky” filename:

   - The name **starts with `--`**, which many commands treat as options.
   - It **contains spaces**, which requires quoting or escaping.

   Two reliable patterns to read the file:

   **Option A – Input redirection (used in this level)**

   ```bash
   cat < --spaces\ in\ this\ filename--
   ```

   Here, the filename is used by the shell for redirection, not passed as an argument to `cat`.

   **Option B – End-of-options marker**

   ```bash
   cat -- '--spaces in this filename--'
   ```

   `--` stops option parsing, and the quoted string is treated as the literal filename.

   Either approach prints the password (redacted here) for `bandit3`.

---

## Moving to Level 3

Back on your local machine:

```bash
ssh bandit3@bandit.labs.overthewire.org -p 2220
```

Use the retrieved password (do **not** commit it to the repository).

---

## Key Takeaways

- Use quoting or escaping for filenames with spaces.
- Use `--` to safely handle filenames starting with dashes.
- Never store actual passwords or secrets in public writeups; use placeholders instead.

```

```
