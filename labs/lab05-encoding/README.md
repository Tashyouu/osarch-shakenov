# Lab 5 — Encoding forensics

Name: Tamerlan Shakenov · Date: 22.11.2026 · Tools: Ubuntu Server 26.04.1 LTS guest (VMware Workstation Pro) + Git Bash / PowerShell on Windows 10 host

## What I received

- **Excel**: Cyrillic and Kazakh names displayed as mojibake (broken characters).
- **LibreOffice Calc**: import dialog appeared; names were unreadable with the default character set.
- **Terminal (`cat`)**: output showed garbled characters instead of readable Russian and Kazakh names.

## Diagnosis (made before the fix)

- `file` said: `Non-ISO extended-ASCII text, with CRLF line terminators` (evidence/file-before.txt)
- First 16 bytes: `49 44 3b d4 e0 ec e8 eb e8 ff 3b c8 ec ff 3b ce` (evidence/hexdump-before.txt)
- **Encoding**: RK1048 — because single-byte high characters are used, and Kazakh letters (Ә = A3, Қ etc.) decode correctly only with RK1048. Windows-1251 produced wrong characters for Kazakh names.
- **Line ending**: CRLF — because lines end with `0D 0A`
- **BOM**: no — file does not start with `EF BB BF` or `FF FE`
- **Separator**: semicolon (`;`)

In the fixed file I **added a UTF-8 BOM** so that Excel recognises the encoding on double-click. I kept CRLF line endings because Excel and Windows users expect them.

## Fix

```bash
#!/bin/sh
# Lab 5: convert staff-broken.csv from RK1048 to UTF-8
in=evidence/staff-broken.csv
out=evidence/staff-fixed.csv

iconv -f RK1048 -t UTF-8 "$in" > "$out"
printf '\357\273\277' | cat - "$out" > tmp && mv tmp "$out"
```

Script saved as `scripts/fix.sh` and executed with `sh scripts/fix.sh`.

## Verification

- `file` after fix reports: `Unicode text, UTF-8 (with BOM) text, with CRLF line terminators` (evidence/file-after.txt)
- Hex dump starts with `EF BB BF` (BOM) followed by correct UTF-8 sequences for Cyrillic letters.
- Screenshot `evidence/opens-correctly.png` shows the fixed file opening correctly in:
  1. Excel (Online / double-click)
  2. LibreOffice Calc (UTF-8 + semicolon separator)
  3. Terminal (`cat`)

Kazakh letters (Ә, Қ, Ұ, etc.) are visible and correct in all three applications.

## What did not work the first time

First I tried decoding with Windows-1251. Russian names looked almost correct, but Kazakh letters (Әбдірахманова, Қасымов, Нұрлан) were wrong. After comparing both `iconv` outputs I identified the correct encoding as RK1048.

## Evidence

- [evidence/staff-broken.csv](evidence/staff-broken.csv) — original file, never modified
- [evidence/staff-fixed.csv](evidence/staff-fixed.csv) — converted file
- [evidence/file-before.txt](evidence/file-before.txt) — output of `file` before any change
- [evidence/hexdump-before.txt](evidence/hexdump-before.txt) — first bytes in hex
- [evidence/file-after.txt](evidence/file-after.txt) — `file` + hex after conversion
- [evidence/opens-correctly.png](evidence/opens-correctly.png) — Excel + LibreOffice + terminal side-by-side
- [scripts/fix.sh](scripts/fix.sh) — the conversion script used
