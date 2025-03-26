# The very inofficial MacOS Virtual Machine on Ubuntu (Unofficial Hackintosh VM Guide)

> ⚠️ **DISCLAIMER**: This guide is for educational purposes only. Running macOS on non-Apple hardware violates Apple's End User License Agreement (EULA). Proceed at your own risk. This method is not officially supported by Apple, and may stop working at any time due to software updates or changes in macOS validation mechanisms.

---

## Introduction
This guide shows how to run macOS as a virtual machine on Ubuntu using QEMU and KVM. It avoids the need for dual booting or installing macOS directly on your hardware (which is far riskier). The macOS VM will run via virtualization with OpenCore bootloader support to bypass Apple's hardware checks.

Tested on: **Ubuntu 24.04 LTS** with **Intel CPU** supporting VT-x/VT-d.

---

## Step 1: Install Required Packages
```bash
sudo apt update
sudo apt install qemu-kvm libvirt-clients libvirt-daemon-system virt-manager bridge-utils git python3
```

---

## Step 2: Verify or Install Python

The `fetch-macOS.py` script requires **Python 3** to run.

### Check if Python is already installed:
```bash
python3 --version
```
If Python is installed, you’ll see a version like `Python 3.12.2`. If not, install it:

### Install Python (if needed):
```bash
sudo apt install python3
```

> 📝 Note: If Python 3 is already installed, you can skip this step.

---

## Step 3: Add User to Required Groups
```bash
sudo usermod -aG kvm $USER
sudo usermod -aG libvirt $USER
```
Then log out and back in (or reboot).

Check virtualization support:
```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
```
If the result is greater than 0, you're good to go.

---

## Step 4: Clone the OSX-KVM Repository
```bash
git clone https://github.com/kholia/OSX-KVM.git
cd OSX-KVM
```
This repository provides the base configuration and scripts for running macOS via QEMU.

---

## Step 5: Download macOS Installer
Run the fetch script to download a version of macOS (Catalina, Big Sur, Monterey, Ventura, etc.):
```bash
python3 fetch-macOS-v2.py
```
Choose your desired version when prompted. This uses Apple's official software catalog.

---

## Step 6: Create the Installation ISO
Use the provided script to convert the installer to a bootable ISO:
```bash
./make.sh --iso
```
This will generate something like `macOS-Catalina.iso` or similar.

---

## Step 7: Configure and Launch the VM
You can use the sample launch scripts, like `OpenCore-Boot.sh` or `basic.sh`. Customize CPU, RAM, and disk paths:

Edit `OpenCore-Boot.sh` as needed:
```bash
# Example edits in the script
-cpu host
-smp 4,cores=2
-m 8192
-drive file=macOS.qcow2,format=qcow2
```
Then launch the VM:
```bash
./OpenCore-Boot.sh
```
You’ll boot into the OpenCore interface, then choose the macOS installer.

---

## Step 8: Install macOS in the VM
Once in the installer:
1. Open Disk Utility.
2. Erase the virtual disk (e.g., `macOS.qcow2`) and format it as **APFS**.
3. Proceed with the installation.

The VM will reboot several times during install — just wait it out and keep selecting the installer until it boots into macOS.

---

## Step 9: Post-Install Notes
- You may need to reconfigure OpenCore to boot from your installed system.
- Consider creating a snapshot of your VM after a successful install.
- For better performance, consider enabling VirtIO GPU, CPU passthrough, or using SPICE.

---

## Running the macOS VM from External Storage

Running the VM from high-speed external storage can be a great performance and portability boost.

### Option A: Use a USB 3.2 Stick

Using a fast USB stick (like a Samsung USB 3.2 bar) is convenient — plug and play across systems.

#### **Preparation:**
1. Format the USB stick as **ext4** using GParted or `mkfs.ext4`.
2. Mount it:
```bash
sudo mkdir /mnt/macusb
sudo mount /dev/sdX1 /mnt/macusb
```
Replace `/dev/sdX1` with your actual device.

#### **Move the VM Directory:**
```bash
mv ~/OSX-KVM /mnt/macusb/OSX-KVM
cd /mnt/macusb/OSX-KVM
```

#### **Update Scripts:**
Edit launchers like `OpenCore-Boot.sh`:
```bash
-drive file=/mnt/macusb/OSX-KVM/macOS.qcow2,format=qcow2
```

#### **Make It Portable:**
- Use a `.desktop` launcher or mount script.
- Optionally configure `/etc/fstab` for auto-mounting.

#### **Performance Tip:**
Use only **USB 3.0/3.2 ports** and native Linux filesystems like `ext4` or `btrfs`. Avoid NTFS or FAT32.

### Option B: Use the Built-In MicroSD Card Slot

If your Surface Pro 6 has a **high-speed UHS-I or UHS-II 1TB MicroSD card**, you can store and run your VM from it just like the USB option.

#### **Preparation:**
1. Format the MicroSD card as **ext4**.
2. Mount it:
```bash
sudo mkdir /mnt/macsd
sudo mount /dev/mmcblk0p1 /mnt/macsd
```
Check `lsblk` to verify the exact device path (commonly `/dev/mmcblk0p1`).

#### **Move the VM Directory:**
```bash
mv ~/OSX-KVM /mnt/macsd/OSX-KVM
cd /mnt/macsd/OSX-KVM
```

#### **Update Scripts:**
```bash
-drive file=/mnt/macsd/OSX-KVM/macOS.qcow2,format=qcow2
```

#### **Performance Tip:**
The internal MicroSD interface is often faster and more stable than external USB if using a high-quality UHS card.

---

## Known Limitations
- No support for Apple-exclusive services like iMessage, FaceTime, AirDrop.
- Touchscreen, pen, and some sensors will **not** work.
- Graphics acceleration is limited (no Metal support).
- You may experience instability with macOS updates.

---

## Optional Enhancements
- Add shared folders using 9p or Samba.
- Use virt-manager for GUI-based VM management.
- Add QEMU Guest Agent for shutdown support and clipboard sharing.

---

## Final Thoughts
Running macOS in a VM is a great way to test or develop for Apple platforms on non-Apple hardware. It’s more stable than dual-boot Hackintosh setups and doesn’t risk your native OS install. Just remember:

- This is **not supported or endorsed by Apple**.
- Expect bugs and breakages with updates.
- Back up your VM image regularly.

---

Happy (unofficial) virtual Hackintoshing! 🍏
