---

## 2) `level0-1-Writeups.md` (full write-up)

```markdown
# Bandit Level 0 → Level 1 – Personal Write-up

## 1. Executive Summary

**Objective**  
Retrieve the password for the `bandit1` account from the `readme` file in the home directory of `bandit0`, then use that password to establish an SSH session as `bandit1` on port `2220`.

**Outcome**  
Successfully located the `readme` file, extracted the password (not documented here for security reasons), and logged into the `bandit1` account, confirming the transition to the next level.

---

## 2. Context and Environment

- **Platform:** OverTheWire Bandit
- **Current Level:** `bandit0`
- **Next Level:** `bandit1`
- **Remote Host:** `bandit.labs.overthewire.org`
- **SSH Port:** `2220`
- **Client Environment:**
  - Local OS: `<your OS here, e.g., Windows 11>`
  - Terminal: `<e.g., Git Bash / Windows Terminal>`
  - Network: outbound SSH allowed to port `2220`

---

## 3. High-Level Approach

1. Reuse the SSH pattern from Level 0 to log in as `bandit0`.
2. Treat the home directory as the primary search scope for the `readme` file.
3. Use simple enumeration (`ls`) to find candidate files.
4. Use a text-inspection command (`cat` / `less`) to reveal the password.
5. Store the password securely outside version control.
6. Use the password to authenticate as `bandit1` on the same host/port.
7. Confirm successful login and capture learnings for future levels.

---

## 4. Step-by-Step Execution

bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ less readme

### 4.1. Log in as `bandit0`

From my local machine:

```bash
ssh -p 2220 bandit0@bandit.labs.overthewire.org
```
