```yaml
wget https://github.com/home-assistant/operating-system/releases/download/18.2/haos_ova-18.2.qcow2.xz
```
```yaml
unxz haos_ova-18.2.qcow2.xz
```
----

Create the VM

General:
- Select your VM name and ID
- Select 'start at boot'

OS:
- Select 'Do not use any media'

System:
- Change 'machine' to 'q35'
- Change BIOS to OVMF (UEFI)
- Select the EFI storage (typically local-lvm)
- Uncheck 'Pre-Enroll keys'

Disks:
- Delete the SCSI drive and any other disks

CPU:
- Set minimum 2 cores

Memory:
- Set minimum 4096 MB

Network:
- Leave default unless you have special requirements (static, VLAN, etc)


Confirm and finish. Do not start the VM yet.

----

```yaml
qm importdisk 100 haos_ova-18.2.qcow2 local-lvm
```

- Close the node's console and select your HA VM

- Go to the 'Hardware' tab

- Select the 'Unused Disk' and click the 'Edit' button

- Check the 'Discard' box if you're using an SSD then click 'Add'

- Select the 'Options' tab

- Select 'Boot Order' and hit 'Edit'

- Check the newly created drive (likely scsi0) and uncheck everything else

----

- Start the VM

- Check the shell of the VM. If it booted up correctly, you should be greeted with the link to access the Web UI.

- Navigate to <VM IP>:8123

Done. Everything should be up and running now.

```bash
https://forum.proxmox.com/threads/guide-install-home-assistant-os-in-a-vm.143251/
```

#### Ресурсы:    

:white_check_mark: **Оригинальный скрипт съема данных** - [github](https://gist.github.com/dmslabsbr/08970d068e2e021312055e7560bcac9a)    

:white_check_mark: **Скрипт правленный съема данных** - [github](https://raw.githubusercontent.com/a1ex-ak/scripts/refs/heads/main/altlinux/ha_post_temp.sh)    

#### Команды и ссылки из урока:  


:ballot_box_with_check: Установка lm sensors в Proxmox    
```yaml
apt-get install lm_sensors
```
:ballot_box_with_check: Команда для получения данных с датчиков температуры    
```yaml
sensors-detect
```

```yaml
sensors
```

:ballot_box_with_check: Создаем файл для скрипта    
```yaml
nano ha_post_temp.sh
```
Вставляем код [скрипта](https://raw.githubusercontent.com/a1ex-ak/scripts/refs/heads/main/altlinux/ha_post_temp.sh)    
`Ctrl X` - для выхода    
`Y` для сохранения    

:ballot_box_with_check: Права на запуск    
```yaml
chmod +x ha_post_temp.sh
```

:ballot_box_with_check: Ручной запуск    
```yaml
/root/ha_post_temp.sh
```

:ballot_box_with_check: Открываем в редакторе nano
```yaml
export EDITOR=nano
```

:ballot_box_with_check: Планировщик cron    
```yaml
crontab -e
```

:ballot_box_with_check: Ежеминутный запуск    
```yaml
*/1 * * * * /root/ha_post_temp.sh
```
`Ctrl X` - для выхода    
`Y` для сохранения    

:ballot_box_with_check: Перезагрузка сервиса cron    
```yaml
systemctl restart crond
```

:ballot_box_with_check: Проверка    
```yaml
systemctl status crond
```

:ballot_box_with_check: Увеличение диска
```yaml
qm resize 100 scsi0 +32G
```
