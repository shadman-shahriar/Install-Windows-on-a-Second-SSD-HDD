# 🧠 Install Windows on a Second SSD/HDD from a Running PC
### *No USB · No Dual Boot · Full Control*

> Install a fresh Windows OS directly onto another drive using **DISM** — from your currently running Windows system. No USB installer, no dual-boot confusion.

---

## 📋 Table of Contents

- [What You Need](#-what-you-need)
- [Step 1 — Identify & Prepare Target Disk](#-step-1-identify--prepare-target-disk)
- [Step 2 — Mount Windows ISO](#-step-2-mount-windows-iso)
- [Step 3 — Apply Windows Image via DISM](#-step-3-apply-windows-image-via-dism)
- [Step 4 — Make the SSD Bootable](#-step-4-make-the-ssd-bootable)
- [Step 5 — Boot into New Windows](#-step-5-boot-into-new-windows)
- [BIOS Settings](#%EF%B8%8F-bios-settings)
- [Hardware Compatibility Notes](#-hardware-compatibility-notes)
- [Why This Method?](#-why-this-method)
- [Troubleshooting](#-troubleshooting)

---

## ⚙️ What You Need

| Requirement | Details |
|---|---|
| 💾 New SSD/HDD | Connected to your PC (SATA or NVMe) |
| 📀 Windows ISO | Windows 7, 10, or 11 |
| 🖥️ Running Windows | Any version with admin access |
| ⏱️ Time | 10–20 minutes |

---

## 🖥️ Step 1: Identify & Prepare Target Disk

Open **Command Prompt as Administrator** and run:

```cmd
diskpart
list disk
```

> 👉 Identify your new SSD by its size. Note the disk number (e.g., `Disk 1`).

Then partition and format it:

```cmd
select disk X
clean
create partition primary
format fs=ntfs quick
active
assign letter=W
exit
```

> ✅ Your new drive is now ready and accessible as **`W:`**

---

## 📦 Step 2: Mount Windows ISO

1. **Right-click** the `.iso` file → **Mount**
2. Windows will assign it a drive letter (e.g., `F:` or `E:`)
3. Navigate to `F:\sources\` and confirm you see one of:

```
install.wim   ✅  (most ISOs)
install.esd   ✅  (some newer ISOs)
```

---

## 🧩 Step 3: Apply Windows Image via DISM

**First, list available editions inside the image:**

```cmd
dism /get-wiminfo /wimfile:F:\sources\install.wim
```

> Note the **Index** number for your desired edition (e.g., `Index 5` = Windows 10 Pro).

**Then apply the image to your new drive:**

```cmd
dism /apply-image /imagefile:F:\sources\install.wim /index:5 /applydir:W:\
```

> ⏳ This will take several minutes. Do not interrupt.

---

## 🔧 Step 4: Make the SSD Bootable

**For legacy BIOS systems:**

```cmd
bcdboot W:\Windows /s W: /f BIOS
```

**For modern UEFI systems (recommended for Windows 10/11):**

```cmd
bcdboot W:\Windows /s W: /f ALL
```

> 💡 Use `/f ALL` if you're unsure — it sets up both BIOS and UEFI boot entries.

---

## 🔁 Step 5: Boot into New Windows

1. **Restart** your PC
2. Press the **Boot Menu key** during POST:

| Manufacturer | Key |
|---|---|
| Most systems | `F12` |
| ASUS | `F8` or `Esc` |
| HP | `Esc` then `F9` |
| Lenovo | `F12` or `Novo button` |
| Dell | `F12` |

3. **Select your new SSD/HDD** from the boot menu

> ✅ No dual-boot menu is created. Your main OS stays completely untouched.

---

## ⚙️ BIOS Settings

Adjust these settings before booting if needed:

| Setting | Value |
|---|---|
| **Windows 7** | Enable Legacy/CSM mode |
| **Windows 10/11** | Use UEFI mode |
| **Secure Boot** | Disable for Windows 7 |
| **Drive Mode** | Set to AHCI (not IDE/RAID) |

---

## ⚠️ Hardware Compatibility Notes

**Windows 7 may not natively support:**

- ❌ USB 3.0 controllers (use USB 2.0 ports during setup)
- ❌ NVMe SSDs (requires driver injection via DISM)
- ❌ 12th Gen+ Intel / Ryzen 5000+ chipsets

**Windows 10/11:**

- ✅ Full NVMe support
- ✅ Full USB 3.0/3.1 support
- ✅ Compatible with modern chipsets

---

## ✅ Why This Method?

| Feature | USB Install | This Method (DISM) |
|---|---|---|
| USB drive required | ✅ Yes | ❌ No |
| Risk of dual-boot mess | ⚠️ High | ✅ None |
| Bootloader conflicts | ⚠️ Possible | ✅ Isolated |
| Main OS affected | ⚠️ Sometimes | ✅ Never |
| Speed | Medium | ⚡ Fast |
| Control over partitions | Limited | ✅ Full |

---

## 🔧 Troubleshooting

**🔴 PC won't boot from new drive?**
```cmd
bcdboot W:\Windows /s W: /f ALL
```
Re-run this and double-check your BIOS boot order.

**🔴 BIOS/UEFI mismatch?**
> The #1 cause of boot failure. If you formatted with `active` (MBR) but your system is UEFI, you'll need to convert the disk to GPT.

```cmd
diskpart
select disk X
clean
convert gpt
```
Then redo Steps 1–4.

**🔴 DISM fails on `.esd` files?**
```cmd
dism /apply-image /imagefile:F:\sources\install.esd /index:1 /applydir:W:\
```
Replace `.wim` with `.esd`.

---

## 🎯 Summary

```
Mount ISO → Apply with DISM → Run bcdboot → Boot via BIOS menu
```

This workflow is ideal for:
- 🧪 **Testing environments** — spin up a clean OS fast
- 🛠️ **Engineers & IT pros** — deploy Windows without installer media
- 💾 **Multi-drive setups** — keep each OS fully isolated
- 🚀 **Fast recovery** — restore a Windows install from ISO in minutes

---

<div align="center">

**Found this useful? Give it a ⭐ and share it.**

*Tested on Windows 10 & 11 host | Target: Windows 7, 10, 11*

</div>
