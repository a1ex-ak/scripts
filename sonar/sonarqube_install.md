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

```yaml
sudo useradd -M -d /opt/sonarqube/ -r -s /bin/bash sonarqube
```
```yaml
sudo chown -R sonarqube: /opt/sonarqube
```

```yaml
sudo nano /etc/systemd/system/sonarqube.service
```
```yaml
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking
ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop
User=sonarqube
Group=sonarqube
Restart=always
LimitNOFILE=65536
LimitNPROC=4096

[Install]
WantedBy=multi-user.target
```
```yaml
sudo systemctl daemon-reload
sudo systemctl restart sonarqube
```
```yaml
sudo systemctl enable sonarqube
```
