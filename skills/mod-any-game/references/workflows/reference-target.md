# Reference game → target game workflow

Use this when the user owns two games and wants to learn from one game's implementation while building a
mod for the other. The **reference game** is studied; the **target game** is where the mod is built.

The purpose is to transfer understanding, not proprietary content.

## Keep the two games physically separate

```text
<project>/
  00_ORIGINALS/
    reference/
    target/
  01_RECON/
    reference/
      RECON.md
    target/
      RECON.md
  02_EXTRACTED/
    reference/               # local-only, never publish
    target/                  # local-only, never publish
  03_MOD/
    target-mod/              # your code + original/recreated assets
  docs/
    COMPARISON.md
    MODDING_PLAN.md
    MODLOG.md
```

Never put extracted reference assets inside `03_MOD`.

## Phase A — independent recon

Recon both games separately before comparing them. For each one record:
- engine/version;
- code backend;
- asset packaging;
- UI system;
- audio/VFX/animation architecture;
- loaders/hooks available;
- save/config/log paths;
- online/anti-cheat constraints;
- unknowns.

## Phase B — compare concepts, not files

Write `docs/COMPARISON.md` with rows such as:
- HUD behavior;
- menu transitions;
- world-space UI;
- hit/death feedback;
- particles/post-processing;
- interaction affordances;
- enemy telegraphs;
- audio event structure.

For every reference observation, classify the target implementation as:
- **recreate** — implement the behavior with original code/assets;
- **adapt** — use the target game's existing system differently;
- **infeasible/unknown** — needs more research;
- **do not transfer** — would require redistributing proprietary content.

## Phase C — target-only vertical slices

Start with one reversible target-game change using placeholder/original content. Do not make success depend
on copied reference assets. Prove the hook and packaging route first, then widen.

Good first slices:
- one custom HUD widget;
- one original sound event;
- one harmless VFX;
- one test item or enemy variant.

## Publishing boundary

Public mods may ship your code, patches/converters and original/recreated assets. Do not ship extracted
assets, decompiled code or game files from either source. If a local experiment temporarily uses an owned
reference asset, keep it outside the mod repo and replace it before packaging.
