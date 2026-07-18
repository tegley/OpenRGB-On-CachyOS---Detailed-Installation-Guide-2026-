# Making RAM visible for OpenRGB on Arch Linux
## 1. Set kernel parameters
1. Open the GRUB configuration file:
```
sudo nano /etc/default/grub
```
2. Find this line:
```
GRUB_CMDLINE_LINUX=""
```
3. Add `acpi_enforce_resources=lax` between both ", do not remove anything:
```
GRUB_CMDLINE_LINUX="acpi_enforce_resources=lax"
```
4. Save and close the file (CTRL+O → Enter → CTRL+X)
## 2. Regenerate GRUB
```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
## 3. Restart system
```
reboot
```
## 4. Loading I²C modules
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
