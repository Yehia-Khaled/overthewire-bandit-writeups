````markdown
# Bandit Level 5 → Level 6 – Writeup

## Level Goal

The password for the next level is stored in a file somewhere under the `inhere` directory. The file must be:

- Human-readable
- Exactly **1033 bytes** in size
- **Not executable**

---

## Connection

From your local machine:

```bash
ssh bandit5@bandit.labs.overthewire.org -p 2220
```
````

Authenticate with the password from Level 4 → Level 5.

---

## Solution Overview

1. Move into the `inhere` directory:

   ```bash
   cd ~/inhere
   pwd
   ```

   You should be in:

   ```text
   /home/bandit5/inhere
   ```

   A quick listing shows multiple `maybehereXX` subdirectories:

   ```bash
   ls -lah -b
   ```

2. Instead of manually checking every file, use `find` to apply all constraints in one query:

   ```bash
   find . -type f -size 1033c ! -executable -ls
   ```

   This returns a single match, for example:

   ```text
   ./maybehere07/.file2
   ```

   Breakdown:

   - `-type f` → regular files only
   - `-size 1033c` → exactly 1033 bytes (in bytes, not blocks)
   - `! -executable` → file is not executable

3. Read the content of the matching file to retrieve the password:

   ```bash
   cat ./maybehere07/.file2
   ```

   The output is the password for `bandit6` (redacted in this writeup).

---

## Moving to Level 6

From your local machine:

```bash
ssh bandit6@bandit.labs.overthewire.org -p 2220
```

Use the retrieved password (do **not** commit it to the repository).

---

## Key Takeaways

- `find` is ideal for searching based on multiple attributes (type, size, permissions).
- `size 1033c` uses **bytes**; omitting `c` changes the meaning.
- Automating the search reduces manual error and scales better than brute-force inspection.

```

```
