---
title: "Reverse Engineering Packed PE Files"
date: 2025-04-15 11:15:00 +0800
categories: [Reverse Engineering]
tags: [reverse-engineering, pe, malware, x64dbg]
author: kynigoi
description: >
  Techniques for identifying and unpacking packed Windows PE malware
  samples using static and dynamic analysis.
severity: HIGH
read_time: 10
featured: true
---

## Identifying packed binaries

Packed executables often exhibit:

- High entropy
- Minimal imports
- Runtime API resolution

## Useful tools

| Tool | Purpose |
|------|---------|
| PEStudio | Static inspection |
| x64dbg | Runtime debugging |
| Procmon | API monitoring |

## Basic unpacking workflow

```text
1. Identify entry point
2. Trace unpacking stub
3. Dump process memory
4. Rebuild imports
```

## Source

Available on GitHub at
[github.com/unresolvedhost](https://github.com/unresolvedhost)