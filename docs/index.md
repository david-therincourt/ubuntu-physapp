# Ubuntu 24.04 - Post installation


# Bases

## Gnome

```bash
$ sudo apt-get install gnome-tweaks gnome-shell-extensions
```

## Flatpak

```bash
$ sudo apt install flatpak
$ sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

## AppImage support

```bash
$ sudo apt install libfuse2t64
```

## Wine

```bash
$ sudo apt install wine64 wine32:i386 ttf-mscorefonts-installer
$ wine msiexec /i application.msi
```

## RamDisk

Pour limiter l'écriture de fichiers temporaires (Firefox, Compilation LaTex, ...) sur le disque SSD, il est préférable de monter le répertoire `/tmp` dans la RAM. Pour un montage automatique au démarrage, ajouter la ligne suivante à la fin du fichier `/etc/fstab` : 

```bash
tmpfs /tmp tmpfs defaults,size=2048M 0 0
```

## Console

Supprimer le chemin long dans le prompt à partir du  fichier `.bashrc`  : changer `w` par `W`  !

```bash
if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\W\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\W\$ '
fi
```






# Web

## Chrome

```bash
$ sudo wget -O- https://dl.google.com/linux/linux_signing_key.pub | gpg --dearmor | sudo tee /usr/share/keyrings/google-chrome.gpg
$ echo deb [arch=amd64 signed-by=/usr/share/keyrings/google-chrome.gpg] http://dl.google.com/linux/chrome/deb/ stable main | sudo tee /etc/apt/sources.list.d/google-chrome.list
$ sudo apt update
$ sudo apt install google-chrome-stable
```

## Chromium

```bash
$ sudo snap install chromium 
```





# Multimédia


## Codecs MP3, ...

```bash
$ sudo apt install ubuntu-restricted-extras
```

## Vidéo

```bash
$ sudo apt install vlc mplayer mplayer-gui
```







# Bureautique

## Utilitaires PDF

```bash
$ sudo apt-get install xournal xournalpp pdfarranger 
```


## Copie d'écran et annotation d'image

```bash
$ sudo apt install ksnip
```





# Markdown

## Pandoc

 Conversion entre formats de balisage.

```bash
$ sudo apt install pandoc context
```

## Editeurs supplémentaires

```bash
$ sudo apt install ghostwriter retext apostrophe
```

## Marktext

Installation manuel à partir DEB sur Github !

Eviter Flatpak !

```bash
$ sudo flatpak install flathub com.github.marktext.marktext
```



# Sphinx

```bash
$ sudo apt install python3-stemmer
$ pip install --break-system-packages sphinx sphinx-rtd-theme sphinx-copybutton sphinx-prompt esbonio
```



# LaTeX

## TeXlive

```bash
$ sudo apt install texlive texlive-lang-french texlive-latex-extra 
$ sudo apt install texlive-science texlive-fonts-extra texlive-publishers
```

## Vérification de syntaxe

```bash
$ sudo apt install chktex
```

## Compilation automatique

```bash
$ sudo apt install latexmk
```

## Editeurs

```bash
$ sudo apt install gummi qtikz
```




# VSCode

## Installation

```bash
$ sudo snap install code --classic
```

## Extension  French Language Pack

Automatique au démarrage.

## Extension Latex

Installer l'extension `latex-workshop`.

Dépendance :

```bash
$ sudo apt install latexmk
```

Paramètres pour preview :

```
The root file directory : changer %DIR% par /tmp/vscode-latex
```

Minted : ajouter l'option `--shell-escape` dans `latex-workshop.latex.tools` pour le compilateur  `latexmk` (fichier JSON)

```json
"name": "latexmk",
            "command": "latexmk",
            "args": [
                "--shell-escape",
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "-outdir=%OUTDIR%",
                "%DOC%"
            ],
