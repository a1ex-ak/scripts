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
----------------------------------
:ballot_box_with_check: Установка архиваторов
```yaml
sudo apt-get install 7-zip lzip bzip3 -y
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
