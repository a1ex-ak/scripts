# *** Настройка Raspberry PI OS после установки ***

----------------------------------
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
