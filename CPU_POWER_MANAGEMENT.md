# CPU Power Management Calibration (CPUFriend)

## Why
`MacPro7,1` (this repo's SMBIOS) only ships with Xeon W CPUs on real Apple hardware, so macOS's native power management table for it doesn't match an `i9-9900K`. Dortania's own SMBIOS guide warns: *"you'll likely need to fix power management as sleep may break if your hardware isn't of this class (ie. HEDT/Server/AMD)"* for `MacPro7,1`/`iMacPro1,1`.

`iMacPro1,1`, however, has broad, well-tested native power management data that works well for many desktop Coffee Lake-class CPUs (this is already the SMBIOS discussed in [Hardware Acceleration](README.md#hardware-acceleration) for iGPU+dGPU hybrid setups). The fix, confirmed by a real user report ([r/hackintosh, "Fixing MacPro7,1 Power Management on Hackintosh"](https://www.reddit.com/r/hackintosh/comments/1l5r9ce/fixing_macpro71_power_management_on_hackintosh/)), is to **temporarily boot as `iMacPro1,1`, extract its working power management data with CPUFriendFriend, then switch back to `MacPro7,1`** while keeping that extracted data injected via `CPUFriend.kext`. From the report:

> Switch your SMBIOS to iMacPro1,1. Use CPUFriendFriend to generate the CPUFriendDataProvider.kext. Add the kext to your EFI and update your config.plist (CPUFriend.kext is also needed). Switch your SMBIOS back to MacPro7,1. Previously, I tried generating CPUFriendDataProvider.kext directly on MacPro7,1, but it didn't work.

`CPUFriend.kext` (already added to this branch's `EFI/OC/Kexts` and wired into `config.plist`, loaded right after `Lilu.kext`) hooks `configResourceCallback()` in `X86PlatformPlugin` and injects whatever data `CPUFriendDataProvider.kext` provides — it works under any SMBIOS, so the final `MacPro7,1` boot doesn't need to match what generated the data.

## What's already staged (this branch: `experiment/cpufriend-calibration`)
- `EFI/OC/Kexts/CPUFriend.kext` (official acidanthera 1.3.0 release, checksum-verified against the GitHub release download)
- Matching `Kernel > Add` entry in `config.plist`, positioned after `Lilu.kext`

## `plugin-type` / SSDT-PLUG — why it's *not* being re-added here
CPUFriend needs `plugin-type=1` set on the CPU for `X86PlatformPlugin`/`AppleACPICPU` to spawn in a way CPUFriend can hook. Historically that came from `SSDT-PLUG.aml`. This repo's `History` shows `SSDT-PLUG` was deliberately removed ("due to macOS version >= 12.3") — confirmed to be because the Z390 Aorus Master's **native DSDT already defines `plugin-type` for CPU0**, so a separate `SSDT-PLUG.aml` would be redundant and risks a duplicate/conflicting `plugin-type` injection (a known cause of kernel panics on modern macOS). **Do not re-add `SSDT-PLUG.aml`** — CPUFriend should hook into the native ACPI-provided `plugin-type` without it.

Sanity-check this assumption once booted, before assuming CPUFriend is doing anything:
```
ioreg -l -p IODeviceTree -w0 | grep -i plugin-type
```
If nothing comes back, `plugin-type` isn't actually set anywhere and CPUFriend has nothing to hook — that would need separate investigation (do **not** just drop `SSDT-PLUG.aml` back in without checking for a conflict first).

## What still needs to happen on real hardware (cannot be done from here)
Generating `CPUFriendDataProvider.kext` requires reading live `ioreg` data from your actual CPU while booted — this has to be done on your Mac.

### Step 1 — Temporarily switch to `iMacPro1,1`
In `EFI/OC/config.plist` → `PlatformInfo > Generic`, you need **all** of these regenerated to match `iMacPro1,1`'s serial format (don't just change `SystemProductName` alone — `MLB`, `SystemSerialNumber`, and `SystemUUID` are model-specific and must match, or macOS/OpenCore may reject them):
- `SystemProductName`: `MacPro7,1` → `iMacPro1,1`
- `SystemSerialNumber`, `MLB`, `SystemUUID`: generate fresh values for `iMacPro1,1` using [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) (same tool class used to generate the current `MacPro7,1` values in this repo) — do **not** reuse the current `MacPro7,1` serial under a different model name.

Keep everything else (kexts, ACPI, DeviceProperties) unchanged. This is a throwaway boot purely to read power management data — don't sign into iCloud/iMessage under this temporary identity.

### Step 2 — Boot and run CPUFriendFriend
1. Boot into macOS as `iMacPro1,1`.
2. Download [CPUFriendFriend](https://github.com/corpnewt/CPUFriendFriend) and run `./CPUFriendFriend.command`.
3. When prompted for LFM/EPP/Perf Bias, you can accept the tool's defaults — the point of this technique is to transplant Apple's own real `iMacPro1,1` calibration, not hand-tune new values. Only deviate if you specifically want a different performance/power-saving bias (see tables below).
4. It outputs a `CPUFriendDataProvider.kext` (prefer this over the SSDT variant, per CPUFriend's own instructions, to avoid injection headaches).

**LFM (Low Frequency Mode) quick reference**, if you'd rather not look up the i9-9900K's TDP-down frequency on Intel ARK (source: [basic.heavietnam.com power management guide](https://basic.heavietnam.com/universal/fix-power-management), already linked from this README):

| CPU/SMBIOS class | LFM |
|---|---|
| Laptop Gen 5+ | `0x08` |
| **Desktop Gen 5+ (i9-9900K falls here)** | **`0x0A`** |
| Haswell/Broadwell HEDT/Server (X99) | `0x0D` |
| Skylake+ HEDT/Server | `0x0C` |

Manual fallback formula: `LFM_MHz = max_clock_GHz / 2 × 1000`, then hex-encode `LFM_MHz / 100`. E.g. base clock 3.6GHz → 1.8GHz → 1800MHz → `1800/100=18` → `0x12`.

**EPP (Energy Performance Preference)**:

| EPP hex | Profile |
|---|---|
| `0x00` | Max performance |
| `0x40` | Balance performance |
| `0x80` | Balance power |
| `0xC0` | Max power saving |

### Step 3 — Switch back to `MacPro7,1` and inject the data
1. Copy the generated `CPUFriendDataProvider.kext` into `EFI/OC/Kexts/`.
2. Add a matching `Kernel > Add` entry for it, positioned **after** `CPUFriend.kext`.
3. Revert `PlatformInfo > Generic` back to the original `MacPro7,1` values (`SystemProductName`, `SystemSerialNumber`, `MLB`, `SystemUUID`, `ROM` already in this repo — don't touch `ROM`, it's independent of model).
4. Boot as `MacPro7,1` again.

## Verification (manual, on real hardware)
- Confirm the injected data loaded: `ioreg -c IOPlatformExpertDevice -w0 | grep -i cf-frequency-data` (or check Lilu's debug log for `CPUFriend`).
- Check CPU no longer throttles unexpectedly under load (Activity Monitor / `pmset -g therm`), and that Turbo Boost ramps as expected.
- Run sleep/wake cycles — this is the symptom Dortania's guide specifically flags for `MacPro7,1` on non-HEDT hardware.
