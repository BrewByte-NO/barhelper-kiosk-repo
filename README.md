# Barhelper Kiosk

Barhelper Kiosk turns a Raspberry Pi into a dedicated kiosk that launches Barhelper automatically in fullscreen.

For use with https://www.barhelper.app/

## Install Raspberry Pi OS

The easiest and official way is to use Raspberry Pi Imager.

Raspberry Pi Imager  
https://www.raspberrypi.com/software/

Official documentation, Getting started  
https://www.raspberrypi.com/documentation/computers/getting-started.html

Short version
1. Install Raspberry Pi Imager on your PC or Mac
2. Select your Raspberry Pi model, Raspberry Pi OS, and storage device
3. Write the image to an SD card or SSD
4. Boot the Pi and complete the initial setup

Tips  
On Pi 3+ we recommend Raspberry Pi OS 64 bit.  
On Pi 4 and Pi 5 we recommend Raspberry Pi OS 64 bit.

## Install Barhelper Kiosk

Made for copy paste in a terminal.

### Step 1 Update the system

Run this first

~~~bash
sudo apt update
sudo apt full-upgrade -y
sudo reboot
~~~

### Step 2 Option A, easiest, unsigned

After the Pi has rebooted, run this

~~~bash
echo "deb [trusted=yes] https://brewbyte-no.github.io/barhelper-kiosk-repo/ ./" | sudo tee /etc/apt/sources.list.d/barhelper-kiosk.list
sudo apt update
sudo apt install -y barhelper-kiosk
sudo reboot
~~~

## Updates

To update, run the following commands

~~~bash
sudo apt update
sudo apt upgrade
~~~

## Uninstall

If you want to uninstall

~~~bash
sudo apt remove barhelper-kiosk
sudo apt autoremove
~~~

## Test matrix

The table below is meant to be updated as you test more models and OS variants.

<table>
  <thead>
    <tr>
      <th>Raspberry Pi model</th>
      <th>Pi OS Trixie 64 bit</th>
      <th>Pi OS Trixie 32 bit</th>
      <th>Pi OS Trixie Lite 64 bit</th>
      <th>Pi OS Trixie Lite 32 bit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Pi 5</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Pi 4B</td>
      <td></td>
      <td></td>
      <td>OK</td>
      <td></td>
    </tr>
    <tr>
      <td>Pi 3B+</td>
      <td></td>
      <td></td>
      <td>OK</td>
      <td>OK</td>
    </tr>
    <tr>
      <td>Pi Zero W</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>

## Common issues and troubleshooting

### 1 APT cannot find the package

Check that the repo is configured

~~~bash
cat /etc/apt/sources.list.d/barhelper-kiosk.list
~~~

Update package lists and verify that apt can see the package

~~~bash
sudo apt update
apt-cache policy barhelper-kiosk
~~~

If `apt-cache policy` does not show your repo, it is usually one of these
1 The repo URL is wrong
2 GitHub Pages is not enabled for the repo
3 `Packages` or `Packages.gz` is missing in the repo

### 2 Install fails due to dependencies

Try this

~~~bash
sudo apt update
sudo apt -f install
sudo apt install -y barhelper-kiosk
~~~

### 3 Kiosk does not start after reboot

It depends on how the kiosk is started on your system. These are the most common checks.

Check if a systemd service exists

~~~bash
systemctl list-unit-files | grep -i barhelper || true
systemctl status barhelper-kiosk --no-pager || true
~~~

View logs if you have a service

~~~bash
journalctl -u barhelper-kiosk -b --no-pager || true
~~~

If you do not have a systemd service, the kiosk is often started via autostart. In that case it can help to check which desktop session is active.

~~~bash
echo $XDG_SESSION_TYPE
~~~

### 4 Black screen or only wallpaper

Check if a display manager and graphical environment are running

~~~bash
systemctl status display-manager --no-pager || true
ps aux | egrep 'wayfire|labwc|openbox|chromium|weston' | grep -v egrep || true
~~~

### 5 Network does not work

Check your IP and DNS

~~~bash
ip a
ping -c 1 1.1.1.1
ping -c 1 google.com
~~~

### 6 Get version and package info

~~~bash
dpkg -s barhelper-kiosk | sed -n '1,60p'
dpkg -L barhelper-kiosk | head
~~~

## Repo

https://github.com/BrewByte-NO/barhelper-kiosk-repo
