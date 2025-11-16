````markdown
# Bandit Level 6 → Level 7 – Writeup

## Level Goal

The password for the next level is stored **somewhere on the server** in a file that is:

- Owned by user `bandit7`
- Owned by group `bandit6`
- Exactly **33 bytes** in size

---

## Connection

From your local machine:

```bash
ssh bandit6@bandit.labs.overthewire.org -p 2220
```
````

Authenticate with the password from Level 5 → Level 6.

---

## Solution Overview

1. Start by confirming your location:

   ```bash
   pwd
   ```

   You’ll be in:

   ```text
   /home/bandit6
   ```

2. Use `find` to search the **entire filesystem** for a file that matches all three properties.
   Suppress permission errors to keep the output clean:

   ```bash
   find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
   ```

   This returns:

   ```text
   /var/lib/dpkg/info/bandit7.password
   ```

   Here’s what each flag does:

   - `/` → search from the filesystem root
   - `-user bandit7` → owned by user `bandit7`
   - `-group bandit6` → owned by group `bandit6`
   - `-size 33c` → file size is exactly 33 bytes (`c` = bytes)
   - `2>/dev/null` → hide “Permission denied” noise from stderr

3. Once the file is identified, read it with `cat`:

   ```bash
   cat /var/lib/dpkg/info/bandit7.password
   ```

   This outputs the password for `bandit7` (redacted in this writeup).

---

## Moving to Level 7

Back on your local machine:

```bash
ssh bandit7@bandit.labs.overthewire.org -p 2220
```

Use the retrieved password (do **not** commit it to the repository).

---

## Key Takeaways

- `find` can be used as a precise filter over the whole system (user, group, size).
- Adding `2>/dev/null` keeps search output focused by hiding permission errors.
- Always double-check user/group names in your filters (`bandit7` vs typos like `babdit7`) to avoid empty results.

```

```
