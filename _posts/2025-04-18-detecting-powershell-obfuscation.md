---
title: "Detecting PowerShell Obfuscation Techniques"
date: 2025-04-18 12:00:00 +0800
categories: [PowerShell]
tags: [powershell, malware, detection, windows]
author: kynigoi
description: >
  Identifying common PowerShell obfuscation techniques used by attackers
  during post-exploitation.
severity: HIGH
read_time: 8
featured: false
---

## Common obfuscation patterns

- Base64 encoding
- String concatenation
- Reflection loading

## Example detection

```spl
index=wineventlog EventCode=4104
| search ScriptBlockText="*FromBase64String*"
```

## Useful Event IDs

| Event ID | Description |
|----------|-------------|
| 4104 | Script block logging |
| 4688 | Process creation |

## Source

Available on GitHub at
[github.com/unresolvedhost](https://github.com/unresolvedhost)