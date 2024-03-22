# addNvidiaDrivers

* Got it from [here](https://dev.to/vitorvargas/how-to-install-the-nvidia-driver-on-archlinux-5bgc).

## 1. Check the archicture of the NVIDIA GPU in pc:

```
lspci | grep -E "VGA|3D
```

## 2. Update the system

```
sudo pacman -Syu --noconfirm
```

## 3. Installation of Required Packages

```
sudo pacman -S --noconfirm nvidia nvidia-utils nvidia-settings opencl-nvidia xorg-server-devel
```

## 4. Xorg Configuration

```
mkdir -p /etc/X11/xorg.conf.d
cd /etc/X11/xorg.conf.d
nano 10-nvidia-drm-outputclass.conf
```

Paste this:

```
EOF
Section "OutputClass"
    Identifier "intel"
    MatchDriver "i915"
    Driver "modesetting"
EndSection

Section "OutputClass"
    Identifier "nvidia"
    MatchDriver "nvidia-drm"
    Driver "nvidia"
    Option "AllowEmptyInitialConfiguration"
    Option "PrimaryGPU" "yes"
    ModulePath "/usr/lib/nvidia/xorg"
    ModulePath "/usr/lib/xorg/modules"
EndSection
EOF
```

## 5. GDM (GNOME Display Manager) Configuration

```
mkdir -p /usr/share/gdm/greeter/autostart

cd /usr/share/gdm/greeter/autostart

sudo nano optimus.desktop
```

Paste this:

```
EOF
[Desktop Entry]
Type=Application
Name=Optimus
Exec=sh -c "xrandr --setprovideroutputsource modesetting NVIDIA-0; xrandr --auto"
NoDisplay=true
X-GNOME-Autostart-Phase=DisplayServer
EOF
```
### Second command
```
mkdir -p /etc/xdg/autostart

cd /etc/xdg/autostart

sudo nano optimus.desktop
```

Paste this:

```
EOF
[Desktop Entry]
Type=Application
Name=Optimus
Exec=sh -c "xrandr --setprovideroutputsource modesetting NVIDIA-0; xrandr --auto"
NoDisplay=true
X-GNOME-Autostart-Phase=DisplayServer
EOF
```

## 6. Modprobe Configuration

```
mkdir -p /etc/modprobe.d

cd /etc/modprobe.d

sudo nano nvidia-drm-nomodeset.conf
```

Paste this:

```
EOF
options nvidia-drm modeset=1
EOF
```

## 7. Initramfs Reconstruction

```
sudo mkinitcpio -P
```

## 7. Running nvidia-xconfig

```
sudo nvidia-xconfig
```

## 8. Testing and Verifying the NVIDIA Driver

Restart the system to apply all configurations.