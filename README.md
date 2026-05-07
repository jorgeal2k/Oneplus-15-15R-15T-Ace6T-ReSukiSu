# Build OnePlus 15 / 15R / 15T / Ace 6T — ReSukiSU Kernel Builder

A GitHub Actions workflow that builds an **Android GKI kernel**, integrates **ReSukiSU** (KernelSU fork), optionally applies **SUSFS**, and packages the result as a flashable **AnyKernel3 ZIP**.

It’s designed for configurable CI runs with useful build metadata (**hashes, sizes, timings, warnings**) and optional features like **NETFILTER**, **BBR+ECN**, **KPM**, and **Baseband Guard (BBG)**.

---

## ✨ Highlights

- **Multi-device support**
  - `oneplus15` → OnePlus 15 (**sm8850**)
  - `oneplus15T` → OnePlus 15 (**sm8850**)
  - `oneplus15r` → OnePlus 15R (**sm8845**)
  - `ace6t` → OnePlus Ace 6T (**sm8845**)
- **ReSukiSU integration** via upstream `setup.sh` (supports `KSU_META`)
- Optional **SUSFS** patching + metadata capture (**branch, commit, version**)
- Optional **KPM** image patching (`patch_linux`) when `KPM=KPM`
- Network toggles: **NETFILTER** + **BBR/ECN**
- Security toggle: **LSM / Baseband Guard (BBG)**
- **Manager compatibility**
  - **Supported Unofficial Manager:** `MKSU`, `RKSU`, `SukiSU-Ultra`, `ReSukiSU`
- Repro/CI-friendly options
  - Custom **build timestamp**
  - Custom **kernel suffix** (or random if empty)
  - Optional **ccache**
  - Artifacts: **flashable ZIP**, **build log**, **build report**

---

## 📦 Output

### What you get
- Uploaded artifact: **flashable AnyKernel3 ZIP** containing the built `Image`
- Uploaded artifact: **build log** (`kernel_workspace/build.log`) when present
- Uploaded artifact: **build report** (`build_report.txt`)

### ZIP naming format
The workflow produces a ZIP like:

- `AnyKernel3_<MANAGER_SOURCE>_<KSUVER>_<DEVICE_NAME_SAFE>_<TKERNEL_VERSION>[_KPM][_LSM].zip`

Where:
- `<MANAGER_SOURCE>` → `MD3` / `MD3_SPOOF`
- `<KSUVER>` → computed KernelSU/ReSukiSU version
- `<DEVICE_NAME_SAFE>` → `OnePlus15` / `OnePlus15R` / `Ace6T`
- `<TKERNEL_VERSION>` → `VERSION.PATCHLEVEL.SUBLEVEL` from `common/Makefile`
- `[_KPM]` → appended only when `KPM=KPM`
- `[_LSM]` → appended only when `LSM=true`

### Build summary includes
- ZIP size + SHA256
- Image size + SHA256
- Build duration
- Warnings count (best-effort from `build.log`)
- ReSukiSU commit (with link)
- SUSFS commit (with link, if enabled)
- Manager compatibility note:
  - **Supported Unofficial Manager:** `MKSU`, `RKSU`, `SukiSU-Ultra`, `ReSukiSU`

---

## 🚀 How to Use

1. **Fork** this repo (or copy the workflow into your repo).
2. Go to **Actions** → your workflow (e.g. **OnePlus 15 / 15R / Ace 6T ReSukiSU**).
3. Click **Run workflow**.
4. Choose your options (`DEVICE`, `KSU_META`, `SUSFS_META`, `KPM`, `NETFILTER`, etc.).
5. Download the artifact ZIP from the workflow run page.

---

## ⚙️ Workflow Inputs (quick reference)

### Device selection
- `DEVICE`: target device (sets SoC + kernel manifest branch)
  - `oneplus15` → `CPU=sm8850`, `KERNEL_BRANCH=android16-6.12-2025-06`
  - `oneplus15r` → `CPU=sm8845`, `KERNEL_BRANCH=android16-6.12-2025-09`
  - `ace6t` → `CPU=sm8845`, `KERNEL_BRANCH=android16-6.12-2025-09`

### ReSukiSU / SUSFS
- `KSU_META`: `branch/custom_tag(optional)/commit_hash(optional)`
  - Example (latest): `main/⚡Ultra⚡/`
  - Example (pinned): `main/MyTag/abc1234`
- `SUSFS_META`:
  - `-1` → disable SUSFS
  - empty → latest
  - git hash → checkout a specific commit
- `SUSFS_DEV`: fetch/use the SUSFS `-dev` branch when available

### Modules / features
- `KPM`: `KPM` / `KPN` / `NoN`
- `LSM`: enable Baseband Guard (BBG)
- `NETFILTER`: enable NETFILTER extras (IPSET + IPv6 NAT where supported)
- `BBR_ECN`: enable BBR + ECN configuration
- `UNICODE_BYPASS`: apply Unicode bypass patch (if selected)
- `XUS_FIX`: prevent generation of 5uS error files (typically used with SUSFS)

### Build customization
- `BUILD_TIME`: custom timestamp, or `F` for current UTC
- `SUFFIX`: custom kernel suffix (random if empty)
- `SUBLEVEL`: custom kernel `SUBLEVEL` override (integer)

### CI / performance
- `BUILD_NOCCACHE`: disable ccache
- `SPACE_NOCLEAN`: disable disk cleanup/max-space steps

---

## 🔐 Security & Reproducibility

- Minimal permissions: `contents: read`
- `concurrency` enabled to avoid overlapping builds for the same device/ref.

### External scripts notice
This workflow downloads and executes upstream scripts (e.g. `setup.sh`). For stronger supply-chain safety and reproducibility, prefer a “secure mode” that:
- pins script URLs to a **commit SHA**
- verifies content using **SHA256**
- pins tool downloads (like the `repo` tool) using **SHA256**

---

## ⚠️ Notes / Disclaimers

- This kernel may include advanced features (root/module frameworks, filesystem hiding layers, etc.).  
  **Use responsibly and only on devices you own and are willing to recover.**
- Always keep a known-good boot image and a recovery path available (fastboot/recovery).
- Not affiliated with OnePlus, Google, or any upstream projects referenced here.

---

## 🙏 Credits

This workflow references and/or uses components from:
- Android kernel manifests
- **ReSukiSU** (KernelSU fork)
- **SUSFS** patch set
- **AnyKernel3** packaging template
- Patch collections used by the CI steps

All credit belongs to their respective maintainers.
