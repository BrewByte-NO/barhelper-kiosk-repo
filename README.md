# Barhelper Kiosk

Barhelper Kiosk gjør en Raspberry Pi om til en dedikert kiosk som starter Barhelper automatisk i fullskjerm.

## Installer Raspberry Pi OS

Den enkleste og offisielle måten er å bruke Raspberry Pi Imager.

Raspberry Pi Imager  
https://www.raspberrypi.com/software/

Offisiell dokumentasjon, Getting started  
https://www.raspberrypi.com/documentation/computers/getting-started.html

Kort versjon
1. Installer Raspberry Pi Imager på PC eller Mac
2. Velg Raspberry Pi modell, Raspberry Pi OS og lagringskort
3. Skriv image til SD kort eller SSD
4. Boot Pi og fullfør første oppsett

Tips  
På Pi 3+ anbefales Raspberry Pi OS 64 bit.
På Pi 4 og Pi 5 anbefales Raspberry Pi OS 64 bit.

## Installer Barhelper Kiosk

Dette er laget for copy paste i terminal.

### Steg 1 Oppdater systemet

Kjør dette først

~~~bash
sudo apt update
sudo apt full-upgrade -y
sudo reboot
~~~

### Steg 2 Alternativ A, enklest, uten signering

Når Pi har startet på nytt, kjør dette

~~~bash
echo "deb [trusted=yes] https://brewbyte-no.github.io/barhelper-kiosk-repo/ ./" | sudo tee /etc/apt/sources.list.d/barhelper-kiosk.list
sudo apt update
sudo apt install -y barhelper-kiosk
sudo reboot
~~~

## Oppdatering

For å oppdatere kjør følgende kode

~~~bash
sudo apt update
sudo apt upgrade
~~~

## Avinstallering

Om du ønsker å avinstallere

~~~bash
sudo apt remove barhelper-kiosk
sudo apt autoremove
~~~

## Testmatrise

Tabellen under er ment å oppdateres etter hvert som du tester flere modeller og OS varianter.

<table>
  <thead>
    <tr>
      <th>Raspberry Pi modell</th>
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
      <td></td>
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





## Vanlige problemer og feilsøking

### 1 APT finner ikke pakken

Sjekk at repoet ligger inne

~~~bash
cat /etc/apt/sources.list.d/barhelper-kiosk.list
~~~

Oppdater pakkelister og se om apt kjenner igjen pakken

~~~bash
sudo apt update
apt-cache policy barhelper-kiosk
~~~

Hvis `apt-cache policy` ikke viser repoet ditt, er det nesten alltid en av disse
1 Repo URL er feil
2 GitHub Pages er ikke aktivert på repoet
3 `Packages` eller `Packages.gz` mangler i repoet

### 2 Install feiler på dependencies

Prøv dette

~~~bash
sudo apt update
sudo apt -f install
sudo apt install -y barhelper-kiosk
~~~

### 3 Kiosk starter ikke etter reboot

Det kommer an på hvordan kiosk startes hos deg. Her er de vanligste sjekkene.

Sjekk om det finnes en systemd tjeneste

~~~bash
systemctl list-unit-files | grep -i barhelper || true
systemctl status barhelper-kiosk --no-pager || true
~~~

Se logger hvis du har en tjeneste

~~~bash
journalctl -u barhelper-kiosk -b --no-pager || true
~~~

Hvis du ikke har en systemd tjeneste, er kiosk ofte startet via autostart. Da er det lurt å sjekke om riktig desktop session er aktiv.

~~~bash
echo $XDG_SESSION_TYPE
~~~

### 4 Skjerm er svart eller bare wallpaper

Sjekk om display manager og grafisk miljø kjører

~~~bash
systemctl status display-manager --no-pager || true
ps aux | egrep 'wayfire|labwc|openbox|chromium|weston' | grep -v egrep || true
~~~

### 5 Nettverk virker ikke

Sjekk IP og at du har DNS

~~~bash
ip a
ping -c 1 1.1.1.1
ping -c 1 google.com
~~~

### 6 Hente versjon og info om den installerte pakken

~~~bash
dpkg -s barhelper-kiosk | sed -n '1,60p'
dpkg -L barhelper-kiosk | head
~~~

## Repo

https://github.com/BrewByte-NO/barhelper-kiosk-repo
