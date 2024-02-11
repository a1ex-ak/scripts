### [Raspberry PI 4B - Установка Debian 12 Bookworm, и Supervised Home Assistant]

:ballot_box_with_check: Переход в режим root    
```yaml
sudo -s
```
:ballot_box_with_check: Обновление списка пакетов и пакетов    
```yaml
apt update
```
```yaml
apt full-upgrade -y
```
:ballot_box_with_check: Обновление прошивки - только при необходимости! (новый одноплатник или не обновляли не менее двух лет)    
```yaml
rpi-update
```
:ballot_box_with_check: Установка обновленных пакетов, в случае отображения диалоговых окон - нажимаем q    
```yaml
apt upgrade --without-new-pkgs -y
```
:ballot_box_with_check: Установка необходимых пакетов    
```yaml
apt-get install -y jq wget curl udisks2 apparmor-utils libglib2.0-bin network-manager dbus systemd-journal-remote systemd-resolved
```
:ballot_box_with_check: Запуск Network Manager    
```yaml
systemctl start NetworkManager
 
systemctl enable NetworkManager
```
   
:ballot_box_with_check: Приложение для настройки - 
```yaml
sudo raspi-config
```
`5 Localisation Options / I1 Change Locale - ищем и выбираем пробелом ru_UA.UTF-8 UTF-8`
`5 Localisation Options / I2 Change Timezone - выбираем часовой пояс`

:ballot_box_with_check: Дополнительные настройки для устранения ошибок в НА    
```yaml
nano /boot/cmdline.txt
```
В конец первой строки файла вставляем -->
```yaml
systemd.unified_cgroup_hierarchy=false lsm=apparmor
```
`Ctrl X` - для выхода    
`Y` для сохранения    

:ballot_box_with_check: Скрипт управления вентилятором для корпусов Argon M2    
Установка скрипта - `curl https://download.argon40.com/argon1.sh | bash`    
Настройка включения - `argonone-config`      

:ballot_box_with_check: Перезагрузка - `sudo reboot` 


:ballot_box_with_check: Установка Docker - 
:ballot_box_with_check: docker 24.0.7 (RaspberryOS & Debian) Home Assistent
```yaml
sudo apt install \
docker-compose-plugin=2.21.0-1~debian.12~bookworm \
docker-ce-cli=5:24.0.7-1~debian.12~bookworm \
docker-buildx-plugin=0.11.2-1~debian.12~bookworm \
docker-ce=5:24.0.7-1~debian.12~bookworm \
docker-ce-rootless-extras=5:24.0.7-1~debian.12~bookworm
```
или
```yaml
curl -fsSL https://get.docker.com -o install-docker.sh
sudo sh install-docker.sh --version 24.0.7-1
```
:ballot_box_with_check: docker 25.0.0 (есть проблемы с Home Assistent, пока не ставим)
[Информация на GitHub](https://github.com/home-assistant/supervisor/issues/4827)

Запретить обновление одних и тех же пакетов в краткосрочной перспективе.:
```yaml
sudo apt-mark hold docker-compose-plugin docker-ce-cli docker-buildx-plugin docker-ce docker-ce-rootless-extras
```
последний docker
```yaml
sudo curl -fsSL get.docker.com | sh
```

:ballot_box_with_check: Добавляем в группу docker пользователя
```yaml
sudo gpasswd -a $USER docker
newgrp docker
```

:ballot_box_with_check: Установка OS-Agent    
:white_check_mark: [Последний релиз](https://github.com/home-assistant/os-agent/releases/latest)    
Загружаем - `wget https://github.com/home-assistant/os-agent/releases/download/1.6.0/os-agent_1.6.0_linux_aarch64.deb` (номер меняем на актуальный)    
Установка - `sudo dpkg -i os-agent_1.6.0_linux_aarch64.deb`    

:ballot_box_with_check: Установка Home Assisistant Supervised    
:white_check_mark: [Последний релиз](https://github.com/home-assistant/supervised-installer/releases)    
Загружаем - `wget https://github.com/home-assistant/supervised-installer/releases/download/1.5.0/homeassistant-supervised.deb`    
Установка - `sudo dpkg -i homeassistant-supervised.deb`    

:arrow_right: Веб интерфейс Home Assistant - `http://IP adress:8123`    

:arrow_right: Информация о системе - `http://IP adress:8123/hassio/system`    

:ballot_box_with_check: Кнопка вызова настроек с панели, в `configuration.yaml`     
```yaml
homeassistant:
  packages: !include_dir_merge_named includes
```
```yaml
panel_custom:
  - name: server_state
    sidebar_title: 'Система'
    sidebar_icon: mdi:server
    js_url: /api/hassio/app/entrypoint.js
    url_path: 'hassio/system'
    embed_iframe: true
    require_admin: true
    config:
      ingress: core_configurator 
```

:ballot_box_with_check: Конфигурация Maria DB    
Logins:    
```yaml
- username: hass
  password: hass
```
Rights:    
```yaml
- username: hass
  database: homeassistant
```
:ballot_box_with_check: Путь к базе в файле `secrets.yaml`    
```yaml
db_link: mysql://hass:hass@core-mariadb/homeassistant?charset=utf8
```

:ballot_box_with_check: Интеграция SQL    
Name:    
```yaml
MariaDB
```
Column:    
```yaml
value
```
Select Query:    
```yaml
SELECT table_schema "database", Round(Sum(data_length + index_length) / 1048576, 2) "value" FROM information_schema.tables WHERE table_schema="homeassistant" GROUP BY table_schema;
```
Unit of Measure:    
```yaml
MB
```

:ballot_box_with_check: Рекордер в `includes/system_sensors.yaml`     
```yaml
system_sensors:

    recorder:
      db_url: !secret db_link
      purge_keep_days: 30
      auto_purge: true
      commit_interval: 60
```

:ballot_box_with_check: HACS    
режим root:    
```yaml
sudo -s
```
:white_check_mark: [Команда с сайта HACS](https://hacs.xyz/docs/setup/download)    
Загрузка и установка HACS - `wget -O - https://get.hacs.xyz | bash -`    
Выход:    
```yaml
exit
```
