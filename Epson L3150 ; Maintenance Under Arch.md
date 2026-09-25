# Installation

Package: `gutenprint`
# Web Interface

```
http://localhost:631
```
# `escputil` Cheatsheet

Tool for maintaining Epson Stylus inkjet printers.
Package: `gutenprint` — it's bundled inside, no separate package needed on Arch.
## Common flags

- `-r /dev/usb/lp0` — talk directly to the USB printer device node
- `-u` — printer is a "new" model (Stylus Color 740 or newer)
## Commands

| Flag | Action                             |
| ---- | ---------------------------------- |
| `-i` | Check ink levels                   |
| `-n` | Nozzle check (prints test pattern) |
| `-c` | Head cleaning                      |
| `-a` | Head alignment (interactive)       |
| `-d` | Identify printer model             |
| `-M` | List supported models              |
## Workflow: Fix Clogged Nozzles

1. `sudo escputil -r /dev/usb/lp0 -u -n` — Nozzle check;
2. `sudo escputil -r /dev/usb/lp0 -u -c` — Clean heads;
3. Repeat nozzle check to verify;