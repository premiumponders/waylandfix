# KDE Plasma + Wayland Reset and Reinstall Script (Debian-based)

This script fully removes KDE, SDDM, Wayland, and X11 components, and reinstalls a clean minimal Plasma Wayland desktop environment.

## How to use:
1. Save this script as `reset_plasma_wayland.sh`
2. Make it executable:
   chmod +x reset_plasma_wayland.sh
3. Run it:
   ./reset_plasma_wayland.sh

---

```bash
#!/bin/bash

# Full KDE Plasma + Wayland Reset and Minimal Reinstall Script for Debian

echo ">>> PHASE 1: Purging all KDE, SDDM, Wayland, X11, and related components..."

sudo apt purge --autoremove -y \
    plasma-desktop plasma-workspace sddm kwin* kde-plasma-desktop kde-standard kde-cli-tools \
    plasma-* wayland* xwayland xserver-xorg* qt5* qt6* qtbase5* qml* libqt5* libqt6* \
    qdbus* systemsettings* libxcb* lightdm* task-kde-desktop

echo ">>> Cleaning up leftover packages and cache..."
sudo apt clean
sudo apt autoclean

echo ">>> PHASE 2: Reboot recommended. Press enter to continue when ready..."
read

echo ">>> PHASE 3: Installing minimal Plasma Wayland environment..."
sudo apt update
sudo apt install -y sddm plasma-desktop plasma-workspace-wayland kde-plasma-desktop kwin-wayland \
    qtwayland5 qt5dxcb-plugin qml-module-org-kde-kirigami2 qml-module-qtquick-virtualkeyboard

echo ">>> Installing session and display server tools..."
sudo apt install -y xwayland plasma-workspace sddm-theme-breeze kscreen

echo ">>> Enabling SDDM login manager..."
sudo systemctl enable sddm

echo ">>> PHASE 4: Configuring user session for Wayland..."
echo -e "[Desktop]\nSession=plasmawayland" > ~/.dmrc
chmod 644 ~/.dmrc

echo ">>> Removing saved KDE session to avoid crashes..."
rm -rf ~/.config/session/

echo ">>> PHASE 5: Rebooting to launch fresh Plasma Wayland session..."
sudo reboot
```
