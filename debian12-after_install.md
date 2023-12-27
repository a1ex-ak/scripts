# *** Настройка DEBIAN 12 после установки ***

----------------------------------------------------------------------
:ballot_box_with_check: Редактирование источников обновления
```yaml
sudo nano /etc/apt/sources.list
```
:ballot_box_with_check: дописываем в конце каждой строчки
```yaml
contrib non-free
```
---------------------------
:ballot_box_with_check: Удаление лишних программ
```yaml
sudo apt-get remove --purge gnome-mahjongg gnome-tetravex gnome-klotski gnome-nibbles four-in-a-row quadrapassel gnome-chess gnome-robots gnome-sudoku iagno lightsoff tali gnome-mines swell-foop five-or-more aisleriot gnome-taquin gnome-2048 hitori hoichess gnome-sound-recorder gnome-maps gnome-contacts gnome-music gnome-weather libreoffice-common totem-common evolution-common liblibreofficekitgtk libreoffice-style-colibre libreofficekit-data liblibreoffice-java shotwell-common rhythmbox-data librhythmbox-core10 transmission-common gnome-documents firefox-esr zutty
```
-------------------------------
:ballot_box_with_check: Обновление системы
```yaml
sudo apt update && sudo apt upgrade -y
```
или этой командой
```yaml
sudo apt update && sudo apt full-upgrade -y
```

