# Ubuntu Installation on PC (amd64)

Minimal Installation is sufficient for Personal Robot OS


## Hardware Accelerated OpenGL

### Nvidia Drivers

see: https://linuxconfig.org/how-to-install-the-nvidia-drivers-on-ubuntu-22-04

```
ubuntu-drivers devices # to detect graphics card and recommended drivers
sudo ubuntu-drivers autoinstall # to install recommended drivers
sudo apt autoremove # to remove redundant packages
reboot
```

### `glxinfo` and `glxgears`

Test OpenGL with `glxinfo`. Fix the failure below

```
name of display: :0
X Error of failed request:  BadValue (integer parameter out of range for operation)
  Major opcode of failed request:  152 (GLX)
  Minor opcode of failed request:  24 (X_GLXCreateNewContext)
```

by installing the recommended driver as described above and rebooting.

NOTE: sometimes GL driver may crash and a reboot may be required?



