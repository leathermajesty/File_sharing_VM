# 📁 VirtualBox Shared Folders — Setup Guide

> **Share files seamlessly between a Windows host and a Linux VM using VirtualBox Guest Additions.**

---

## 📑 Table of Contents

- [Prerequisites](#-prerequisites)
- [Step 1 — Install Dependencies on Linux VM](#step-1--install-dependencies-on-linux-vm)
- [Step 2 — Insert Guest Additions CD Image](#step-2--insert-guest-additions-cd-image)
- [Step 3 — Install Guest Additions](#step-3--install-guest-additions)
- [Step 4 — Configure Shared Folder on Windows Host](#step-4--configure-shared-folder-on-windows-host)
- [Step 5 — Access the Shared Folder](#step-5--access-the-shared-folder)
- [Best Practices](#-best-practices)
- [Troubleshooting](#-troubleshooting)
- [Quick Reference](#-quick-reference)

---

## 📋 Prerequisites

| Requirement       | Details                          |
| ----------------- | -------------------------------- |
| **Host OS**       | Windows 10/11                    |
| **Hypervisor**    | Oracle VirtualBox (latest)       |
| **Guest OS**      | Ubuntu / Debian-based Linux      |
| **Network**       | Internet access on VM (for `apt`)|

---

## Step 1 — Install Dependencies on Linux VM

Open a terminal in the Linux VM and install the required build tools:

```bash
sudo apt update
sudo apt install -y build-essential dkms linux-headers-$(uname -r)
```

> **What this does:** Installs the compiler toolchain and kernel headers needed to build the Guest Additions kernel modules.

---

## Step 2 — Insert Guest Additions CD Image

In the VirtualBox VM window menu bar:

```
Devices → Insert Guest Additions CD Image
```

This virtually inserts the Guest Additions ISO into the VM's CD drive.

---

## Step 3 — Install Guest Additions

Back in the Linux VM terminal:

```bash
# Mount the CD image
sudo mkdir -p /mnt/cdrom
sudo mount /dev/cdrom /mnt/cdrom

# Run the installer
cd /mnt/cdrom
sudo ./VBoxLinuxAdditions.run

# Reboot to apply changes
sudo reboot -f
```

After reboot, verify the installation:

```bash
lsmod | grep vboxguest
```

You should see `vboxguest` in the output, confirming Guest Additions are loaded.

---

## Step 4 — Configure Shared Folder on Windows Host

1. In VirtualBox, go to:
   ```
   VM Settings → Shared Folders → Add New Shared Folder (📁+ icon)
   ```

2. Fill in the fields:

   | Field            | Value                                              | Example                        |
   | ---------------- | -------------------------------------------------- | ------------------------------ |
   | **Folder Path**  | The folder on your Windows host to share            | `C:\Users\YourName\Share`      |
   | **Folder Name**  | A label to identify the share inside the VM         | `Shared_Folder`                |
   | **Mount Point**  | Where the folder appears inside the Linux VM        | `/mnt/shared`                  |

3. Enable these options:
   - ✅ **Auto-mount** — Mounts automatically on VM boot
   - ✅ **Make Permanent** — Persists across VM restarts

---

## Step 5 — Access the Shared Folder

After configuring, the shared folder should appear at your mount point. If it doesn't auto-mount:

```bash
# Create the mount point
sudo mkdir -p /mnt/shared

# Mount manually
sudo mount -t vboxsf Shared_Folder /mnt/shared

# Verify
ls /mnt/shared
```

To make manual mounts persistent across reboots, add this to `/etc/fstab`:

```bash
Shared_Folder   /mnt/shared   vboxsf   defaults,uid=1000,gid=1000   0   0
```

---

## 💡 Best Practices

- **Don't use `/mnt/cdrom` as the mount point** for shared folders — it conflicts with the Guest Additions CD mount. Use `/mnt/shared` or a custom path instead.
- **Add your user to the `vboxsf` group** to avoid permission issues:
  ```bash
  sudo usermod -aG vboxsf $USER
  ```
  Then log out and log back in.
- **Use `Make Permanent`** in VirtualBox settings so you don't lose the shared folder config on restart.

---

## 🔧 Troubleshooting

| Problem | Solution |
| ------- | -------- |
| `mount: unknown filesystem type 'vboxsf'` | Guest Additions not installed correctly. Re-run Step 3. |
| Shared folder is empty | Check the folder path on the Windows host is correct. |
| `Permission denied` on shared folder | Run `sudo usermod -aG vboxsf $USER` and re-login. |
| Guest Additions installer fails | Ensure `build-essential`, `dkms`, and correct `linux-headers` are installed. |
| `/dev/cdrom` not found | Try `/dev/sr0` instead: `sudo mount /dev/sr0 /mnt/cdrom` |
| Changes on host not reflected in VM | Try `sudo umount /mnt/shared && sudo mount -t vboxsf Shared_Folder /mnt/shared` |

---

## 📌 Quick Reference

```bash
# Full setup in one go (after inserting Guest Additions CD):
sudo apt update && sudo apt install -y build-essential dkms linux-headers-$(uname -r)
sudo mkdir -p /mnt/cdrom && sudo mount /dev/cdrom /mnt/cdrom
cd /mnt/cdrom && sudo ./VBoxLinuxAdditions.run
sudo reboot -f

# After reboot — mount shared folder:
sudo mkdir -p /mnt/shared
sudo mount -t vboxsf Shared_Folder /mnt/shared
sudo usermod -aG vboxsf $USER
```

---

## 📚 See Also

- [`git_commands.md`](git_commands.md) — Git command reference for this project

---

**License:** This guide is open-source and free to use.
