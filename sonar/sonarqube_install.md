```yaml
sudo apt install net-tools wget unzip curl
```
```yaml
sudo nano /etc/sysctl.conf
```
```yaml
vm.max_map_count=262144
fs.file-max=65536
```
```yaml
sudo sysctl --system
```

```yaml
sudo apt install openjdk-17-jdk
```

```yaml
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.7.96285.zip
```
```yaml
unzip sonarqube-*.zip
```
```yaml
sudo mv sonarqube-*/ /opt/sonarqube
```
