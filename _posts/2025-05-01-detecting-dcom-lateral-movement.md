---
title: "Detecting DCOM Lateral Movement with EQL"
date: 2025-05-01 09:00:00 +0800
categories: [Detection Engineering]
tags: [dcom, eql, lateral-movement, windows, splunk]
author: kynigoi
description: >
  DCOM-based lateral movement is a favourite technique for blending into
  Windows administrative traffic. This post breaks down the execution chain
  and delivers ready-to-deploy EQL rules for Elastic SIEM.
severity: HIGH
mitre:
  - "T1021.003 — DCOM"
  - "T1059.001 — PowerShell"
read_time: 10
featured: true
---

## Overview

Distributed Component Object Model (DCOM) provides a mechanism for
software components to communicate over a network. Adversaries abuse it
for lateral movement because it leverages legitimate Windows
infrastructure — `mmc.exe`, `excel.exe`, and `outlook.exe` are all
known DCOM abusable targets.

## Execution chain

A typical DCOM lateral movement chain looks like this:

1. Attacker calls a remote DCOM object over RPC (port 135 + high ports)
2. The target host instantiates the COM object under a legitimate process
3. Code executes in the context of that process — no new service, no PSExec

The most common abused classes are `MMC20.Application`,
`ShellWindows`, and `ShellBrowserWindow`.

## Detection logic

### EQL — DCOM spawning suspicious child

```eql
sequence by host.id with maxspan=30s
  [network where process.name : ("mmc.exe", "excel.exe", "outlook.exe")
   and network.direction == "ingress"
   and source.port == 135]
  [process where process.parent.name : ("mmc.exe", "excel.exe", "outlook.exe")
   and process.name : ("cmd.exe", "powershell.exe", "wscript.exe", "cscript.exe")]
```

### KQL — MMC20 remote instantiation

```kql
event.category: "process" and
process.parent.name: "mmc.exe" and
process.name: ("cmd.exe", "powershell.exe") and
not source.ip: ("127.0.0.1", "::1")
```

## Hunting pivot

Stack unique `process.parent.command_line` values where the parent is
`mmc.exe` and the child is a shell. Any remote IP in the lineage that
isn't a known management host is a strong lead.

## References

- [MITRE ATT&CK T1021.003](https://attack.mitre.org/techniques/T1021/003/)
- [CyberChef DCOM Research](https://www.cybereason.com/blog/dcom-lateral-movement-techniques)