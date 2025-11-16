# Bandit Level 1 → Level 2 – Instructions

## Level Goal

> The password for the next level is stored in a file called `-` located in the home directory.

---

## Commands You May Need

- `ls`
- `cd`
- `cat`
- `file`
- `du`
- `find`

---

## Concept: Files Literally Named `-`

On Unix-like systems, a lone `-` is often treated as a **special argument**:

- For many commands, `-` means **“read from stdin / write to stdout”**, not “open a file named `-`”.

Examples:

```bash
cat -
file -
vim -
All of these will try to work with standard input, not the file -.

To work with a real file whose name is -, you must:

Qualify it with a path, e.g. ./- or /home/bandit1/-, or

Use the end-of-options marker --, e.g. cat -- -

Step-by-Step Walkthrough
1. Ensure You’re in the Correct Home Directory
bash
Copy code
bandit1@bandit:~$ cd ~
bandit1@bandit:~$ pwd
/home/bandit1
2. List All Files (Including Hidden)
bash
Copy code
bandit1@bandit:~$ ls -la
total 24
-rw-r----- 1 bandit2 bandit1   33 Oct 14 09:26 -
drwxr-xr-x 2 root    root    4096 Oct 14 09:26 .
drwxr-xr-x 150 root  root    4096 Oct 14 09:29 ..
-rw-r--r-- 1 root    root     220 Mar 31 2024 .bash_logout
-rw-r--r-- 1 root    root    3851 Oct 14 09:19 .bashrc
-rw-r--r-- 1 root    root     807 Mar 31 2024 .profile
Key points:

There is a file literally named -

Size is 33 bytes (consistent with Bandit password length)

Owner is bandit2, group is bandit1

3. What Happens if You Just Run cat -
If you try:

bash
Copy code
bandit1@bandit:~$ cat -
the command is now reading from stdin, so it will wait for input until you interrupt:

bash
Copy code
bandit1@bandit:~$ cat -
^C
Same behavior applies for:

bash
Copy code
bandit1@bandit:~$ file -
bandit1@bandit:~$ vim -
They treat - as “read from standard input”.

4. Correct Way to Read the Dashed Filename
Use the explicit path to target the file, not stdin:

bash
Copy code
bandit1@bandit:~$ cat /home/bandit1/-
<REDACTED_PASSWORD_OUTPUT>
or:

bash
Copy code
bandit1@bandit:~$ cat ./-
<REDACTED_PASSWORD_OUTPUT>
You will see a 33-character string – that is the password for bandit2.
```
