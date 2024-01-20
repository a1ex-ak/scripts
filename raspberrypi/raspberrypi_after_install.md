# *** Настройка Raspberry PI OS после установки ***

----------------------------------
:ballot_box_with_check: TorrServer (https://github.com/YouROK/TorrServer)
```yaml
curl -s https://raw.githubusercontent.com/YouROK/TorrServer/master/installTorrServerLinux.sh | sudo bash
```
:ballot_box_with_check: Разгон малинки (редактируем файл)
```yaml
sudo gedit /boot/config.txt
```
:ballot_box_with_check: находим строку #arm_freq=800 и вместо неё вставляем
```yaml
arm_freq=2147
gpu_freq=750
over_voltage=6
```
