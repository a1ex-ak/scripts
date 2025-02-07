идём сюда
```yaml
cd /opt/selksd/SELKS/
./easy-setup.sh --non-interactive --no-pull-containers -i eth0 --iA --restart-mode always --es-memory 8G
запускаем
docker compose up -d
```


копируем  скрипты script.tar.gz в /opt
trafr.sh редактируем
```yaml
/usr/bin/tzsp2pcap -f -s 1450 | /usr/bin/tcpreplay --topspeed -i eth0 -
```

tap.sh редактируем 
```yaml
#!/bin/bash

/usr/bin/screen -dmS tzsp2pcap /opt/trafr.sh
```

./tap.sh
старутем


копируем нужные правила в контейнер, например ptopen-all.rules
```yaml
docker cp ptopen-all.rules suricata:/etc/suricata/rules
```
```yaml
docker cp ptopen-all.rules suricata:/var/lib/suricata/rules
```

рестарт сурика
```yaml
cd /opt/selksd/SELKS/docker
```
```yaml
docker compose restart suricata
```

