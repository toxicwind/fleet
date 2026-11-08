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