```

## Extension Sphynx

Installer les extensions suivantes uniquement :

- reStructuredText Syntax highlighting

- Esbonio (server)

```bash
$ pip install esbonio
```

- Installe l'extension Python de Microsoft.






# Python

## Python is Python 3

```bash
$ sudo apt install python-is-python3 
```

## PIP

```bash
$ sudo apt install python3-pip
```

Pour forcer l'installation avec PIP, ajouter dans `/etc/environment` :

```bash
PIP_BREAK_SYSTEM_PACKAGES=1
```

## Librairies

```bash
$ sudo apt install python3-numpy python3-matplotlib python3-scipy 
#ou
$ pip install numpy matplolib scipy
```

## Editeurs

```bash
$ sudo apt-get install thonny  # BUG !!!
#ou
$ pip install thonny
```

Avec PIP, installer manuellement le raccourci.

## Spyder

```bash
$ pip install spyder
```

## Jupyter

```bash
$ pip install jupyter
$ sudo apt install texlive-xetex # Pour export PDF dans Jupyter Notebook
```





# Physique

## Latis

Fonctionne avec Wine.

```bash
$ msiexe /i Latis.msi
```

## Regressi

Fonctionner avec Wine.

```bash
$ msiexe /i regressi-mpeg-setup.msi
```

## LTSpice

```bash
$ msiexe /i LTSpice.msi
```

# Microcontrôleurs

## Udev rules

Règles pour l'accés aux périphériques via `udev` à mettre dans un nouveau fichier  `/etc/udev/rules.d/100-physapp.rules` avec le contenu : 

```bash
# Agilent / Keysight
SUBSYSTEMS=="usb", ATTRS{idVendor}=="0957", MODE="0666"
# Siglent
SUBSYSTEMS=="usb", ATTRS{idVendor}=="f4ec", MODE="0666"
# Arduino UNO R3
SUBSYSTEMS=="usb", ATTRS{idVendor}=="2a03", MODE="0666"
# Carte Arduino R4 minima
SUBSYSTEMS=="usb", ATTRS{idVendor}=="2341", MODE="0666"
# ESP32 - Silicon Labs CP210x UART Bridge
SUBSYSTEMS=="usb", ATTRS{idVendor}=="10c4", MODE="0666"
# STMicroelectronic
SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", MODE="0666"
SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", MODE="374b"
```

## Arduino IDE 1.8.x

```bash
$ sudo apt install arduino
```

## Arduino IDE 2

### Installation

A partir du fichier ZIP télécharger sur le site d'Arduino.

### Problème d'exécution

Arduino IDE 2 est développé avec Electron (Chromium et Node Js) et son code s’exécute dans un environnement limité (bas à sable). Ce qui empèche son lancement dans Gnome !

L'application `apparmor` permet de contourner cette restriction. Ajouter le fichier de configuration `/etc/apparmor.d/arduinoide` avec le contenu :

```bash
abi <abi/4.0>,
include <tunables/global>
profile arduino /opt/arduino-ide_2.3.3_Linux_64bit/arduino-ide flags=(unconfined) {
  userns,
  include if exists <local/arduino>}
```

Ne pas oublier de modifier le chemin de l'application Arduino IDE !

## ESP32

Application ESPTool pour la programmation en ligne de commande.

```bash
$ pip3 install --break-system-packages esptool
```



# Interfaces USB

## Digilent Analog Discovery 3 

Pilotes `adept2.runtime` , `adept2.utililities` et `WaveForms` sur le site Siglent.

```bash
$ sudo dpkg -i digilent.adept.runtime_2.27.9-amd64.deb digilent.adept.utilities_2.7.1-amd64.deb
$ sudo dpkg -i digilent.waveforms_3.22.2_amd64.deb
$ sudo apt install libqt5multimedia5 # Erreur
$ sudo apt --fix-broken install
```


## Analog Device ADALM 2000

- Pilote `libio` :

```bash
$ sudo apt install libiio-utils
$ iio_info -u ip:192.168.2.1
```

- Logiciel Scopy à télécharger sur le site d'Analog Device au format `flapak` :

```bash
$ sudo flatpak install Scopy-v1.4.1-Linux-x86-64.flatpak
$ flatpak run org.adi.Scopy
```
Lourd à installer !

- Libairie `libm2k`  (problème avec 24.04)

- Python : pas de librairie !






# Radio Logiciel (SDR)

## Logiciel GNU Radio

```bash
 sudo apt install gnuradio
```

## Logiciel Gqrx SDR

```bash
$ sudo apt install gqrx-sdr
```

## Carte USRP B200

- Pilote :

```bash
$ sudo apt install uhd-host
```

- Téléchargement des images :

```bash
$ sudo uhd_images_downloader
```

- Droits d'acces aux cartes :

```bash
$ sudo cp /usr/libexec/uhd/utils/uhd-usrp.rules /etc/udev/rules.d/
$ sudo udevadm control --reload-rules
$ sudo udevadm trigger
```

- Recherche :

```bash
$ uhd_find_devices
```

- GNURadio : USRP Source.

- GQRX : support natif.

## Carte ADALM Pluto

- Pilote :

```bash
$ sudo apt install libiio-utils
```

- Information sur la carte :

```bash
$ iio_info -u ip:192.168.2.1
```

- Session SSH (mot de passe = analog) :

```bash
$ ssh root@192.168.2.1
```

- GnuRadio : PlutoSDR Source

- GQRX : ne fonctionne pas !

- Python :

```bash
$ pip install pyadi-iio
```
