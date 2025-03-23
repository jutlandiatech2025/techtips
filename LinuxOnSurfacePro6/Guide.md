```markdown
# 🐧 Installing Fedora 36 on Surface Pro 6 – Full Guide

## 🌟 Why Fedora 36 is the Best Baseline

Installing Linux on the Surface Pro 6 can be tricky due to hardware quirks and regressions in newer kernels. Fedora 36 offers a perfect balance:

- ✅ **Best Compatibility:** Touch, pen input, iio-sensor-proxy, and rotation work reliably.
- ✅ **Wayland and X11 Options:** Works well with both — X11 often fixes touch/rotation issues.
- ✅ **Update-Friendly:** Once installed, you can safely upgrade to newer versions.
- ✅ **Surface Kernel Support:** The `linux-surface` kernel is tested and well-supported.

---

## 🧰 Requirements

- Surface Pro 6 with **Secure Boot disabled**
- USB stick (8GB or larger)
- Fedora 36 ISO: [Download Fedora 36 Workstation (x86_64)](https://dl-iad02.fedoraproject.org/pub/archive/fedora/linux/releases/36/Workstation/x86_64/iso/)
- [Rufus](https://rufus.ie) on your Windows PC

---

## 🪟 Step 1: Create Fedora USB Stick (on Windows)

1. Plug in your USB stick.
2. Launch **Rufus**.
3. Select:
   - **Device:** your USB stick
   - **Boot selection:** Fedora 36 ISO
   - **Partition scheme:** **GPT**
   - **Target system:** UEFI (non-CSM)
4. Click **Start** and let it write the image.
5. Eject the USB safely when done.

---

## 💻 Step 2: Boot Surface Pro 6 from USB

1. Plug in the USB stick.
2. Hold **Volume Down** and press **Power** to enter UEFI boot.
3. Select the USB device to boot.
4. When GRUB appears:
   - If it hangs after selecting the default, hit `e`.
   - Locate the line starting with `linuxefi`.
   - Add this at the end:

     ```
     fbcon=rotate:1
     ```

   - Press `Ctrl + X` or `F10` to boot.

---

## 🛠️ Step 3: Install Fedora 36

1. Start the **Install to Hard Drive** wizard.
2. Choose language and region.
3. Choose installation disk:
   - Use **Custom (Advanced)** to partition manually.
   - GPT recommended for UEFI.
4. Set:
   - Hostname
   - Username and password
5. Begin installation.
6. Reboot after completion and remove USB stick.

---

## ⏳ Step 4: Update the System

```bash
sudo dnf update --refresh -y
sudo dnf upgrade --refresh -y
sudo reboot
```

---

## 🧩 Step 5: Essential Packages and Surface Kernel (Optional but Recommended)

### Enable RPM Fusion:

```bash
sudo dnf install \
  https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-36.noarch.rpm \
  https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-36.noarch.rpm
```

### Install tools:

```bash
sudo dnf install iio-sensor-proxy libwacom libinput xinput evemu xrandr
```

### Install Surface Linux Kernel:

```bash
sudo dnf config-manager --add-repo=https://pkg.surfacelinux.com/fedora/linux-surface.repo
sudo rpm --import https://pkg.surfacelinux.com/fedora/keys.asc
sudo dnf update -y
sudo dnf install kernel-surface
sudo grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg
sudo reboot
```

---

## 💡 Step 6: (Optional) Switch to X11 for Touchscreen Fixes

1. Edit GDM config:

   ```bash
   sudo nano /etc/gdm/custom.conf
   ```

2. Uncomment or add:

   ```ini
   WaylandEnable=false
   ```

3. Save and reboot.

---

## ✅ Summary

- Fedora 36 is the **best base** for Linux on Surface Pro 6 right now.
- It gives you working:
  - Touch
  - Pen input
  - Rotation
  - Ambient light sensor
- Post-install upgrades are easy and safe.
- Can later move to Fedora 37/38 once confident.

---

**Enjoy Fedora 36 on your Surface Pro 6!**  
Let me know if you want to automate some of these steps or share your own version of the guide!
```
