# CHANGELOG — fleet pen

Pen-level log of fleet device/product changes. Product repos carry their own history; this file records what changed at the pen/fleet level and why.

## 2026-11-08

- **android-fleet (device repo): Hisense wuyishan Shizuku black-screen fix, end-to-end.**
  - Root cause: v12-era Shizuku start command (`app_process / moe.shizuku.server.ShizukuServer`) — class absent from the v13.6 release APK → SIGABRT tombstones. Real start: bundled `lib/arm/libshizuku.so --apk=<base.apk>` (entry `rikka.shizuku.server.ShizukuService`).
  - TV "black screen" root-caused as display-standby (wakeable; not a panel fault).
  - New supervised daemon `estate/hisense-shizuku-keeper` (`src/hisense-shizuku-keeper.ts`, TDD 15 unit + 4 live green): event-driven `ip monitor neigh` boot-detect re-run of the start one-liner — no sleep-polling. Registered in the estate pitchfork monolith.
  - Evidence archived: `devices/android-fleet/docs/evidence/2026-11-08-shizuku-black-screen/` (moved from `/home/toxic/tmp/hisense-shizuku-20261108`, checksums in MANIFEST.md).
  - Recon updated: `devices/android-fleet/docs/hisense-wuyishan.md` addendum 6 (start contract, standby semantics, grants correction, /data 68%).
- Namespace note: pitchfork defines only estate/geode/barn/ranch namespaces; fleet-pen daemons register in the estate monolith by absolute path and are addressed `estate/<name>` (fleet's own `pitchfork.toml` stays an empty shell — no second truth).

- **fleet-tv: esencial M3U + XMLTV EPG seeding (seeder v2)** — one-shot per install, idempotent, live-verified on the wuyishan TV; Xtream audit 56/60 alive, 4 dead parked (docs: devices/android-fleet/docs/hisense-wuyishan.md addendum 7).

- **nuvio-tv-fleet max-auto (2026-11-09)** — StreamProbe (/live classification, HttpURLConnection exact codes, 302-token follow, VLC UA for xtream), mirror [Opc.N] fallback with per-channel index, stream-viability bouncer filter, PanelDim (long-press PLAY = panel floor brightness, "black-but-usable"), deploy script scripts/deploy-fleet-tv.sh. Commits 06a8e61c5..3bf301dc3.
- **Xtream stream-gate recon** — player_api auth ≠ playable; N1 edge 407s all clients; working flow = 302→token→LB with VLC/3.0.20 UA (android-iptv m3u8+ts proven); PlayTorrio finder audit (login-verifier gap + its VLC _isAlive is right). Evidence: android-fleet docs/evidence/xtream-stream-gate-recon-20261108.md, playtorrio-xtream-finder-audit-20261109.md.
