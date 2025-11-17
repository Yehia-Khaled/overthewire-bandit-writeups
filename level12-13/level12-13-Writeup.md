# Bandit Level 12 → Level 13 — Public-friendly Writeup (Password Redacted)

## Objective

Recover the next-level credential from `/home/bandit12/data.txt`, which is a **hexdump** of a file containing multiple nested compressed containers. This public version documents the **methodology** and **commands** but **does not include** the actual password — that token has been redacted for security and responsible disclosure.

---

## Executive summary

We reconstruct the original binary from the hexdump, then run a detection-and-decompress lifecycle until plaintext is revealed. For public distribution the final credential is replaced with `<PASSWORD_REDACTED>`. The content below is suitable for publishing (no secrets, fully instructional).

---

## Tools you will use

`awk`, `xxd`, `file`, `xxd`, `gzip`/`gunzip`, `bunzip2`, `tar`, `unzip`, `strings`, `sed`, `mktemp`, `cp`, `mv`, `sha256sum`

---

## High-level process (playbook)

1. **Isolate** — operate in a temporary workspace so original artifacts remain immutable.
2. **Reconstruct** — convert the hexdump back to raw bytes using a token-safe extractor (avoid brittle column cuts).
3. **Detect** — use `file` and `xxd` to identify container/magic bytes for the current layer.
4. **Unpack** — apply the appropriate decompressor or extract from archives.
5. **Iterate** — repeat detect → unpack until the result is readable text.
6. **Extract & redact** — read the plaintext; if publishing, redact the credential.
7. **Clean** — remove workspace and any sensitive intermediate artifacts.

---

## Reproducible commands (safe to publish)

### 1) Create an isolated workspace

```bash
WORKDIR=$(mktemp -d /tmp/bandit12-XXXXXX)
cd "$WORKDIR"
cp /home/bandit12/data.txt .
```

### 2) Robust hexdump → binary conversion

Use a token-regex approach to avoid dropping bytes or depending on exact column counts:

```bash
awk '
{
  sub(/^[^:]*: */, "")              # remove offset & colon
  for (i=1;i<=NF;i++) {
    if ($i ~ /^[0-9a-fA-F]{2}$/)   # accept only 2-digit hex tokens
      printf "%s", $i
    else
      break                        # stop at ASCII column
  }
}
END { printf "\n" }' data.txt | xxd -r -p > data.bin
```

Validate the reconstruction:

```bash
file data.bin
xxd -g1 -l 16 data.bin
sha256sum data.bin   # optional audit hash
```

### 3) Iterative detection + decompression (manual)

Use `file` to decide the next action, then stream out the decompressed result to a new file:

```bash
# Example pattern (do not run blindly; check `file` first)
file data.bin               # -> e.g., "gzip compressed data"
gzip -dc data.bin > layer1.bin

file layer1.bin             # -> e.g., "bzip2 compressed data"
bunzip2 -c layer1.bin > layer2.bin

file layer2.bin             # -> e.g., "POSIX tar archive"
tar -tf layer2.bin          # list archive members
tar -xOf layer2.bin member > next.bin
```

Repeat until `file` reports `ASCII text` or similar. When the final artifact is text, inspect it with:

```bash
strings final.bin | sed -n '1,5p'
sed -n '1p' final.bin
```

**Note (public-safe):** when you see a line like `The password is <token>`, do **not** paste the token into public materials. Replace it with `<PASSWORD_REDACTED>` for documentation.

---

## Automated, audit-friendly loop (publishable template)

Below is an operational template you can run locally; it preserves each layer for audit and stops when plaintext is detected. It does **not** echo the final secret to stdout if you choose to redact.

```bash
#!/bin/bash
CUR="data.bin"
ITER=0
MAX=25

while [ $ITER -lt $MAX ]; do
  echo "=== Iteration $ITER: $CUR ==="
  TYPE=$(file -b "$CUR")
  echo "file: $TYPE"

  case "$TYPE" in
    *gzip*) gzip -dc "$CUR" > next.bin ;;
    *bzip2*) bunzip2 -c "$CUR" > next.bin ;;
    *XZ*|*xz*) unxz -c "$CUR" > next.bin ;;
    *zip*) unzip -p "$CUR" > next.bin ;;
    *tar*) mkdir -p unpack && tar -xf "$CUR" -C unpack && echo "extracted to unpack/"; ls -l unpack && break ;;
    *text*|*ASCII*) echo "TEXT FOUND"; sed -n '1p' "$CUR"; break ;;
    *) echo "Unhandled type: $TYPE"; xxd -l 64 "$CUR"; strings "$CUR" | head -n 40; break ;;
  esac

  mv -f "$CUR" "layer_${ITER}.bin"
  mv -f next.bin "data_layer_${ITER}.bin"
  CUR="data_layer_${ITER}.bin"
  ITER=$((ITER+1))
done
```

**Operational note:** Inspect `layer_*.bin` artifacts to understand the container chain. Keep an audit trail (`sha256sum layer_*.bin`) if you need reproducibility.

---

## Example final line (redacted for publication)

When you complete the unpacking process you will see a plaintext line similar to:

```
The password is <PASSWORD_REDACTED>
```

Replace `<PASSWORD_REDACTED>` with the real token **only** in your private notes or when logging in to the next level. Do not publish the actual token.

---

## Common pitfalls & mitigations (public-safe)

- **Fragile hex parsing** — avoid fixed-column `cut` approaches; use the regex-based `awk` shown above.
- **Truncated decompression** — if decompression reports EOF or CRC errors, re-check the reconstructed `data.bin` and its checksum.
- **Wrong decompressor** — always `file` before running a decompressor.
- **Publishing secrets** — scrub tokens from any public writeup; use placeholders like `<PASSWORD_REDACTED>`.

---

## Hygiene & cleanup

After you validate the result locally, remove the temp workspace to avoid leaving sensitive intermediate files on disk:

```bash
cd ~
rm -rf "$WORKDIR"
```

---

## Closing (policy + responsible disclosure)

This document is designed for public consumption: it provides a runnable, repeatable methodology while **intentionally omitting** the actual credential. For any public writeup derived from CTF-style exercises, always redact secrets and avoid publishing exploitable credentials. If you want, I can render this text into a polished `README.md` suitable for a public GitHub repository (with placeholders and a short FAQ on redaction practices).
