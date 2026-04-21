# 🖥️ Install Windows on a Second SSD/HDD from a Running PC (No USB / No Dual Boot)

This guide explains how to install a fresh Windows OS directly onto a second SSD/HDD from a running system (like Windows 11), without using a bootable USB and without creating a traditional dual-boot setup.

It uses **DISM (Deployment Image Servicing and Management)** and **BCDBoot** to deploy Windows manually.

---

## ⚡ Overview

- Works from a running OS (e.g. Windows 11)
- Installs Windows to another SSD/HDD
- No USB installer required
- No automatic dual-boot menu
- Boot is controlled via BIOS/UEFI boot selection

---

## 🧰 Requirements

- A working Windows system (e.g. Windows 11)
- Target SSD/HDD connected internally
- Windows ISO file (Windows 7 / 10 / 11)
- Administrator access (CMD)
- 10–30 minutes

---

## ⚠️ Important Notes

- Always double-check disk selection to avoid data loss
- Windows 7 may not support modern hardware (USB 3.0 / NVMe / UEFI issues)
- Use Legacy/CSM mode for Windows 7 installations
- Secure Boot may need to be disabled

---

# 🚀 Step 1: Identify Target Disk

Open **Command Prompt (Admin)**:

```bash
diskpart
list disk
