# GPU Passthrough on Mac Pro 7,1 (Proxmox VE 9.1.2)

This repository documents the first successful case of **GPU passthrough** on the Mac Pro 7,1 under Proxmox VE, validated with piKVM.

---

## 🖥 Environment Setup
- **Hardware:** Mac Pro 7,1 (2019)
- **GPU:** AMD RX 580 / Pro 580X
- **Hypervisor:** Proxmox VE 9.1.2 (installed from 9.1.1 ISO, upgraded via `apt`)
- **Kernel:** 6.17.2‑2‑pve‑t2 (AdiityaGarg8’s T2‑patched kernel)
- **Validation:** piKVM for video output + USB passthrough for input

---

## 🔍 Identify Devices
```bash
lspci -nn | grep -i amd
# Confirms GPU PCI IDs (e.g. 1002:67df)

lspci | grep USB
lsusb -t
# Lists USB controllers for passthrough (keyboard/mouse)

⚙️ Configure VFIO
/etc/modprobe.d/vfio.conf
options vfio-pci ids=1002:67df,1002:aaf0 disable_vga=1
Bind GPU + HDMI audio functions to VFIO.

/etc/modprobe.d/blacklist.conf
blacklist radeon
blacklist amdgpu
blacklist drm
Prevent host drivers from grabbing GPU.

/etc/modprobe.d/kvm.conf
options kvm ignore_msrs=1 report_ignored_msrs=0
Avoid MSR errors when passing GPU to VM.

/etc/modules
vfio
vfio_iommu_type1
vfio_pci
Ensure VFIO modules load at boot.

🔄 Rebuild Initramfs
update-initramfs -u
reboot
Applies VFIO + blacklist configs.

✅ Verify Binding
lspci -k -nn -d 1002:67df
# Confirms GPU bound to vfio-pci

🖥 VM Configuration
Assign GPU PCI device(s) to Ubuntu VM
Assign USB controller for input passthrough
Boot VM with OVMF (UEFI) firmware

🔎 Validation
piKVM: Confirms real video output from GPU passthrough
USB passthrough: Provides keyboard/mouse input

Inside Ubuntu VM:
glxinfo | grep "renderer"
# Shows AMD Radeon RX 580 (hardware acceleration)

📓 Notes
RDP sessions show CPU rendering (llvmpipe), but GPU acceleration is active in VM.
piKVM provides direct proof of GPU output.
Setup offloads graphics workloads from CPU, improving overall VM performance.

🔊 Audio
GNOME RDP sessions show CPU rendering, but audio still comes from the AMD GPU’s HDMI/DP audio function.
RDP forwards audio independently of graphics, so passthrough audio remains intact.
Blacklisting snd_hda_intel is unnecessary.

🎉 Achievement

✅ First documented success case of GPU passthrough on Mac Pro 7,1 under Proxmox VE, validated with piKVM.


Proxmox Edge Kernels
Custom Linux kernels for Proxmox VE 9 — forked to add support for T2 Macs.This fork contains CI setup to compile kernels using scripts and documentation from:
fabianishere/pve-edge-kernel
proxmox/pve-kernel
t2linux/linux-t2-patches
See the t2linux wiki for additional help.Fan control guide: here.

📦 Installation
Download the required kernel from Releases and install:
apt install ./pve-kernel-VERSION_amd64.deb
Note: This fork uses tested scripts from fabianishere and proxmox. New kernels will only be released when they release updates.

🛠 Building Manually
Refer to the CI workflow.

Prerequisites:
apt install devscripts debhelper equivs git

🗑 Removal
apt remove pve-kernel-6.5*t2 pve-headers-6.5*t2

🙏 Credits
fabianishere
t2linux
