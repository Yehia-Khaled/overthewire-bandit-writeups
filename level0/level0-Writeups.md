# Bandit Level 0 → Personal Write-up

## 1. Executive Summary

Objective: establish an SSH session to the Bandit wargame using the provided credentials for **bandit0** on port **2220**, confirming connectivity and readiness to proceed to Level 1.

Outcome: successfully connected to `bandit.labs.overthewire.org` via SSH and verified the remote shell prompt as user `bandit0`.

---

## 2. Environment & Access Details

- **Host:** `bandit.labs.overthewire.org`
- **Port:** `2220`
- **Username:** `bandit0`
- **Authentication:** password-based (`bandit0`)
- **Client:** `<your OS / terminal here, e.g., Windows 11 + Git Bash>`

---

## 3. Approach / Reasoning

1. Confirmed that SSH client was installed locally.
2. Reviewed the level instructions to identify:
   - Target hostname
   - Non-standard SSH port (2220 instead of 22)
   - Initial username/password pair
3. Constructed the SSH command with:
   - `-p` to override the default SSH port
   - `<user>@<host>` format for authentication
4. Verified successful login by:
   - Checking the shell prompt (`bandit0@bandit:~$`)
   - Running a simple command such as `whoami` or `ls`
5. Navigated to the online page for **Level 1** to continue.

---

## 4. Commands Used (Sanitized)

whoami
pwd
ls

```bash
ssh -p 2220 bandit0@bandit.labs.overthewire.org
