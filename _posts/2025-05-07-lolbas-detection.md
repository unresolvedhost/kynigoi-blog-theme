---
title: "Living Off the Land: Detecting LOLBas Abuse via Process Lineage"
date: 2025-05-07 09:00:00 +0800
categories: [Detection Engineering]
tags: [lolbas, eql, kql, elastic, windows]
featured: true
author: kynigoi
description: >
  Adversaries abuse built-in Windows binaries to blend into legitimate
  traffic. This post covers process lineage heuristics and EQL/KQL
  detection rules for Elastic SIEM.
severity: HIGH
mitre:
  - "T1218 — Signed Binary Proxy Execution"
  - "T1059.001 — PowerShell"
  - "T1036.003 — Rename System Utilities"
read_time: 12
pin: true
---

## Overview

Living-off-the-land binaries (LOLBas) are legitimate system tools —
`certutil.exe`, `mshta.exe`, `regsvr32.exe` — repurposed by adversaries
to proxy execution, stage payloads, or establish persistence.

> **Analyst note:** Tune these rules in environments with high
> administrative activity before alerting.

## Detection logic

### EQL — parent–child anomaly

```eql
sequence by host.id, process.parent.entity_id
  [process where process.name : (
    "mshta.exe", "certutil.exe", "regsvr32.exe",
    "rundll32.exe", "wscript.exe", "cscript.exe"
  ) and process.parent.name : (
    "winword.exe", "excel.exe", "outlook.exe"
  )]
  [network where network.direction == "egress"
   and not cidr_match(destination.ip, "10.0.0.0/8")]
with maxspan=30s
```

### KQL — certutil abuse

```kql
process.name : "certutil.exe" and (
  process.args : ("-urlcache", "/urlcache") and
  process.args : ("http*", "ftp*")
)
```

## Indicators of compromise

| Type | Value | Context |
|------|-------|---------|
| SHA-256 | `e3b0c44298fc1c14...` | Certutil-decoded dropper |
| Domain | `update-cdn[.]systems` | C2 staging domain |
| IP | `185.220.101[.]47` | Tor exit node |

## References

- [MITRE ATT&CK T1218](https://attack.mitre.org/techniques/T1218/)
- [LOLBAS Project](https://lolbas-project.github.io)