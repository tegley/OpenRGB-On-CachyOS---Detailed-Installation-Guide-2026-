# Detailed Installation Guide
## Documentation Links
OpenRGB:
- [README](https://gitlab.com/CalcProgrammer1/OpenRGB/-/blob/master/README.md?ref_type=heads)
- [GitLab - SMBus Access](https://gitlab.com/CalcProgrammer1/OpenRGB/-/blob/master/Documentation/SMBusAccess.md?ref_type=heads)
- [GitLab - KernelParameters](https://gitlab.com/CalcProgrammer1/OpenRGB/-/blob/master/Documentation/KernelParameters.md?ref_type=heads)
- [Website - Supported Devices](https://openrgb.org/devices_0.9.html)

OpenLinkHub:
- [README](https://github.com/jurkovic-nikola/OpenLinkHub/blob/main/README.md)
- [OpenRGB Integration](https://github.com/jurkovic-nikola/OpenLinkHub/blob/main/openrgb/README.md)

Resizing Addressable RGB Zones:
- [Arctic Liquid Freezer III Pro 360 ARGB - Led Count](https://gitlab.com/CalcProgrammer1/OpenRGB/-/work_items?sort=created_date&state=opened&search=%5BFeature+Request%5D&first_page_size=20&show=eyJpaWQiOiI1NDc3IiwiZnVsbF9wYXRoIjoiQ2FsY1Byb2dyYW1tZXIxL09wZW5SR0IiLCJpZCI6MTg0NzQxMzM1fQ%3D%3D)
- [7900 XTX Nitro+ - Led Count](https://gitlab.com/CalcProgrammer1/OpenRGB/-/work_items?sort=created_date&state=opened&search=7900+xtx&first_page_size=20&show=eyJpaWQiOiI0ODE3IiwiZnVsbF9wYXRoIjoiQ2FsY1Byb2dyYW1tZXIxL09wZW5SR0IiLCJpZCI6MTY3NzUyNDA5fQ%3D%3D)
- [Corsair RS120 ARGB - Led Count](https://www.corsair.com/us/en/p/case-fans/co-9050180-ww/rs120-argb-120mm-pwm-fan-co-9050180-ww)

Other:
- [Arch Linux Wiki - Info on GRUB](https://wiki.archlinux.org/title/Kernel_parameters#GRUB)
- [Helpful Reddit thread](https://www.reddit.com/r/archlinux/comments/1kl9g4o/openrgb_and_ram/)
- [Another helpful Reddit thread](https://www.reddit.com/r/linuxquestions/comments/10n1chc/trying_to_get_my_i2cdev_and_i2cpiix4_to_load/)

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
3. Add `acpi_enforce_resources=lax` between both double quotes (" "), should be EXACTLY as below:
```
GRUB_CMDLINE_LINUX="acpi_enforce_resources=lax"
```
4. Save and close the file (CTRL+O → Enter → CTRL+X)
### 2. Regenerate GRUB
```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
### 3. Restart your system.

## For those with RGB RAM Modules or Other I2C Devices (see Gitlab - SMBus Access)
### 1. Open Konsole and load I2C modules
```
sudo modprobe i2c-dev
```
### 2. Enter following commands based according to your motherboard:
| For AMD Motherboards  | For Intel Motherboards |
| ------------- | ------------- |
| `sudo modprobe i2c-piix4`  | `sudo modprobe i2c-i801`  |
| `sudo nano /etc/modules-load.d/i2c.conf` | `sudo nano /etc/modules-load.d/i2c.conf` |
| `i2c-dev` <br> `i2c-piix4` |`i2c-dev`<br>`i2c-i801`|
Save and close the file (CTRL+O → Enter → CTRL+X)

### 3. Give user access to piix4/i801 controllers
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
4. Reload UDev rules:
```
sudo udevadm control --reload-rules
sudo udevadm trigger
```
### 4. Restart your system.

## Create application shortcuts
[Desktop Entry]
Type=Application
Name=OpenRGB
Exec=/usr/bin/openrgb --profile /home/yourusername/.config/openrgb/myprofileXY.orp
X-GNOME-Autostart-enabled=true
Comment=Start OpenRGB with my RGB profile
```
6. Reboot
