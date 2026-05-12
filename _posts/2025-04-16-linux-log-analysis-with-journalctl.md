---
title: "Linux Log Analysis with journalctl"
date: 2025-04-16 13:20:00 +0800
categories: [Linux]
tags: [linux, journald, logs, threat-hunting]
author: kynigoi
description: >
  Using journalctl effectively for Linux incident response and system
  troubleshooting.
severity: LOW
read_time: 5
featured: false
---

## Why journalctl

Systemd-based Linux distributions centralize logs using journald.

## Useful commands

```bash
journalctl -xe
journalctl -u ssh
journalctl --since yesterday
```

## High-value logs

| Service | Relevance |
|---------|-----------|
| sshd | Remote access |
| sudo | Privilege escalation |

## Source

Available on GitHub at
[github.com/unresolvedhost](https://github.com/unresolvedhost)