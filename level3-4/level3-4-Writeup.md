````markdown
# Bandit Level 3 → Level 4 – Writeup

## Level Goal

The password for the next level is stored in a hidden file inside the `inhere` directory.

---

## Connection

From your local machine:

```bash
ssh bandit3@bandit.labs.overthewire.org -p 2220
```
````

Log in with the password obtained from Level 2 → Level 3.

---

## Solution Overview

1. List the home directory and identify `inhere`:

   ```bash
   ls -lah
   ```

   You should see:

   ```text
   drwxr-xr-x   2 root root 4.0K Oct 14 09:26 inhere
   ```

2. Inspect the contents of `inhere`, including hidden files:

   ```bash
   ls -lah inhere
   ```

   Output will reveal a hidden-looking file:

   ```text
   -rw-r----- 1 bandit4 bandit3 33 Oct 14 09:26 ...Hiding-From-You
   ```

   Note:

   - The file name starts with **three dots** (`...`), not just one.
   - It begins with a dot, so it is hidden from a normal `ls`.

3. Move into the directory and read the file:

   ```bash
   cd inhere
   ls -lah          # to verify the file again
   cat ...Hiding-From-You
   ```

   This prints the password for `bandit4` (redacted in this writeup).

---

## Moving to Level 4

Back on your local machine:

```bash
ssh bandit4@bandit.labs.overthewire.org -p 2220
```

Use the retrieved password (do **not** commit it to the repository).

---

## Key Takeaways

- Hidden files start with `.` and require `ls -a` (or `ls -lah`) to be visible.
- File names must be matched exactly; `...Hiding-From-You` is different from `.Hiding-From-You`.

```

```
