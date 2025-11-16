````markdown
# Bandit Level 4 → Level 5 – Writeup

## Level Goal

The password for the next level is stored in the **only human-readable** file inside the `inhere` directory.

---

## Connection

From your local machine:

```bash
ssh bandit4@bandit.labs.overthewire.org -p 2220
```
````

Authenticate using the password from Level 3 → Level 4.

---

## Solution Overview

1. Locate the `inhere` directory:

   ```bash
   ls
   ```

   You should see:

   ```text
   inhere
   ```

2. Move into the directory:

   ```bash
   cd inhere
   ls
   ```

   This lists multiple files named like `-file00`, `-file01`, …, `-file09`.
   Note: they all start with `-`, which can be parsed as options by some commands.

3. Identify the only human-readable file using `file`:

   ```bash
   file ./*
   ```

   Most entries will show some form of “data” or non-text content.
   One of them (e.g., `./-file07`) will be reported as **ASCII text**.

4. Read the password from the ASCII text file:

   ```bash
   cat ./-file07
   ```

   This prints the password for `bandit5` (redacted in this writeup).

   Using `./-file07` avoids `cat` treating `-file07` as an option.

---

## Moving to Level 5

Back on your local machine:

```bash
ssh bandit5@bandit.labs.overthewire.org -p 2220
```

Use the retrieved password (do **not** commit it to the repository).

---

## Key Takeaways

- Use `file` to quickly distinguish human-readable text from binary or arbitrary data.
- When filenames start with `-`, prefix them with `./` (e.g., `./-file07`) or use `--` to avoid option parsing issues.

```

```
