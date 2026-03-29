# Disclaimer

The script is as-is and was in general just for personal use, I set this git up so others who stumble upon might find it useful too. I personally have no experience in python and the script was made by 99% with ChatGPT (with a few hours of troubleshooting and feeding it with outputs). So expect a spaghetti code... sorry.

# Description

I wanted a tool that works systemwide for any game so I don't always have to rely on SteamInput/Bottles/Lutris. Works wired and via Bluetooth. Works for PS4 and PS5 controller.

It in general reads the raw input from the PS controllers and sends it as a virtual controller in uinput (xinput), which I needed especially in older games which don't support DirectInput at all.

**It only works for one active controller!**

# Installing
Download the files manually or clone
```
cd ~
git clone https://github.com/sera-ina/DualShock-uinput.git
```

## Requirements
### packages
- python3
- python3-dev
- python3-venv
### pip
- python_uinput
- evdev
### system
- bluetoothctl
  - if desired to be able to disconnect the controller via (PS + Start) combination
### dependency installation
- for an ubuntu based distro, use:
```
sudo apt update
sudo apt install python3-dev python3-venv
```
note: rest of the dependencies are not needed in my testing. might have to use ```bluez``` instead of ```bluetoothctl``` for ubuntu based distros.

## Preparation

Once you installed python on your system, create a virtual enviroment (for example ".venv" in your home folder)
```
python3 -m venv ~/.venv
```
Update pip
```
~/.venv/bin/pip install --upgrade pip setuptools wheel
```
When it is done, install the dependencies manually or with the requirements.txt file
```
~/.venv/bin/pip install -r ~/DualShock-uinput/requirements.txt
```
Create udev rules file
```
sudo nano /etc/udev/rules.d/99-psinput.rules
```
and add
```
KERNEL=="uinput", MODE="0660", GROUP="input"
```
Add user to input group
```
sudo usermod -aG input $USER
```
Load uinput module
```
sudo modprobe uinput
```
Reload udev rules
```
sudo udevadm control --reload-rules
sudo udevadm trigger
```

# Usage

**Steps:**
- **Connect you PS4/PS5 controller first via USB or Bluetooth**, then run the script
```
~/.venv/bin/python ~/DualShock-uinput/ds4input.py
```
as the script looks for the controller directly on start else the script will just stop with an error.

# Additional Infos
- No vibration / force feedback
- The PS button is a separate button that you can map, for example in AntiMicroX
- In the config.py file you can change the deadzone of each stick, the name of the controller and if you want to be able to use the (PS + Start) combo to disconnect the controller.
- If you want your GNOME desktop enviornment to show the battery of the controller, use this extension (it will appear in the top bar): https://extensions.gnome.org/extension/1283/dual-shock-4-battery-percentage/
