:ballot_box_with_check: Настройка -> Общий доступ к файлам
```shell
sudo apt-get install gnome-user-share
```
:ballot_box_with_check: Настройка -> Общий доступ к медиа
```shell
sudo apt-get install rygel 
```
:ballot_box_with_check: Настройка -> Удаленный рабочий стол
```shell
sudo apt-get install gnome-remote-desktop
```
----------------------------------
:ballot_box_with_check: Установка брандмауэр (firewall) UFW
```yaml
sudo apt install apt-https lsb-release ca-certificates curl dirmngr gnupg -y
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
----------------------------------------------
:ballot_box_with_check: Установка ZeroTier-One
```shell
sudo apt-get install zerotier-one
```
через epm
```shell
epm -i zerotier-one
```

:ballot_box_with_check: Запуск zerotier-one
```shell
sudo serv zerotier-one on
```

:ballot_box_with_check: Установка AnyDesk
```shell
sudo apt-get install libgtkglext
```
```shell
sudo epm play anydesk
```
```shell
sudo serv anydesk on
```
