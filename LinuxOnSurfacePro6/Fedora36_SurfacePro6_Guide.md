# 🐧 Installing Fedora 36 on Surface Pro 6 – Complete Guide

## 🌟 Why Fedora 36 is the Best Baseline

Fedora 36 strikes the perfect balance for Surface Pro 6 users:

- ✅ **Excellent Hardware Compatibility:** Touchscreen, pen, sensors, and auto-rotation work better than in later Fedora releases.
- ✅ **Wayland and X11 Support:** Choose the best session for your needs — X11 is recommended if multitouch or screen rotation is buggy on Wayland.
- ✅ **Upgradable:** Once Fedora 36 is installed and working well, you can choose to upgrade to a newer Fedora version.
- ✅ **Surface Kernel Support:** The `linux-surface` team provides a compatible kernel with better support for Surface devices.

This guide was tested out on Surface Pro 6 but would likely also work with other Surface Pro generations like Surface Pro 5.

---

## 🧰 What You’ll Need

- Surface Pro 6 with **Secure Boot disabled**
- USB stick (8GB or larger)
- **Fedora 36 ISO**:  
  [📥 Download Fedora 36 Workstation (x86_64)](https://dl-iad02.fedoraproject.org/pub/archive/fedora/linux/releases/36/Workstation/x86_64/iso/)
- A Windows 10/11 PC with [Rufus](https://rufus.ie)

---

## 🪟 Step 1: Create a Fedora USB Stick on Windows

1. Insert your USB stick.
2. Open **Rufus**.
3. Configure the following:
   - **Device:** Your USB stick
   - **Boot selection:** Select the downloaded Fedora 36 ISO
   - **Partition scheme:** GPT
   - **Target system:** UEFI (non-CSM)
4. Click **Start** and wait for it to complete.
5. Eject the USB stick safely.

---

## 💻 Step 2: Boot the Surface Pro 6 from USB

1. Plug in the Fedora USB stick.
2. Press **Volume Down + Power** to enter UEFI Boot Menu.
3. Select the USB stick as boot device.
4. When GRUB appears:
   - If it hangs, press `e` to edit the boot entry.
   - Find the line starting with `linuxefi`.
   - Add this to the end of that line:

     ```
     fbcon=rotate:1
     ```

   - Press `Ctrl + X` or `F10` to boot.

---

## 🛠️ Step 3: Install Fedora 36

1. Launch the **Install to Hard Drive** wizard.
2. Select your language and region.
3. On disk selection:
   - Choose **Custom (Advanced)** if needed.
   - Use **GPT** partitioning (recommended for UEFI).
4. Set hostname, user, and password.
5. Begin installation.
6. Reboot after installation and remove the USB stick.

---

## 🧩 Step 4: Install Tools and Surface Kernel (Optional but Recommended)

### 🔓 Enable RPM Fusion:

```bash
sudo dnf install \
  https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-36.noarch.rpm \
  https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-36.noarch.rpm
```

### 🔧 Install Utilities:

```bash
sudo dnf install iio-sensor-proxy libwacom libinput xinput evemu xrandr
```

### 🧬 Install the Surface Kernel:

```bash
sudo dnf config-manager --add-repo=https://pkg.surfacelinux.com/fedora/linux-surface.repo
sudo rpm --import https://pkg.surfacelinux.com/fedora/keys.asc
sudo dnf update -y
sudo dnf install kernel-surface
sudo grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg
sudo reboot
```

---

## 💡 Step 5: (Optional) Switch to X11 (for multitouch/rotation fixes)

1. Edit GDM config:

```bash
sudo nano /etc/gdm/custom.conf
```

2. Uncomment or add:

```ini
[daemon]
WaylandEnable=false
```

3. Save and reboot:

```bash
sudo reboot
```

---

## ✅ Summary

By using Fedora 36, you:
- Avoid bugs in newer Fedora kernels.
- Gain full pen, touch, and sensor support with optional `linux-surface` kernel.
- Retain the flexibility of upgrading later, once things are working smoothly.

Once you're happy with the setup, you can explore upgrading to Fedora 38+ using:

```bash
sudo dnf system-upgrade download --releasever=38 --allowerasing
sudo dnf system-upgrade reboot
```

Enjoy your fully working Linux-powered Surface Pro 6! 🐧
