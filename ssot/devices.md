# Fleet devices — SSOT

Single source of truth for fleet devices, IPs, ADB endpoints, and SSH identities.
Authoritative over any prose doc; when a doc disagrees, the code/device wins.

## Android (ADB-driven)

| Device | Codename | IP | ADB | Android | Role |
|---|---|---|---|---|---|
| Pixel 9 Pro XL | `pixel` | 10.0.0.77 | wireless debug (port rotates, keepalive tracks) | 17 | auth vault, browser (Fennec) |
| Hisense SmartTV 4K FFM | `wuyishan` | 10.0.0.225 | 10.0.0.225:5555 (fixed) | 11 (SDK 30) | OwnTV IPTV host |

- Hisense model: 55E82GUA, mt5895 SoC, `user` build, NO root path (locked AVB).
- Pixel pairing keys: `/home/toxic/.android/adbkey*` (credentials — never commit).

## webOS (LG)

| Device | IP | Access | Role |
|---|---|---|---|
| LG TV (SLATE) | 10.0.0.176 | webOS ares/SSH | lg-fleet deploy target |
| Marantz AV | — | webOS-adjacent | AV receiver |

## Kodi

| Box | IP | Platform | Role |
|---|---|---|---|
| CoreELEC | 10.0.0.246 | Amlogic CoreELEC | Kodi addon host |
| Android-TV Kodi | 10.0.0.225 | Android TV | Kodi addon host (same box as Hisense) |

## Ports (numeric SSOT)

See `ssot/ports.env` — the authoritative port registry.

## Identities

- SSH user on SnackyLaptop: `beebe`
- Hisense serial: `0000dc9a7d3e0b65`
- Hisense MAC: `38:64:07:f0:48:56` (Wi-Fi) / `38:64:07:f0:48:55` (BT)