:ballot_box_with_check: Установка необходимых пакетов
```yaml
sudo apt-get install -y jq wget curl udisks2 apparmor-utils libglib2.0-bin network-manager dbus systemd-journal-remote systemd-resolved bluez
```
:ballot_box_with_check: прописываем DNS
```yaml
sudo nano /etc/systemd/resolved.conf
```
`Ctrl-O` - для перезаписи    
`Y` - для сохранения    
`Ctrl-X` для выхода    

:ballot_box_with_check: Перезапуск DNS    
```yaml
sudo systemctl restart systemd-resolved
```
:ballot_box_with_check: Установка docker - 
```yaml
curl -fsSL get.docker.com | sh
```

:ballot_box_with_check: Установка OS-Agent    
:white_check_mark: [Последний релиз](https://github.com/home-assistant/os-agent/releases/latest)    
Загружаем - `wget https://github.com/home-assistant/os-agent/releases/download/1.6.0/os-agent_1.6.0_linux_x86_64.deb` (номер меняем на актуальный)    
Установка - `sudo dpkg -i os-agent_1.6.0_linux_x86_64.deb`    

:ballot_box_with_check: Установка Home Assisistant Supervised    
:white_check_mark: [Последний релиз](https://github.com/home-assistant/supervised-installer/releases)    
Загружаем - `wget https://github.com/home-assistant/supervised-installer/releases/download/1.5.0/homeassistant-supervised.deb`    
Установка - `sudo dpkg -i homeassistant-supervised.deb`    

:arrow_right: Веб интерфейс Home Assistant - `http://IP adress:8123`    

:arrow_right: Информация о системе - `http://IP adress:8123/hassio/system`  
