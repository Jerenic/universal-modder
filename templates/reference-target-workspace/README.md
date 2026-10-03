# Reference → target project workspace

Copy this folder outside the universal-modder repository when starting a two-game project.

```text
00_ORIGINALS/      immutable user-owned backups/install snapshots
01_RECON/          inventories and RECON.md per game
02_EXTRACTED/      local-only extraction/decompile output
03_MOD/            target mod source + original assets
docs/              comparison, plan and journal
```

Rules:
- never edit `00_ORIGINALS`;
- never commit `02_EXTRACTED`;
- keep reference-game assets out of `03_MOD`;
- perform recon on both games before choosing the first target-game vertical slice.
