# 💽 Troubleshooting Guide: Undetected HDD and When to Replace

**Author:** Jose Antonio Acebuche  
**Use Case:** Diagnose a hard disk drive (HDD) that is not detected by the system and determine if it requires replacement.

---

## 🧩 Common Symptoms of Undetected HDD:
- BIOS/UEFI does not show the HDD
- HDD missing in Disk Management (Windows) or `lsblk`/`fdisk -l` (Linux)
- "No Bootable Device" or similar error on startup
- System freezes or lags when connected

---

## 🛠️ Step-by-Step Troubleshooting Procedure

### 1. 🔌 Physical Check
- Power off the system and open the case.
- Reseat both the **SATA/Power cables**.
- Try a different SATA port or cable.
- Plug the HDD into a known-working system.

---

### 2. 🧪 BIOS Detection
- Reboot and enter BIOS/UEFI.
- Check if the HDD appears under Storage/Boot section.
- If **not detected**, switch SATA mode (AHCI/IDE) and check again.

---

### 3. 🗃️ OS-Level Detection
#### On Windows:
- Open **Disk Management**:
  - `Win + X` → **Disk Management**
  - Look for **Unallocated** or **Offline** disks

#### On Linux:
- Use `lsblk` or `fdisk -l` to list block devices.
- Use `dmesg | grep sd` to look for detection logs.

---

### 4. 🧰 Test with Tools
- Use HDD diagnostic software:
  - **Windows**: CrystalDiskInfo, HDDScan
  - **Linux**: `smartctl`, `badblocks`

Example:
```bash
smartctl -a /dev/sdX
