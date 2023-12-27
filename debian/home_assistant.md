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
