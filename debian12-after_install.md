//////////////////////////////////////////////
// 001. НАСТРОЙКА DEBIAN 12 ПОСЛЕ УСТАНОВКИ //
/////////////////////////////////////////////

---------------------------
  sudo nano /etc/apt/sources.list

дописываем - contrib non-free

---------------------------
# Удаление лишних программ
sudo apt-get remove --purge gnome-mahjongg gnome-tetravex gnome-klotski gnome-nibbles four-in-a-row quadrapassel gnome-chess gnome-robots gnome-sudoku iagno lightsoff tali gnome-mines swell-foop five-or-more aisleriot gnome-taquin gnome-2048 hitori hoichess gnome-sound-recorder gnome-maps gnome-music libreoffice-common totem-common evolution-common liblibreofficekitgtk libreoffice-style-colibre libreofficekit-data liblibreoffice-java shotwell-common rhythmbox-data librhythmbox-core10 gnome-documents zutty

-------------------------------
# обновление системы
sudo apr update && sudo apr upgrade -y
sudo apt full-upgrade

-------------------------------
# Установка поддержки Flatpak
sudo apt install flatpak
sudo apt install gnome-software-plugin-flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

----------------------------------
# настройка swap
echo -e "vm.dirty_background_ratio = 50" | sudo tee -a /etc/sysctl.conf
echo -e "vm.dirty_ratio = 80" | sudo tee -a /etc/sysctl.conf
echo -e "vm.vfs_cache_pressure=50" | sudo tee -a /etc/sysctl.conf - устанвливаем для ssd

echo -e "vm.vfs_cache_pressure=1000" | sudo tee -a /etc/sysctl.conf - устанваливаем для hdd
  
----------------------------------
# установка tlp
sudo apt install tlp tlp-rdw
sudo systemctl enable tlp

sudo systemctl start tlp
sudo systemctl status tlp

----------------------------------
# установка кодеков
sudo apt install ffmpeg libavcodec-extra gstreamer1.0-libav gstreamer1.0-plugins-ugly gstreamer1.0-vaapi

----------------------------------
# установка ZSH
sudo apt install git neofetch zsh

git clone https://github.com/zplug/zplug ~/.zplug

grep tecmint /etc/passwd
chsh -s $(which zsh)
grep tecmint /etc/passwd

----------------------------------
# Archer T3U Plus (WiFi)
https://github.com/cilynx/rtl88x2bu

----------------------------------
# установка драйверов NVidia
#sudo apt install linux-headers-amd64
sudo apt install linux-headers-$(uname -r)

sudo apt update
sudo apt install nvidia-driver firmware-misc-nonfree

# nvidia x32
sudo dpkg --add-architecture i386 && sudo apt update
sudo apt install nvidia-driver-libs:i386

--------------------------------------------
# Debian 12/Sid включение Wayland с Nvidia
--------------------------------------------
sudo sudo gnome-text-editor /lib/udev/rules.d/61-gdm.rules

----------------------------------------
# Нужно закомментировать данные строки:
# Check if suspend/resume services necessary for working wayland support is ava>
#TEST{0711}!="/usr/bin/nvidia-sleep.sh", GOTO="gdm_disable_wayland"
#TEST{0711}!="/usr/lib/systemd/system-sleep/nvidia", GOTO="gdm_disable_wayland"
#IMPORT{program}="/bin/sh -c \"sed -e 's/: /=/g' -e 's/\([^[:upper:]]\)\([[:upp>
#ENV{NVIDIA_PRESERVE_VIDEO_MEMORY_ALLOCATIONS}!="1", GOTO="gdm_disable_wayland"
#IMPORT{program}="/bin/sh -c 'echo NVIDIA_HIBERNATE=`systemctl is-enabled nvidi>
#ENV{NVIDIA_HIBERNATE}!="enabled", GOTO="gdm_disable_wayland"
#IMPORT{program}="/bin/sh -c 'echo NVIDIA_RESUME=`systemctl is-enabled nvidia-r>
#ENV{NVIDIA_RESUME}!="enabled", GOTO="gdm_disable_wayland"
#IMPORT{program}="/bin/sh -c 'echo NVIDIA_SUSPEND=`systemctl is-enabled nvidia->
#ENV{NVIDIA_SUSPEND}!="enabled", GOTO="gdm_disable_wayland"
#LABEL="gdm_nvidia_end"

# If this is a hybrid graphics laptop with vendor nvidia driver, disable wayland
#LABEL="gdm_hybrid_nvidia_laptop_check"
#TEST!="/run/udev/gdm-machine-is-laptop", GOTO="gdm_hybrid_nvidia_laptop_check_>
#TEST!="/run/udev/gdm-machine-has-hybrid-graphics", GOTO="gdm_hybrid_nvidia_lap>
#TEST!="/run/udev/gdm-machine-has-vendor-nvidia-driver", GOTO="gdm_hybrid_nvidi>
#GOTO="gdm_disable_wayland"

sudo sudo gnome-text-editor /etc/default/grub
# Добавить
GRUB_CMDLINE_LINUX="nvidia-drm.modeset=1"

# Затем выполнить
sudo update-grub

# В файле .profile в домашней директории нужно добавить:
if [ "$XDG_SESSION_TYPE" = "wayland" ]; then
    export MOZ_ENABLE_WAYLAND=1
fi

# Чтобы работало аппаратное видеоускорение в Firefox в Wayland нужно установить пакет nvidia-vaapi-driver:
sudo apt install nvidia-vaapi-driver

-----------------------------------
# Затем  в about:config переключить параметр media.ffmpeg.vaapi.enabled в true.

----------------------------------
# Настройка сетевых папок - SAMBA
sudo apt install samba

https://dzen.ru/a/YwiCaenDbjRkMQuW

sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
sudo systemctl unmask sleep.target suspend.target hibernate.target hybrid-sleep.target

curl -s https://raw.githubusercontent.com/YouROK/TorrServer/master/installTorrServerLinux.sh | sudo bash