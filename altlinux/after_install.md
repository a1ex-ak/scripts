
:ballot_box_with_check: Настройка -> Общий доступ к файлам
```yaml
sudo apt-get install gnome-user-share
```
:ballot_box_with_check: Настройка -> Общий доступ к медиа
```yaml
sudo apt-get install rygel 
```
:ballot_box_with_check: Настройка -> Удаленный рабочий стол
```yaml
sudo apt-get install gnome-remote-desktop
```
:ballot_box_with_check: Расширенная настройка сети
```yaml
sudo apt-get install NetworkManager-applet-gtk
```
----------------------------------
:ballot_box_with_check: Установка шрифтов
```yaml
sudo apt-get install fonts-ttf-ms fonts-cascadia-code fonts-ttf-google-crosextra-carlito fonts-ttf-google-noto-emoji fonts-ttf-google-noto-emoji-color
```
----------------------------------
:ballot_box_with_check: Установка архиваторов
```yaml
sudo apt-get install 7-zip lzip bzip3 -y
```
----------------------------------
:ballot_box_with_check: Установка шрифтов Microsoft ClearType (Calibri, Cambria, Candara, Consolas, Constantia, Corbel)
```yaml
wget -q -O - https://gist.githubusercontent.com/Blastoise/72e10b8af5ca359772ee64b6dba33c91/raw/2d7ab3caa27faa61beca9fbf7d3aca6ce9a25916/clearType.sh | bash
```
:ballot_box_with_check: Установка шрифтов Tahoma и  Segoe-UI
```yaml
wget -q -O - https://gist.githubusercontent.com/Blastoise/b74e06f739610c4a867cf94b27637a56/raw/96926e732a38d3da860624114990121d71c08ea1/tahoma.sh | bash
```
```yaml
wget -q -O - https://gist.githubusercontent.com/Blastoise/64ba4acc55047a53b680c1b3072dd985/raw/6bdf69384da4783cc6dafcb51d281cb3ddcb7ca0/segoeUI.sh | bash
```
----------------------------------
:ballot_box_with_check: Зависимости Nextcloud
```yaml
sudo apt-get install nautilus-python rpm-macros-qt5-webengine rpm-build-python3 rpm-build-gir rpm-build-kf5 doxygen extra-cmake-modules graphviz kf5-kio-devel libqtkeychain-qt5-devel libsqlite3-devel libssl-devel python3-dev qt5-tools-devel qt5-webkit-devel zlib-devel libgio-devel glib2-devel qt5-svg-devel kf5-kwindowsystem-devel qt5-quickcontrols2-devel qt5-websockets-devel kf5-karchive-devel rpm-build-python3
```

----------------------------------
:ballot_box_with_check: Установка брандмауэр (firewall) UFW
```yaml
sudo apt-get install apt-https lsb-release ca-certificates curl dirmngr gnupg python3-module-setuptools python3-module-systemd -y
```

```yaml
sudo apt-get install ufw
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
----------------------------------------------
:ballot_box_with_check: Установка WAF
```yaml
sudo apt-get install modsecurity
```
----------------------------------------------
:ballot_box_with_check: Установка ZeroTier-One
```yaml
sudo apt-get install zerotier-one
```
через epm
```yaml
epm -i zerotier-one
```

:ballot_box_with_check: Запуск сервиса ZeroTier-One
```yaml
sudo serv zerotier-one on
```

:ballot_box_with_check: Установка AnyDesk
```yaml
sudo apt-get install libgtkglext
```
```yaml
sudo epm play anydesk
```
```yaml
sudo serv anydesk on
```
