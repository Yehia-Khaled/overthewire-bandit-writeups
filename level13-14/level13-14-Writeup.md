# Bandit Level 13 → Level 14  

## Executive summary

Operational objective: use the **private SSH key** provided by level 13 to authenticate as `bandit14` and retrieve the password file located at `/etc/bandit_pass/bandit14`. This document is a sanitized, publish-ready runbook: it contains the procedural steps, rationale, and troubleshooting guidance without exposing any secret tokens. Treat this as an operational playbook to onboard the workflow and reproduce the task in a safe, auditable manner.

---

## Prerequisites & assumptions

* You have network access to the Bandit lab host and port (typically `bandit.labs.overthewire.org:2220`).
* You possess the private key file issued by level 13 (example filename: `sshkey.private`).
* You have an OpenSSH-compatible client (`ssh`, `scp`) on your workstation.
* You understand basic shell operations and file permission semantics.

---

## Key security policy (non-negotiable)

* **Do not publish** private keys or passphrases. Redact any credentials in public artifacts.
* Apply **least privilege**: lock private keys to user-read-only (`chmod 600`).
* Maintain an audit trail for any steps that touch sensitive artifacts (checksums, timestamps).

---

## High-level workflow (one-line)

1. Secure the private key locally → 2. Use `scp` (if needed) to fetch the key → 3. Lock permissions (`chmod 600`) → 4. `ssh -i <key>` into the host as `bandit14` → 5. Read `/etc/bandit_pass/bandit14` (redact before publishing).

---

## Step-by-step runbook (operational)

> Replace placeholders: `<KEYFILE>` = local private key filename, `<HOST>` = bandit host, `<PORT>` = 2220.

1. **Inspect key metadata (non-invasive)**

```bash
file <KEYFILE>                 # quick file-type sanity check
ssh-keygen -lf <KEYFILE>       # fingerprint & bit-length (detect corruption)
```

2. **Secure file permissions (required by OpenSSH)**

```bash
chmod 600 <KEYFILE>
# verify
ls -l <KEYFILE>
```

3. **Copy the key from remote (if the key is hosted on the prior account)**

```bash
# from your workstation: fetch via SCP (use -P for port)
scp -P <PORT> bandit13@<HOST>:sshkey.private .
# then lock the file as above
chmod 600 sshkey.private
```

4. **Authenticate using the key**

```bash
ssh -i <KEYFILE> -p <PORT> bandit14@<HOST>
# once connected:
cat /etc/bandit_pass/bandit14     # do not publish the value
```

5. **Alternative: use ssh-agent (for repeated usage)**

```bash
eval "$(ssh-agent -s)"
ssh-add <KEYFILE>
ssh -p <PORT> bandit14@<HOST>
```

---

## Operational notes & rationale

* `chmod 600` is enforced by `sshd` for private key files; overly permissive ACLs cause the client to refuse or the server to reject the key.
* Use `ssh -vvv` to collect negotiation logs when diagnosing `Permission denied (publickey)`. These logs show which key algorithms and identities are being offered.
* Use `ssh-keygen -y -f <KEYFILE> > <KEYFILE>.pub` to derive the public key if you need to validate server-side authorized keys (do **not** expose the private key).

---

## Troubleshooting playbook (symptom → triage → remediation)

* **Symptom:** `The authenticity of host '...' can't be established`
  **Triage:** Host key unknown.
  **Remediation:** For lab usage, accept (`yes`) to add to `~/.ssh/known_hosts`. In production, validate fingerprint out-of-band before accepting.

* **Symptom:** `Permission denied (publickey).`
  **Triage:** Wrong username, corrupted key, or permission issue.
  **Remediation:** Confirm `bandit14` username, run `ssh-keygen -lf <KEYFILE>`, ensure `chmod 600`, try `ssh -vvv -i <KEYFILE> ...` to inspect the offer/accept flow.

* **Symptom:** Immediate `Connection closed` after banner
  **Triage:** Server-side policy or ephemeral closure.
  **Remediation:** Retry, check network connectivity (`nc -vz <HOST> <PORT>`), verify key integrity, or consult verbose logs.

* **Symptom:** `scp` errors on Windows/PowerShell (usage/arguments)
  **Triage:** Windows scp wrapper can differ; ensure correct ordering of arguments and `-P` for port.
  **Remediation:** Use native OpenSSH or WSL; example: `scp -P 2220 user@host:file .`

---

## Audit & hygiene checklist (post-session)

* Do **not** commit the key into source control. Add it to `.gitignore`.
* Remove the key from ephemeral systems when no longer needed: `shred`/`rm -f` (per your organizational policy).
* Record a sanitized note: `Retrieved bandit14 credential and validated access. Secret redacted in public records.`

---

## Minimal sanitized example (copy-friendly)

```bash
# 1) Fetch key if needed (from bandit13)
scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .

# 2) Lock it down
chmod 600 sshkey.private

# 3) Use it to log in
ssh -i sshkey.private -p 2220 bandit14@bandit.labs.overthewire.org

# 4) Read the pass file (do not share)
cat /etc/bandit_pass/bandit14
# >> The password is <PASSWORD_REDACTED>
```

---

## Deliverable metadata (recommended for GitHub)

* **Filename:** `level13-14-Runbook.md`
* **Commit message:** `docs: add Level13→14 SSH key runbook (sanitized)`
* **README blurb:** `Key-based SSH walkthrough for Bandit Level 13→14. No credentials included.`

---

## Reflection prompts (for learning & review)

* Why does OpenSSH require strict permissions on private keys? (security posture)
* When would you prefer `ssh-agent` over `ssh -i`? (usability vs. isolation)
* How would you validate a remote host key out-of-band in a production environment? (operational controls)

 
