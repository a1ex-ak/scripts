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
:ballot_box_with_check: Docker 24.0.7 (RaspberryOS & Debian) Home Assistent
```yaml
sudo apt install \
docker-compose-plugin=2.21.0-1~debian.12~bookworm \
docker-ce-cli=5:24.0.7-1~debian.12~bookworm \
docker-buildx-plugin=0.11.2-1~debian.12~bookworm \
docker-ce=5:24.0.7-1~debian.12~bookworm \
docker-ce-rootless-extras=5:24.0.7-1~debian.12~bookworm
```
```yaml
curl -fsSL https://get.docker.com -o install-docker.sh
sudo sh install-docker.sh --version 24.0.7-1
```
