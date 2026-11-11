# Fleet

Hardware devices, TV runtimes, and mobile clients across the estate.

This pen owns physical hardware, so its checkouts live under `devices/`.
Each device is a **peer standalone checkout** (referenced-not-owned); the pen
tracks only its own metadata, `bin/`, and `docs/`. Devices are grouped by
**platform** so the fleet scales without a flat pile of unrelated repos.

## Layout

```text
fleet/
  devices/
    android-fleet/   ← ALL Android OS work (ADB-driven): Pixel 9 Pro XL + Hisense TV + OwnTV
      products/      ← every Android product consolidated (MCP servers, IPTV apps, control plane)
    kodi/            ← Kodi addon fleet (CoreELEC 246 + Android-TV 225)
    ops/             ← fleet operations (chat, watchdog)
    webos/           ← LG webOS TVs + AV (lg-fleet, lg-hot, marantz-audyssey)
  ssot/              ← single source of truth (devices, ports, identities)
  docs/              ← pen-owned documentation
  bin/               ← entrypoint symlink surface -> ~/.local/bin
```

## Devices

### `android-fleet/` — Android OS (ADB-driven)

The primary Android repo: Pixel 9 Pro XL (auth vault, wireless ADB) + Hisense
SmartTV 4K FFM `wuyishan` (10.0.0.225, OwnTV IPTV host). All Android device
work is consolidated here as `products/`:

| Product | Port | Language | Purpose |
|---|---|---|---|
| `owntv-fork/` | — | Kotlin/Compose | OwnTV IPTV fork (v99.99.99 on the Hisense) + hot-reload |
| `luaj-dropin/` | 38383 | Kotlin + Lua | Lua-over-HTTP control plane for any Android app |
| `nuvio/` | — | Kotlin/JS | Nuvio IPTV apps (Android TV build on the Hisense) |
| `android-mcp/` | — | TS (Node 22) | ADB device/UI/app control MCP; device profiles |
| `android-activity-mcp/` | — | TS (Node 22) | activity enumerate/cache/launch over ADB |
| `phone-mcp/` | — | Python | phone control MCP (ADB commands) |
| `fennec-cdp/` | — | TS | Fennec (Firefox for Android) RDP→CDP on the Pixel |
| `newblackdex/` | — | — | non-root dex unpacking tooling |

### `kodi/` — Kodi addon fleet

| Device | Subdir | Port | Notes |
|---|---|---|---|
| Kodi fleet | `kodi-fleet/` | 8080 (RPC) | CoreELEC 246 + Android-TV 225 addons |

### `ops/` — fleet operations

| Device | Subdir | Notes |
|---|---|---|
| Fleet chat | `fleet-chat/` | delendai orchestrated chat |
| Fleet ops | `fleet-ops/` | watchdog + io-governor |

### `webos/` — LG webOS TVs + AV

| Device | Subdir | Port | Notes |
|---|---|---|---|
| LG fleet | `lg-fleet/` | — | LG TV lane (key/deploy/slate) + nuvio-webos IPTV products |
| LG hot | `lg-hot/` | 18080 | live-reload + blue-green sync bridge |
| Marantz | `marantz-audyssey/` | — | AV receiver control |

## Tooling

- **bin surface**: entrypoints symlinked into `~/.local/bin`, pinned by
  `pens/barn/estate-ops/bin/manifest.yaml` and verified by
  `pens/barn/estate-ops/bin/bin-verify.sh`. A broken link is a FAIL, never a WARN.
- **README**: generated from the pen's own device set by
  `pens/barn/estate-ops/scaffold/bin/readme-gen.ts`; regenerate with
  `bun run readme`.
- **manifest**: regenerate with
  `pens/barn/estate-ops/bin/manifest-gen.py`.

## Conventions

- Devices are **peer standalone checkouts**, referenced-not-owned. This repo
  tracks only its own metadata, `bin/`, and `docs/`.
- Daemon ids use **dashes, never underscores** (`config-naming-conventions.md`).
- Ports are declared in the control plane's `config/ports.env` as
  `UPPER_SNAKE_*_PORT=<25xxx>` — that file is the numeric SSOT.
- Runtime language across the estate is **Bun/TypeScript**.

## License

Apache-2.0 unless a product states otherwise. Confidential products keep
their own license and their own visibility.