# *** Настройка Raspberry PI OS после установки ***
:ballot_box_with_check: Установка необходимых пакетов    
```yaml
sudo apt-get install -y jq wget curl udisks2 apparmor-utils libglib2.0-bin network-manager dbus systemd-journal-remote systemd-resolved
```
:ballot_box_with_check: Запуск Network Manager    
```yaml
systemctl start NetworkManager
```
```yaml
systemctl enable NetworkManager
```
:ballot_box_with_check: Прописываем DNS    
```yaml
sudo nano /etc/systemd/resolved.conf
```

`DNS`
```yaml
DNS=10.47.100.1 77.88.8.8
FallbackDNS=77.88.8.1#common.dot.dns.yandex.net 8.8.8.8#dns.google
```
`и`
```yaml
DNSSEC=yes
DNSOverTLS=yes
```

`Ctrl X` - для выхода    
`Y` для сохранения

:ballot_box_with_check: Перезапуск DNS    
```yaml
sudo systemctl restart systemd-resolved
```

:ballot_box_with_check: TorrServer (https://github.com/YouROK/TorrServer)
```yaml
curl -s https://raw.githubusercontent.com/YouROK/TorrServer/master/installTorrServerLinux.sh | sudo bash
```
:ballot_box_with_check: Разгоняем RaspberryPI 4B (редактируем файл)
```yaml
sudo nano /boot/firmware/config.txt
```
:ballot_box_with_check: находим строку #arm_freq=800 и вместо неё вставляем
```yaml
#Enable to overclock the arm. 700 MHz is the default.
arm_freq=2147
gpu_freq=750
over_voltage=6
#force_turbo=1
```
