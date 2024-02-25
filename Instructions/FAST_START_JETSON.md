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
mkdir -p ~/.config/autostart
cp /usr/share/applications/vino-server.desktop ~/.config/autostart

# Configure the VNC server
gsettings set org.gnome.Vino prompt-enabled false
gsettings set org.gnome.Vino require-encryption false

# Set a password to access the VNC server
# Replace thepassword with your desired password
gsettings set org.gnome.Vino authentication-methods "['vnc']"
gsettings set org.gnome.Vino vnc-password $(echo -n 'thepassword'|base64)

# Reboot the system so that the settings take effect
sudo reboot
```
6. Установить псевдо-дисплей: 
```bash
sudo apt-get install xserver-xorg-video-dummy
```
7. Настроить псевдо-дисплей:
```bash
Section "Device"
    Identifier  "Configured Video Device"
    Driver      "dummy"
    # Default is 4MiB, this sets it to 16MiB
    VideoRam    16384
EndSection

Section "Monitor"
    Identifier  "Configured Monitor"
    HorizSync 31.5-48.5
    VertRefresh 50-70
EndSection

Section "Screen"
    Identifier  "Default Screen"
    Monitor     "Configured Monitor"
    Device      "Configured Video Device"
    DefaultDepth 24
    SubSection "Display"
    	Depth 24
    	Modes "1920x1080"
    EndSubSection
EndSection
```