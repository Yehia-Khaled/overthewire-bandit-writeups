# Bandit Level 0 → Level 1 – Instructions

## 1. Level Overview

This level focuses on the most fundamental Unix workflow:

1. Access a remote system where you are already logged in as **bandit0**.
2. Locate a specific file (`readme`) in your home directory.
3. Extract the contents of that file to obtain the password for **bandit1**.
4. Use the retrieved password to log into the next level via SSH on port **2220**.

The emphasis is on basic file discovery and inspection in a Unix-like shell.

---

## 2. Goal Statement

> **Goal:** Retrieve the password for user `bandit1` from a file named `readme` located in the home directory of `bandit0`, then use that password to log into `bandit1` on `bandit.labs.overthewire.org` (port `2220`).

---

## 3. Prerequisites

Before starting this level, you should already have:

- A working **SSH client** (e.g., OpenSSH on Linux/macOS, Git Bash on Windows).
- Network connectivity that allows outbound SSH to:
  - **Host:** `bandit.labs.overthewire.org`
  - **Port:** `2220`
- Valid credentials for **bandit0**:
  - Username: `bandit0`
  - Password: `bandit0` (initially; may change over time per game rules).

You should also know how to:

- Open a terminal / shell on your local machine.
- Run basic commands and read their output.

---

## 4. Relevant Commands

You may need some or all of the following commands in this level and future ones:

- `ls` – list directory contents.
- `cd` – change directory.
- `cat` – print file contents to standard output.
- `file` – identify file type.
- `du` – estimate file/directory disk usage.
- `find` – search for files and directories.

For more detailed usage, refer to man pages on a Unix-like system:

```bash
man ls
man cat
man find
```
