# Infinix / Transsion 45W FastCharge Fix (KernelSU / Magisk)
# Disclaimer: This module might brick your phone. Author is not responsible for aftermath of using this module on your device. Use it by your own risk and be careful

A safe, kernel-native fix for restoring proprietary **Fast Charging** (Pump Express 5.0 / Transsion Multi-level Charging) on Infinix and Tecno (MediaTek MT6789 / Helio G99 / Dimensity) devices running **Generic System Images (GSI)**.

---

## 📌 Background & Technical Overview

On custom GSI ROMs (e.g., Infinity X, LineageOS, Pixel Experience), proprietary Transsion charging services are often either uninitiated or revert to standard USB SDP mode (limiting charge rate to ~7W-10W). 

Previous workaround scripts attempted to override raw thermal files and forcefully shut down thermal daemons (`thermal_core`), causing system instability, emergency reboots, or risk of battery swelling.

### The Solution
This module interacts directly with the **official kernel sysfs interface** (`chgspeed_ctl`) and the system property manager (`persist.sys.mutilelvel.charge`), unlocking full 45W charge capability without bypassing hardware thermal protection (`PID_CHG`).

---

## ⚡ How It Works

1. **Daemon & HAL Binding:** Ensures `chg_sence` and `vendor.hardware.trancharge-service` AIDL interfaces run in their proper SELinux context.
2. **Multi-Level Charge Unlocking:** Sets `persist.sys.mutilelvel.charge=6` (Level 6 = 45W peak) and echoes `1:6` to `/sys/devices/platform/charger/chgspeed_ctl`.
3. **Safe Thermal Coexistence:** Keeps the kernel's internal `PID_CHG` thermal controller active (`target_temp = 4000` / 40.0°C). As the battery warms up past 40°C, current is dynamically and safely throttled to 10W-16W, preventing battery degradation while maximizing speed when cool.

---

## 📁 Module File Structure

- **`service.sh`** — Background monitoring script launched on boot after `sys.boot_completed=1`.
- **`module.prop`** — KernelSU / Magisk module metadata.
- **`ChargeBoost_KSU.zip`** — Pre-packaged flashable zip module.

---

## 🚀 Installation Instructions

### Install via KernelSU / Magisk Manager
1. Copy `ChargeBoost_KSU.zip` to your device storage.
2. Open **KernelSU** or **Magisk** app.
3. Go to the **Modules** tab -> **Install from storage**.
4. Select `ChargeBoost_KSU.zip` and flash it.
5. Reboot your smartphone.

---

## 👤 Credits & Maintainers
* **Author:** Coldy
* **Target Hardware:** Infinix / Tecno (MT6789 / MediaTek platforms)
* **Tested On:** Infinix hot 60 pro+. Android 16.  Project Infinity X GSI 
