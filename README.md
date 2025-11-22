
# ⭐ Installing VirtualBox Guest Additions & Setting Up Shared Folders (Linux VM on Windows Host)

## **1. On the Linux VM**

### **Update system & install required packages**

```bash
sudo apt update
sudo apt install -y build-essential dkms linux-headers-$(uname -r)
```

---

## **2. On the Windows Host**

### **Insert the Guest Additions image**

In the VM window menu:

**Devices → Insert Guest Additions CD Image**

---

## **3. Back on the Linux VM**

### **Mount the CD image**

```bash
sudo mkdir -p /mnt/cdrom
sudo mount /dev/cdrom /mnt/cdrom
cd /mnt/cdrom
```

### **Run the installer**

```bash
sudo ./VBoxLinuxAdditions.run
sudo reboot -f
```

---

# ⭐ Setting Up VirtualBox Shared Folders

## **4. On the Windows Host**

Open:

**Settings → Shared Folders → (Edit or Add New)**

Enable:

* ✔ **Auto-mount**
* ✔ **Make Permanent**

Fill the fields:

| Field            | Value                                                |
| ---------------- | ---------------------------------------------------- |
| **Folder Path:** | *Path on Windows* (e.g., `C:\Users\YourName\Share`)  |
| **Folder Name:** | *Name to identify the share* (e.g., `Shared_Folder`) |
| **Mount Point:** | `/mnt/cdrom` *(or better: `/mnt/shared`)*            |

---

# ✔ Recommended Improvement

Avoid using `/mnt/cdrom` for shared folders—you already used it for Guest Additions.

Use this instead:

**Mount point:** `/mnt/shared`
Create if needed:

```bash
sudo mkdir -p /mnt/shared
```

---

If you want, I can also provide a **troubleshooting section**, **full workflow diagram**, or **commands to auto-mount shared folders manually**.
