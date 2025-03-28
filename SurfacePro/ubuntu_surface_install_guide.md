# ✅ How to Install Ubuntu on a Microsoft Surface Pro 5, 6, or 7

This guide builds upon the YouTube tutorial:  
📺 **https://youtu.be/H669Fwtv-3o**  
…and includes missing details, better prep steps, full links, and compatibility tips for Surface-specific hardware. Verified with **Ubuntu 24.04 LTS**.

---

## 🧹 Step 0: Prepare Your Surface Pro for Ubuntu

### 💾 1. Backup Important Data (Optional but Strongly Recommended)

Before you do anything else:

- **Back up your personal files** (e.g., Desktop, Documents, Downloads)
- If you plan to wipe Windows completely, make a **full system image**

Recommended tools:

- Macrium Reflect Free (Windows):  
  💾 **https://www.macrium.com/reflectfree**
- GParted for partition management:  
  💾 **https://gparted.org/livecd.php**
- rsync, dd, or Timeshift for Linux backups

Cloud or external USB/SSD backups are also good options.

➡️ **Reminder:** Data on deleted partitions is **non-recoverable**.

---

### ⚙️ 2. Enter Surface UEFI Settings (Before Booting USBs!)

1. Shut down the Surface
2. Hold **Volume Up** and tap **Power**
3. Release Power, keep holding Volume Up → UEFI menu appears

Change the following:

- **Boot Configuration**:
  - Enable **USB Boot**
  - Move USB device to **top priority**
- **Secure Boot**:
  - Set to **Disabled**

💡 *This must be done before trying to boot GParted or Ubuntu from USB!*

---

### 🖥️ 3. Optional: Connect to External Monitor

GParted Live and Ubuntu installers may show **tiny menus** due to the Surface's high DPI screen and default scaling.

✅ If available, connect the Surface to an **external monitor** via USB-C or Mini DisplayPort to improve readability during setup.  
✅ Alternatively, zoom levels and screen scaling can be adjusted later after installation.

---

### 🗂️ 4. Clean Up or Repartition the Internal SSD

#### Option A: Full Wipe (Erase Everything)

1. Boot into **GParted Live**:  
   🔗 **https://gparted.org/livecd.php**
2. Select your internal drive (likely `/dev/nvme0n1` or `/dev/sda`)
3. Right-click each partition → **Delete**
4. Leave space as “unallocated” or create one large empty partition
5. Click ✅ “Apply All Operations”

#### Option B: Dual Boot with Windows

1. Boot into Windows
2. Open **Disk Management** (`Win + X` → Disk Management)
3. Right-click on the main partition (usually C:) → **Shrink Volume**
4. Shrink by 30 GB or more for Ubuntu
5. Leave the new space **unallocated**

If BitLocker is active, **suspend it temporarily** via Control Panel.

---

## 🔧 Requirements Checklist

- ✅ USB stick (8 GB+)
- ✅ Surface Pro 5, 6, or 7
- ✅ Another computer for ISO + USB prep
- ✅ Internet access
- ✅ Optional: USB mouse/keyboard during install

---

## 📥 Step 1: Download Ubuntu ISO

Download from Ubuntu's official site:  
🌐 **https://ubuntu.com/download/desktop**

We recommend **Ubuntu 24.04 LTS** for best support.

---

## 💽 Step 2: Create a Bootable USB Drive

### On Windows (with Rufus):

Download Rufus:  
💻 **https://rufus.ie/**

Steps:
1. Open Rufus, select your USB and ISO
2. Use these settings:
   - Partition scheme: **GPT**
   - Target system: **UEFI (non-CSM)**
3. Click **Start**

### On Linux:

Use `Startup Disk Creator` or:

```bash
sudo dd if=ubuntu-24.04-desktop-amd64.iso of=/dev/sdX bs=4M status=progress
```

➡️ Replace `/dev/sdX` with your USB device (e.g. `/dev/sdb`)

---

## 🧪 Step 3: Install Ubuntu

1. Plug in USB and reboot
2. Ubuntu should start from USB

During install:

- Select **“Erase disk and install Ubuntu”** if doing a clean install
- Select **“Install Ubuntu alongside Windows Boot Manager”** for dual boot

Follow the rest of the install prompts.

---

## 🛠️ Step 4: Post-Install Tweaks (Highly Recommended)

Although this does **not fix screen rotation**, installing Tweaks is highly useful for:

- Adjusting UI scaling on high-resolution displays
- Managing appearance and fonts
- Enabling/Disabling extensions

Install it with:

```bash
sudo apt install gnome-tweaks
```

Then:

1. Open **Tweaks**
2. Go to **Displays**
3. Set **Scale** to 125% or 150% as preferred

---

## 🧰 Step 5: Install Surface Kernel, Touch Support, and Drivers

Open a Terminal after install and run:

```bash
sudo apt update
sudo apt install curl gnupg
curl -s https://packagecloud.io/install/repositories/linux-surface/linux-surface/script.deb.sh | sudo bash
sudo apt install linux-image-surface linux-headers-surface iptsd
sudo systemctl enable iptsd
sudo systemctl start iptsd
```

More details:  
📘 **https://github.com/linux-surface/linux-surface**

---

## 📸 Step 6: Camera Support

✅ Verified: Both front and back cameras work out-of-the-box with the linux-surface kernel.

If needed, consult:
📘 **https://github.com/linux-surface/linux-surface/wiki/Camera-Support**

---

## 🔄 Step 7: Final System Update

After setup, run:

```bash
sudo apt update
sudo apt upgrade
sudo reboot
```

---

## 💬 Surface Linux Resources

- Linux Surface GitHub:  
  🐧 **https://github.com/linux-surface/linux-surface**

- SurfaceLinux Subreddit:  
  💬 **https://www.reddit.com/r/SurfaceLinux/**

---

Let me know if you'd like to extend this with:
- 🔄 Screen rotation/Xorg workaround (separate file already available)
- 💡 Touchscreen gesture support
- 🧪 Trying Fedora or Arch on Surface
