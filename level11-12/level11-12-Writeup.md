````markdown
# Bandit Level 11 → Level 12 – Writeup

## Level Goal

The password for the next level is stored in the file `data.txt`, where all lowercase (`a–z`) and uppercase (`A–Z`) letters have been rotated by 13 positions (**ROT13**).

Your objective: **decode the ROT13-encoded string** and extract the password.

---

## Connect to the Level

From your local machine:

```bash
ssh bandit11@bandit.labs.overthewire.org -p 2220
````

Use the password retrieved from the previous level (Level 10 → 11).

---

## Initial Recon

Once logged in:

```bash
ls
file data.txt
cat data.txt
```

Example output:

```bash
bandit11@bandit:~$ ls
data.txt
bandit11@bandit:~$ file data.txt
data.txt: ASCII text
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
```

Key observations:

* `file` confirms this is **ASCII text**, not binary.
* The content clearly looks like a **ROT13-style** string (gibberish but only letters/digits/spaces, no special symbols).
* The level description explicitly calls out **rotated by 13 positions**, i.e. ROT13.

---

## Strategy

1. Use `tr` to apply a ROT13 transformation:

   * Map all letters `A–Z a–z` to their counterparts shifted by 13 positions.
2. Feed `data.txt` into `tr` via input redirection.
3. Read the decoded English sentence and extract the password token.

---

## Decoding with `tr` (ROT13)

From the home directory of `bandit11`:

```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt
```

### Command Breakdown

* `tr 'A-Za-z' 'N-ZA-Mn-za-m'`

  * First set: `A-Za-z` → all upper and lowercase letters.
  * Second set: `N-ZA-Mn-za-m` → same alphabet but rotated by 13 characters.
  * This is a classic ROT13 mapping.

* `< data.txt`

  * Redirects the content of `data.txt` to `tr` as standard input.

Sample output structure:

```text
The password is <REDACTED_PASSWORD>
```

* The substring after `The password is` is the **actual password** for the next level.
* Do **not** commit the real password to version control; keep it out of this writeup.

---

## Logging in to the Next Level

From your local machine, use the decoded password:

```bash
ssh bandit12@bandit.labs.overthewire.org -p 2220
```

When prompted, paste **only** the password token (no leading/trailing spaces, no extra text).

---

## Conceptual Notes: Why This Works

* ROT13 is a **Caesar cipher with a fixed shift of 13**.
* The alphabet has 26 letters, so:

  * Applying ROT13 once → encodes.
  * Applying ROT13 again → decodes back to the original.
* `tr` is a lightweight tool for performing **simple substitution ciphers** like ROT13 directly in the shell.

---

## Reusable Pattern for ROT13 Challenges

For any challenge that specifies “letters have been rotated by 13 positions” or mentions ROT13:

```bash
file data.txt        # confirm it’s text
cat data.txt         # optional preview
tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt
# → Look for: "The password is XXXXX"
```

Use the `XXXXX` token as the password / flag in the next step of the workflow.

```
```