-------------------------------
:ballot_box_with_check: Установка поддержки Flatpak
```yaml
sudo apt install flatpak
```
```yaml
sudo apt install gnome-software-plugin-flatpak
```
```yaml
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

-------------------------------
:ballot_box_with_check: Установка Extension Manager
```yaml
flatpak install flathub com.mattjakeman.ExtensionManager
```

-------------------------------
:ballot_box_with_check: Установка Flatseal
```yaml
flatpak install flathub com.github.tchx84.Flatseal
```

-------------------------------
:ballot_box_with_check: Перезапуск системы
```yaml
sudo reboot
```
:ballot_box_with_check: Удаление зависимостей
```yaml
sudo apt autoremove
```

----------------------------------
:ballot_box_with_check: Установка шрифта Noto-emoji
```yaml
sudo apt install fonts-noto-color-emoji -y
```

----------------------------------
:ballot_box_with_check: Установка кодеков
```yaml
sudo apt install ffmpeg libavcodec-extra gstreamer1.0-libav gstreamer1.0-plugins-ugly gstreamer1.0-vaapi -y
```

---------------------------------
:ballot_box_with_check: Установка архиваторов
```yaml
sudo apt install p7zip-rar rar unrar unace arj cabextract -y
```

----------------------------------
:ballot_box_with_check: Установка брандмауэр (firewall) UFW
```yaml
sudo apt install apt-transport-https lsb-release ca-certificates curl dirmngr gnupg -y
```

```yaml
sudo apt install ufw
```

```yaml
sudo systemctl enable ufw --now
```

--> Применяем политики по умолчанию
```yaml
sudo ufw default deny incoming
```
```yaml
sudo ufw default allow outgoing
```
 
--> Открываем RDP
```yaml
sudo ufw allow 3389/tcp
```
  
--> Включаем 
```yaml
sudo ufw enable
```
  
--> Проверяем
```yaml
sudo ufw status verbose
```

--> Просмотр профилей приложений UFW
```yaml
sudo ufw app list
```
  
----------------------------------
:ballot_box_with_check: Установка ZSH Neofetch Git
```yaml
sudo apt install git neofetch zsh
```
--> Клонируем .zplug
```yaml
git clone https://github.com/zplug/zplug ~/.zplug
```
--> Запускаем 
```yaml
zsh
```
--> ZSH по умолчанию
```yaml
grep tecmint /etc/passwd
```
```yaml
chsh -s $(which zsh)
```
```yaml
grep tecmint /etc/passwd
```
--> Перезапускаем
```yaml
sudo reboot
```

----------------------------------
:ballot_box_with_check: Archer T3U Plus (WiFi)
```yaml
https://github.com/cilynx/rtl88x2bu
```

----------------------------------
:ballot_box_with_check: установка драйверов NVidia
```yaml
#sudo apt install linux-headers-amd64
```
```yaml
sudo apt install linux-headers-$(uname -r)
```
```yaml
sudo apt update
```
```yaml
sudo apt install nvidia-driver firmware-misc-nonfree
```
--> nvidia x32
```yaml
sudo dpkg --add-architecture i386 && sudo apt update
```
```yaml
sudo apt install nvidia-driver-libs:i386
```

--------------------------------------------
:ballot_box_with_check: Debian 12/Sid включение Wayland с Nvidia
--------------------------------------------
```yaml
sudo sudo gnome-text-editor /lib/udev/rules.d/61-gdm.rules
```

----------------------------------------
	--> Нужно закомментировать данные строки:
 
	# Check if suspend/resume services necessary for working wayland support is ava>
	TEST{0711}!="/usr/bin/nvidia-sleep.sh", GOTO="gdm_disable_wayland"
	TEST{0711}!="/usr/lib/systemd/system-sleep/nvidia", GOTO="gdm_disable_wayland"
	IMPORT{program}="/bin/sh -c \"sed -e 's/: /=/g' -e 's/\([^[:upper:]]\)\([[:upp>
	ENV{NVIDIA_PRESERVE_VIDEO_MEMORY_ALLOCATIONS}!="1", GOTO="gdm_disable_wayland"
	IMPORT{program}="/bin/sh -c 'echo NVIDIA_HIBERNATE=`systemctl is-enabled nvidi>
	ENV{NVIDIA_HIBERNATE}!="enabled", GOTO="gdm_disable_wayland"
	IMPORT{program}="/bin/sh -c 'echo NVIDIA_RESUME=`systemctl is-enabled nvidia-r>
	ENV{NVIDIA_RESUME}!="enabled", GOTO="gdm_disable_wayland"
	IMPORT{program}="/bin/sh -c 'echo NVIDIA_SUSPEND=`systemctl is-enabled nvidia->
	ENV{NVIDIA_SUSPEND}!="enabled", GOTO="gdm_disable_wayland"
	LABEL="gdm_nvidia_end"

	--> а так же эти:
 
	# If this is a hybrid graphics laptop with vendor nvidia driver, disable wayland
	LABEL="gdm_hybrid_nvidia_laptop_check"
	TEST!="/run/udev/gdm-machine-is-laptop", GOTO="gdm_hybrid_nvidia_laptop_check_>
	TEST!="/run/udev/gdm-machine-has-hybrid-graphics", GOTO="gdm_hybrid_nvidia_lap>
	TEST!="/run/udev/gdm-machine-has-vendor-nvidia-driver", GOTO="gdm_hybrid_nvidi>
	GOTO="gdm_disable_wayland"
 	LABEL="gdm_hybrid_nvidia_laptop_check_end"

	--> Обновляем GRUB
	sudo sudo gnome-text-editor /etc/default/grub

	--> добавить в файл
	GRUB_CMDLINE_LINUX="nvidia-drm.modeset=1"

	--> Затем выполнить
	sudo update-grub

	-->В файле .profile в домашней директории нужно добавить:
	if [ "$XDG_SESSION_TYPE" = "wayland" ]; then
	    export MOZ_ENABLE_WAYLAND=1
	fi

	--> Чтобы работало аппаратное видеоускорение в Firefox в Wayland нужно установить пакет nvidia-vaapi-driver:
	sudo apt install nvidia-vaapi-driver

  	-->> 
 	sudo apt install intel-media-va-driver

	-----------------------------------
	--> Затем в Firefox --> about:config - переключить параметр в true
	media.ffmpeg.vaapi.enabled

-----------------------------------
# GS Connect (KDE Connect)
	--> GS Connect
	https://github.com/GSConnect/gnome-shell-extension-gsconnect/wiki
 
	--> Настраиваем Firewall:
 	sudo ufw allow 1714:1764/tcp
  	sudo ufw allow 1714:1764/udp

	--> OpenSSL:
 	sudo apt install openssl

   	--> Устанавливаем KDE Connect
    	https://f-droid.org/packages/org.kde.kdeconnect_tp/

----------------------------------
# Настройка сетевых папок - SAMBA
	sudo apt install samba

	https://dzen.ru/a/YwiCaenDbjRkMQuW
----------------------------------
# Чтобы Linux не засыпал

	sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
	sudo systemctl unmask sleep.target suspend.target hibernate.target hybrid-sleep.target
----------------------------------
# TorrServer

	curl -s https://raw.githubusercontent.com/YouROK/TorrServer/master/installTorrServerLinux.sh | sudo bash

----------------------------------
# Проверка батареи ноутбука в Linux
	upower -i `upower -e | grep 'BAT'`

