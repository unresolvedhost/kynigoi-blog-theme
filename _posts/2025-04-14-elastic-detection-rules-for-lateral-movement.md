---
title: "Elastic Detection Rules for Lateral Movement"
date: 2025-04-14 09:45:00 +0800
categories: [Detection Engineering]
tags: [elastic, eql, detection-engineering, windows]
author: kynigoi
description: >
  Detecting common lateral movement techniques using Elastic EQL and
  Windows telemetry sources.
severity: HIGH
read_time: 6
featured: false
---

## The challenge

Lateral movement often blends into legitimate administrative activity.

## Example EQL query

```sql
sequence by host.id
  [process where process.name == "cmd.exe"]
  [network where destination.port == 445]
```

## High-value telemetry

| Source | Value |
|--------|------|
| Sysmon | Process lineage |
| Security Logs | Authentication |
| PowerShell | Script execution |

## Source

Available on GitHub at
[github.com/unresolvedhost](https://github.com/unresolvedhost)