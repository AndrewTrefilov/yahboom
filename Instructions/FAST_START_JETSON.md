**Подготовка Jetson Orin NX**  

---
1. Установить [NVIDIA SDK Manager](https://developer.nvidia.com/sdk-manager).
2. Выбрать и установить [JetPack](https://developer.nvidia.com/embedded/jetpack).
3. Инструкцию по установке можно найти [тут](https://www.waveshare.com/wiki/JETSON-ORIN-NX-16G-DEV-KIT#JETSON-ORIN-IO-BASE_User_Guide).
4. После установки не забыть, выполнить следующие команды:
```bash
sudo apt update
sudo apt install nvidia-jetpack

sudo apt install python3-pip
sudo pip3 install jetson-stats
sudo reboot
```
5. Установить VNC Server:
```bash
sudo apt update
sudo apt install vino
# Enable the VNC server to start each time you log in
cd /usr/lib/systemd/user/graphical-session.target.wants
sudo ln -s ../vino-server.service ./.

# Configure the VNC server
gsettings set org.gnome.Vino prompt-enabled false
gsettings set org.gnome.Vino require-encryption false

# Set a password to access the VNC server
# Replace thepassword with your desired password
gsettings set org.gnome.Vino authentication-methods "['vnc']"
gsettings set org.gnome.Vino vnc-password $(echo -n 'thepassword'|base64)

# Reboot the system so that the settings take effect
sudo reboot

# Settings → Users → Automatic Login
```
6. Установить псевдо-дисплей: 

```bash
# Settings → Sharing → Screen Sharing → set ‘Active’
sudo apt install xserver-xorg-video-dummy
```
7. Создать конфиг
```bash
cd /etc/X11
sudo vim xorg.conf.dummy
```
8. Настроить псевдо-дисплей:
```bash
Section "Device"
    Identifier "DummyDevice"
    Driver "dummy"
    VideoRam 256000
EndSection
 
Section "Screen"
    Identifier "DummyScreen"
    Device "DummyDevice"
    Monitor "DummyMonitor"
    DefaultDepth 24
    SubSection "Display"
        Depth 24
        Modes "1920x1080_60.0"
    EndSubSection
EndSection
 
Section "Monitor"
    Identifier "DummyMonitor"
    HorizSync 30-70
    VertRefresh 50-75
    ModeLine "1920x1080" 148.50 1920 2448 2492 2640 1080 1084 1089 1125 +Hsync +Vsync
EndSection

```
9. Обновить */etc/X11/xorg.conf*
```bash
sudo cp xorg.conf xorg.conf.backup
sudo cp xorg.conf.dummy xorg.conf

sudo reboot
```
