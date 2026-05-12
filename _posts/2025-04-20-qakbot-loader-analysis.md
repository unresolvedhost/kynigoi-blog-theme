---
title: "QakBot Loader: Unpacking and Behavioural Analysis"
date: 2025-04-20 09:00:00 +0800
categories: [Malware Analysis]
tags: [qakbot, loader, unpacking, windows, memory-forensics]
author: kynigoi
description: >
  QakBot continues to evolve its loader mechanisms to evade static
  detection. This post walks through manual unpacking, behavioural
  artefacts in memory, and YARA signatures for hunting across your fleet.
severity: HIGH
mitre:
  - "T1055 — Process Injection"
  - "T1027 — Obfuscated Files or Information"
  - "T1071.001 — Web Protocols"
read_time: 14
featured: true
---

## Overview

QakBot (also QBot, Pinkslipbot) is a modular banking trojan turned
initial access broker. Its loader has been heavily re-engineered since
the 2023 FBI takedown, with new samples observed using XOR-encrypted
shellcode blobs and stomped PE headers to defeat memory scanners.

## Sample metadata

| Field | Value |
|-------|-------|
| SHA-256 | `a1b2c3d4e5f6...` |
| File type | PE32 DLL |
| First seen | 2025-04-15 |
| Packer | Custom XOR + RtlDecompressBuffer |
| C2 protocol | HTTPS over port 443/65400 |

## Unpacking approach

The loader follows a three-stage sequence:

1. **Stage 1** — A benign-looking DLL is side-loaded via `regsvr32.exe`
2. **Stage 2** — XOR decryption loop extracts a shellcode blob into an
   RWX heap region allocated via `VirtualAlloc`
3. **Stage 3** — Shellcode reflectively loads the core QakBot DLL and
   hooks `NtQuerySystemInformation` for process hiding

### Finding the XOR key

```python
# Simple brute-force XOR key recovery (single byte)
with open("stage2.bin", "rb") as f:
    data = f.read()

# MZ header expected at offset 0 after decryption
for key in range(0x00, 0xFF):
    if (data[0] ^ key) == 0x4D and (data[1] ^ key) == 0x5A:
        print(f"[+] XOR key: 0x{key:02X}")
        decrypted = bytes(b ^ key for b in data)
        with open("unpacked.bin", "wb") as out:
            out.write(decrypted)
        break
```

## Behavioural artefacts

Post-injection, QakBot exhibits these consistent behaviours:

- Injects into `wermgr.exe` or `AtBroker.exe`
- Creates a scheduled task under `\Microsoft\Windows\` with a random GUID name
- Beacons every 3–5 minutes with a JA3 fingerprint of `72a589da586844d7f0818ce684948eea`
- Drops encrypted configuration to `%APPDATA%\Microsoft\` as a `.dat` file

## YARA rule

```yara
rule QakBot_Loader_XOR_Stub {
    meta:
        author      = "kynigoi"
        description = "Detects QakBot XOR loader stub"
        date        = "2025-04-20"
        severity    = "high"

    strings:
        $xor_loop   = { 8A 04 0F 30 04 1E 40 3B C2 7C F6 }
        $virtualalloc = "VirtualAlloc" ascii
        $rtldecomp  = "RtlDecompressBuffer" ascii
        $guid_task  = /\\Microsoft\\Windows\\[0-9A-F]{8}-/ wide

    condition:
        uint16(0) == 0x5A4D and
        all of ($xor_loop, $virtualalloc, $rtldecomp) and
        $guid_task
}
```

## Indicators of compromise

| Type | Value | Context |
|------|-------|---------|
| SHA-256 | `a1b2c3d4...` | Stage 1 loader DLL |
| IP | `91.215.85[.]209` | C2 server |
| Domain | `secure-cdn-update[.]com` | C2 domain |
| JA3 | `72a589da586844d7f0818ce684948eea` | Beacon TLS fingerprint |
| Mutex | `Global\{Random GUID}` | Anti-sandbox check |

## References

- [ANY.RUN QakBot analysis](https://any.run)
- [MITRE ATT&CK T1055](https://attack.mitre.org/techniques/T1055/)