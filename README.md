# Barhelper Kiosk

Barhelper Kiosk gjør en Raspberry Pi om til en dedikert kiosk som starter Barhelper automatisk i fullskjerm.

## 1. Installer Raspberry Pi OS

Bruk Raspberry Pi Imager, det er den offisielle metoden.

Raspberry Pi software og Raspberry Pi Imager  
https://www.raspberrypi.com/software/

Offisiell dokumentasjon, Getting started  
https://www.raspberrypi.com/documentation/computers/getting-started.html

Kort versjon
1. Installer Raspberry Pi Imager på PC
2. Velg Device, OS og lagringskort
3. Skriv image til SD kort eller SSD
4. Boot Pi og gjør første oppsett

Anbefaling
1. Bruk Raspberry Pi OS 64 bit på Pi 4 og Pi 5
2. Skru på SSH i Imager settings om du vil administrere over nett

## 2. Installer Barhelper Kiosk for en nybegynner

Dette er laget for copy paste i terminal.

Først, oppdater systemet
```bash
sudo apt update
sudo apt full-upgrade -y
sudo reboot
