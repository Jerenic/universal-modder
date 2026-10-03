# Android / Meta Quest package backups

Use this playbook when the user owns the app/game and provides a local APK backup, a SideQuest backup, or
files pulled from their own Quest. This is a **platform playbook**: after fingerprinting the engine, continue
with the matching engine playbook (usually Unity or Unreal).

## 1. Freeze the source

Treat the acquired backup as immutable. Never decompile, repack or sign in-place.

Recommended layout:

```text
<project>/
  00_ORIGINALS/
    <game>/                 # untouched SideQuest/ADB backup
  01_RECON/
    <game>/                 # inventories and notes only
  02_EXTRACTED/
    <game>/                 # derived/decompiled content; never publish
  03_MOD/
    <game>/                 # your code, patches and original assets
  docs/
    MODDING_PLAN.md
    MODLOG.md
```

If the source lives in a synced folder such as OneDrive, keep the immutable archive there if desired, but
prefer a normal local working directory for large extraction trees and build output.

## 2. Inventory the package set

A SideQuest backup may contain one APK or a split install:

```text
apk/
  base.apk
  split_config.arm64_v8a.apk
  split_config.en.apk
  ...
manifest.json
icon.png
```

Record:
- package id;
- version name/code;
- every APK/split and its size/hash;
- supported ABI(s), normally `arm64-v8a` on Quest;
- whether OBB or external app data exists.

Useful read-only commands:

```bash
unzip -l base.apk
aapt2 dump badging base.apk
apkanalyzer manifest print base.apk
```

If the headset is still available, separately check the owned app's external paths:

```text
/sdcard/Android/obb/<package>/
/sdcard/Android/data/<package>/
```

Do not assume a missing folder is an error; many games keep everything in the APK or download content
elsewhere.

## 3. Fingerprint the engine

### Unity
Common signals:
- `assets/bin/Data/`, `globalgamemanagers`, `resources.assets`, `sharedassets*.assets`;
- `lib/arm64-v8a/libunity.so`;
- IL2CPP: `libil2cpp.so` plus `global-metadata.dat`;
- Mono: managed assemblies such as `Assembly-CSharp.dll`.

Then read `../engines/unity.md`.

### Unreal
Look for cooked `.pak`, `.utoc`, `.ucas`, Unreal asset paths, or Unreal-native libraries. Large cooked
content may be in an OBB rather than the base APK.

Then read `../engines/unreal.md`.

### Other
Use `apktool`/JADX for Android-side resources and code, then identify the engine/runtime from native
libraries and asset signatures.

## 4. Recon before modification

Produce a short inventory that answers:
- engine and likely version;
- Mono / IL2CPP / native;
- base vs split APK structure;
- native libraries;
- asset locations;
- whether OBB/data is required;
- what can be inspected safely;
- the smallest reversible vertical slice.

During recon, do **not**:
- overwrite the original backup;
- re-sign or reinstall packages;
- patch ownership checks, platform auth, DRM or anti-cheat;
- publish extracted assets or decompiled code.

## 5. Lab and first vertical slice

Only after recon:
1. clone the backup into a disposable lab;
2. back up saves/profile data;
3. choose one tiny change (one original texture, one harmless UI element, one sound, or one runtime hook);
4. rebuild/repack only the working copy;
5. sign with a local development key if the normal Android install flow requires it;
6. test offline on the user's device;
7. record exactly how to restore the original.

A repacked APK is not the preservation copy. Keep the untouched original archive permanently separate.
