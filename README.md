# 🚀 OnePlus ReSukiSU Kernel Builder

GitHub Actions workflow for building flashable **ReSukiSU GKI kernels** for supported OnePlus devices.

It syncs Android GKI sources, adds **ReSukiSU**, optionally applies **SUSFS**, and packages the result as an **AnyKernel3 ZIP**.

---

## 📱 Supported Devices

| Device | ID | Codename | SoC |
|---|---|---|---|
| OnePlus 15 | `oneplus15` | `Infinity` | `sm8850` |
| OnePlus 15T | `oneplus15t` | `Infinity` | `sm8850` |
| OnePlus 15R | `oneplus15r` | `Infinity` | `sm8845` |
| OnePlus Ace 6T | `ace6t` | `Infinity` | `sm8845` |

---

## ✨ Features

- ReSukiSU integration
- Optional SUSFS
- Optional Baseband Guard / LSM
- Optional Netfilter + IPSet
- Optional BBR + ECN
- Optional Unicode bypass patch
- Flashable AnyKernel3 ZIP
- GitHub Release or artifact output
- Build logs, hashes, and summary

---

## 🚀 Quick Start

1. Fork this repo
2. Open **Actions**
3. Run the workflow
4. Select your device and options
5. Download the generated ZIP

---

## ⚙️ Key Options

| Option | Description |
|---|---|
| `DEVICE` | Device to build, or `all` |
| `KSU_META` | ReSukiSU source: `branch/tag/commit` |
| `SUSFS_META` | Empty = latest, `-1` = disabled, hash = pinned |
| `MANAGER_SOURCE` | `MD3` or `MD3_SPOOF` |
| `LSM` | Enable Baseband Guard | Do not enable as it does not work with 6.12 kernels !
| `NETFILTER` | Enable Netfilter/IPSet |
| `BBR_ECN` | Enable BBR + ECN |
| `CREATE_RELEASE` | Publish ZIP to GitHub Releases |

---

## 📦 Output

Example ZIP:
`AK3_ReSukiSU_43000_OnePlus15_6.12.0.zip`

With LSM:
`AK3_ReSukiSU_43000_OnePlus15_6.12.0_LSM.zip`

---

## ⚠️ Notice

Use at your own risk. Keep a backup boot image and make sure fastboot/recovery access is available.

---

## 🙏 Credits

Thanks to the maintainers of Android GKI, ReSukiSU, SUSFS, AnyKernel3, Baseband Guard, and related community patches.
