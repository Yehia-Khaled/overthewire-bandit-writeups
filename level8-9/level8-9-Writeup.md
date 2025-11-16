````markdown
# Bandit Level 8 → Level 9 – Writeup

## Level Goal

The password for the next level is stored in `data.txt` and is the **only line that appears exactly once** in the file. Every other line is duplicated many times.

---

## Connection

From your local machine:

```bash
ssh bandit8@bandit.labs.overthewire.org -p 2220
```
````

Authenticate with the password from Level 7 → Level 8.

---

## Solution Overview

1. Validate that `data.txt` exists in the home directory:

   ```bash
   ls
   ```

   You should see:

   ```text
   data.txt
   ```

2. Use `sort` + `uniq` to identify the **unique** line:

   ```bash
   sort data.txt | uniq -u
   ```

   Explanation:

   - `sort data.txt` groups identical lines together.
   - `uniq -u` outputs only lines that occur **once** in the sorted stream.

   This returns a single line:

   ```text
   <UNIQUE_LINE_HERE>
   ```

   That value is the password for the next level (do **not** commit the actual string to your repository).

3. (Optional) To verify counts for all lines:

   ````bash
   sort data.txt | uniq -c
   ```

   You’ll see most lines with count `10` and exactly one line with count `1`.
   ````

---

## Moving to Level 9

From your local machine:

```bash
ssh bandit9@bandit.labs.overthewire.org -p 2220
```

Use the unique line from `uniq -u` as the password.

---

## Key Takeaways

- Combining `sort` and `uniq` is an efficient pattern for **frequency analysis** in text files.
- `uniq -u` surfaces only lines that appear once; `uniq -c` gives full counts for auditing.
- This pattern is broadly reusable for log analysis, de-duplication, and anomaly detection tasks.

```

```
