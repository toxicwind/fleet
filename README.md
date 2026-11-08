# Fleet

Hardware devices, TV runtimes, and mobile clients across the estate.

One of the estate's five pens (`pens/{estate,geode,barn,ranch,olympus}`).
Every pen has the same shape: `devices/` for peer standalone device
checkouts, `docs/` for pen-owned documentation, and a `bin/` entrypoint
surface that is symlinked into `~/.local/bin` and pinned by the estate-ops
manifest. This pen owns hardware devices, so its checkouts are called
`devices/` rather than `products/`.

## Devices

| Device | Subdir | Port | Language | Notes |
|---|---|---|---|---|
| Android fleet | `android-fleet/` | 25245 (flotilla-mcp) | Bun/TS + Kotlin | Pixel 9 Pro XL + Google TV; keepalive + flotilla MCP daemons |
| Android MCP | `android-mcp/` | — | TS (Node 22) | ADB device/UI/app control MCP; device profiles (Hisense TV, Pixel 9 Pro XL) |
| Android activity MCP | `android-activity-mcp/` | — | TS (Node 22) | activity enumerate/cache/launch over ADB; JSON activity manifest; fork of android-mcp |
| Fleet chat | `fleet-chat/` | — | Bun/TS | delendai orchestrated chat |
| Fleet ops | `fleet-ops/` | — | Python/TS | watchdog + io-governor |
| Kodi fleet | `kodi-fleet/` | 8080 (RPC) | Python | CoreELEC 246 + Android-TV 225 addons |
| LG fleet | `lg-fleet/` | — | Bun/TS + webOS | LG TV lane (key/deploy/slate) |
| LG hot | `lg-hot/` | 18080 | Bun/TS | live-reload + blue-green sync bridge |

## Layout

```text
fleet/
  .gitignore    kitchen-sink `*` + whitelist (submoduleless doctrine)
  README.md     this file — role, member table, ports, languages
  LICENSE
  package.json
  mise.toml
  AGENTS.md
  bin/          entrypoint symlink surface -> ~/.local/bin
  devices/      peer standalone device checkouts (auto-ignored, never tracked)
  docs/         pen-owned documentation
```

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
