````markdown
# Bandit Level 7 → Level 8 – Writeup

## Level Goal

The password for the next level is stored in the file `data.txt` in the home directory of `bandit7`, **next to** the word `millionth`.

---

## Connection

From your local machine:

```bash
ssh bandit7@bandit.labs.overthewire.org -p 2220
```
````

Authenticate using the password from Level 6 → Level 7.

---

## Solution Overview

1. Confirm the home directory and presence of `data.txt`:

   ```bash
   pwd
   ls -lah
   ```

   You should see a large `data.txt` (~4.0M) owned by `bandit8:bandit7`:

   ```text
   -rw-r----- 1 bandit8 bandit7 4.0M ... data.txt
   ```

2. The level hint says the password is located **next to** the word `millionth` inside this file.
   Use `grep` to search for the line containing that word:

   ```bash
   grep millionth data.txt
   ```

   This returns a single line similar to:

   ```text
   millionth       <PASSWORD_FOR_BANDIT8>
   ```

   The token after `millionth` is the password for the next level (do **not** commit it to your repo).

---

## Moving to Level 8

From your local machine:

```bash
ssh bandit8@bandit.labs.overthewire.org -p 2220
```

Use the password you retrieved from the `millionth` line.

---

## Key Takeaways

- `grep <pattern> <file>` is the fastest way to locate a specific marker in large text data.
- Focusing on the current user’s accessible files avoids unnecessary permission errors in other home directories.
- Always extract just the secret value for use; don’t store the actual password in public writeups.

```

```
