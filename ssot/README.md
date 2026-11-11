# Fleet SSOT

Single sources of truth for fleet devices, IPs, SSH identities, and ports.

| File | What it holds |
|---|---|
| `devices.md` | device inventory: IPs, ADB endpoints, codenames, roles |
| `ports.env` | the numeric port registry (UPPER_SNAKE_*_PORT) |

Authoritative over prose docs. When a README/doc disagrees with these, the
device/code wins — fix the doc, never these.