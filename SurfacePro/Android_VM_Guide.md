# Running Android-x86 in a VM on Ubuntu 24.04 with QEMU/KVM (X11-Compatible)

This guide walks you through setting up a virtual machine running Android-x86 on **Ubuntu 24.04** using **QEMU/KVM**, optimized for systems running **X11** — such as the **Surface Pro 6** with the custom `linux-surface` kernel.

## ✅ Why This Guide?
- Works with **X11** (avoids Wayland-based solutions like Waydroid)
- Ensures **touchscreen and screen rotation** on host are not broken
- Enables **Google Play Store** (via Android-x86 ISO)
- Uses **QEMU/KVM** for near-native VM performance

---

## 📋 Prerequisites

### Hardware Requirements:
- A system with **Intel VT-x** or **AMD-V** (most modern CPUs, including Surface Pro 6)
- At least **4 GB RAM** (8 GB or more recommended)
- **10 GB+** free disk space

### Software Requirements:
- Ubuntu 24.04 (X11 session)
- `linux-surface` kernel (for touchscreen & rotation support)
- Internet connection (for downloads)

---

## 🧰 Step 1: Install QEMU, KVM, and virt-manager

Open a terminal and run:

```bash
sudo apt update
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager
```

Verify your user is in the `libvirt` and `kvm` groups:

```bash
groups $USER
```

If not, add your user:

```bash
sudo usermod -aG libvirt,kvm $USER
newgrp libvirt
```

Reboot for group changes to take full effect.

---

## 🌐 Step 2: Download Android-x86 ISO

Go to the official Android-x86 download page:

👉 https://www.android-x86.org/download.html

Download the latest **stable ISO**. For example:

- android-x86_9.0-r2.iso (based on Android 9 Pie)

Save it in a known folder, e.g., `~/Downloads`.

---

## 🏗️ Step 3: Create Android VM in virt-manager

1. Open **Virtual Machine Manager** (search for `virt-manager` in your app menu).
2. Click **`Create a new virtual machine`**.
3. Choose **`Local install media (ISO image)`**.
4. Click **Browse**, select the Android ISO.
5. Set OS type:
   - Type: `Generic Linux 2020 or later`
   - Arch: `x86_64`
6. Assign RAM and CPUs:
   - 2048 MB RAM (minimum), 4096 MB recommended
   - 2 CPUs minimum
7. Create a virtual disk:
   - Size: 8 GB or more (16+ GB recommended)
8. On final screen:
   - Check **Customize configuration before install**

Click **Finish**.

---

## ⚙️ Step 4: Tweak VM Configuration

In the configuration window:

### a) Change Firmware to BIOS
- In the left pane, click **Overview**
- Change **Firmware** to `BIOS` (not UEFI)

### b) Add USB Tablet (for better touch support)
- Click **Add Hardware > Input > USB Tablet**

### c) Set Display to Spice + QXL (optional for smooth GUI)
- Under **Display**, use `Spice` and QXL video

Click **Begin Installation**.

---

## 📲 Step 5: Install Android-x86

Once booted into the ISO:

1. Choose **Advanced options > Auto Installation** (or manual if preferred).
2. Let it install to the virtual disk.
3. After installation, **do not reboot immediately**. First, shut down the VM.

---

## 🛠️ Step 6: Final Boot Tweaks

1. Reopen the VM’s configuration.
2. Remove the ISO from the **CDROM/ISO** device.
3. Boot the VM.

You should now see Android boot up!

---

## 🛍️ Step 7: Sign into Play Store

Some builds of Android-x86 include Play Store out of the box.
If not:
- You can sideload **Open GApps** from https://opengapps.org/ (use x86 platform)
- Or install **Aurora Store** as an open-source Play Store client

---

## 💡 Tips

- Enable **mouse integration** for smoother input in VM.
- Touch may partially work in the VM depending on drivers.
- Use **fullscreen mode** in virt-manager (`View > Fullscreen`) for a better tablet experience.
- To copy files into the VM, use shared folders or upload APKs via browser.

---

## 📌 Useful Links

- Android-x86 Project: https://www.android-x86.org/
- Open GApps (optional): https://opengapps.org/
- virt-manager Manual: https://virt-manager.org/
- QEMU Project: https://www.qemu.org/
- Surface Linux Kernel: https://github.com/linux-surface/linux-surface

---

## ✅ You're Done!

You now have a fully working Android VM on your Ubuntu 24.04 system running **X11**, without breaking your touchscreen or rotation setup. This forms part of your ultimate hybrid setup: Microsoft hardware + Linux OS + Google Android (in VM).
