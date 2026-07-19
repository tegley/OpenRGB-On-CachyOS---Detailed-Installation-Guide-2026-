# Detailed Installation Guide
## Documentation Links
OpenRGB:
- [GitLab - README](https://gitlab.com/CalcProgrammer1/OpenRGB/-/blob/master/README.md?ref_type=heads)
- [GitLab - SMBus Access](https://gitlab.com/CalcProgrammer1/OpenRGB/-/blob/master/Documentation/SMBusAccess.md?ref_type=heads)
- [GitLab - KernelParameters](https://gitlab.com/CalcProgrammer1/OpenRGB/-/blob/master/Documentation/KernelParameters.md?ref_type=heads)
- [Website - Supported Devices](https://openrgb.org/devices_0.9.html)
OpenLinkHub:
- [GitHub - README](https://github.com/jurkovic-nikola/OpenLinkHub/blob/main/README.md)
- [OpenRGB Integration](https://github.com/jurkovic-nikola/OpenLinkHub/blob/main/openrgb/README.md)
Other:
- [Helpful Reddit thread](https://www.reddit.com/r/archlinux/comments/1kl9g4o/openrgb_and_ram/)
- [Arch Linux info on GRUB]

## Application Installations
1. Open CachyOS Hello: Install Apps → Repo → Search 'openrgb' → Install
2. Check to see if UDev rules installed properly, they should be located here:
```
/usr/lib/udev/rules.d/60-openrgb.rules
```
If you have Corsair iCUE Products)
3. Open CachyOS Hello: Install Apps → Repo → Search 'openlinkhub' → Install

## For those with Gigabyte Motherboards With APCI Conflict (see Gitlab - Kernel Parameters)
### 1. Set kernel parameters
1. Open the GRUB configuration file:
```
sudo nano /etc/default/grub
```
2. Find this line:
```
GRUB_CMDLINE_LINUX=""
```
3. Add `acpi_enforce_resources=lax` between both ", type out exactly as below:
```
GRUB_CMDLINE_LINUX="acpi_enforce_resources=lax"
```
4. Save and close the file (CTRL+O → Enter → CTRL+X)
### 2. Regenerate GRUB
```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
### 3. Restart your system.

## For those with RGB RAM Modules and Other I2C Devices (see Gitlab - SMBus Access)
### 1. Load I2C modules
After the reboot
```
sudo modprobe i2c-dev
```


- then for AMD:
```
sudo modprobe i2c-piix4
```
- or for Intel:
```
sudo modprobe i2c-i801
```
## 5. Enable modules permanently
Create file:
```
sudo nano /etc/modules-load.d/i2c.conf
```
- for AMD enter content:
```
i2c-dev
i2c-piix4
```
- or for Intel enter content:
```
i2c-dev
i2c-i801
```
Save and close the file (CTRL+O → Enter → CTRL+X)
## 6. Set user rights for I²C
1. Create rule:
```
sudo nano /etc/udev/rules.d/60-i2c.rules
```
2. Enter content:
```
SUBSYSTEM=="i2c", KERNEL=="i2c-*", MODE="0660", GROUP="i2c"
```
Save and close the file (CTRL+O → Enter → CTRL+X)

3. Create group and add user:
```
sudo groupadd i2c
sudo usermod -aG i2c $USER
```
4. Reload rules:
```
sudo udevadm control --reload-rules
sudo udevadm trigger
```
5. Log out and log back in, or reboot
## 7. Setup autostart (if you want)
1. Find OpenRGB (usually */usr/bin/openrgb*)
```
which openrgb
```
2. Check where your OpenRGB profile is saved, for example
```
/home/yourusername/.config/openrgb/myprofileXY.orp
```
3. Create autostart folder
```
mkdir -p ~/.config/autostart
```
4. Create file for OpenRGB
```
nano ~/.config/autostart/openrgb.desktop
```
5. Insert the following content (adjust the path to OpenRGB and your OpenRGB profile)
```
[Desktop Entry]
Type=Application
Name=OpenRGB
Exec=/usr/bin/openrgb --profile /home/yourusername/.config/openrgb/myprofileXY.orp
X-GNOME-Autostart-enabled=true
Comment=Start OpenRGB with my RGB profile
```
6. Reboot
