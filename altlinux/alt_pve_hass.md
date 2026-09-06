#### Создание VM HAOS в Альт Виртуализации:

:white_check_mark: Obtain the VM image

- Navigate to the installation page on the HA website: [Alternative](https://www.home-assistant.io/installation/alternative)

- Simply right-click the "KVM/Proxmox (.qcow2)" link and copy the address

- In your Proxmox console (Shell), use wget to download the file

```yaml
wget https://github.com/home-assistant/operating-system/releases/download/18.2/haos_ova-18.2.qcow2.xz
```
```yaml
unxz haos_ova-18.2.qcow2.xz
```
----

:white_check_mark: Create the VM

:ballot_box_with_check: General:
- Select your VM name and ID
- Select 'start at boot'

:ballot_box_with_check: OS:
- Select 'Do not use any media'

:ballot_box_with_check: System:
- Change 'machine' to 'q35'
- Change BIOS to OVMF (UEFI)
- Select the EFI storage (typically local-lvm)
- Uncheck 'Pre-Enroll keys'

:ballot_box_with_check: Disks:
- Delete the SCSI drive and any other disks

:ballot_box_with_check: CPU:
- Set minimum 2 cores

:ballot_box_with_check: Memory:
- Set minimum 4096 MB

:ballot_box_with_check: Network:
- Leave default unless you have special requirements (static, VLAN, etc)


Confirm and finish. Do not start the VM yet.


:white_check_mark: Import Disk

```yaml
qm importdisk 100 haos_ova-18.2.qcow2 local-lvm
```

:ballot_box_with_check: - Close the node's console and select your HA VM

:ballot_box_with_check: - Go to the 'Hardware' tab

:ballot_box_with_check: - Select the 'Unused Disk' and click the 'Edit' button

:ballot_box_with_check: - Check the 'Discard' box if you're using an SSD then click 'Add'

:ballot_box_with_check: - Select the 'Options' tab

:ballot_box_with_check: - Select 'Boot Order' and hit 'Edit'

:ballot_box_with_check: - Check the newly created drive (likely scsi0) and uncheck everything else



:white_check_mark: Start the VM

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
