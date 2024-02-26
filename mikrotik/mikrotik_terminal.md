### Terminal Mikrotik
https://настройка-микротик.рф

:ballot_box_with_check: сохраниение настроек в текстовый файл
```yaml
export file=config_backup.rsc
```

:ballot_box_with_check: проверка прошивки LTE-модема
```yaml
/interface lte firmware-upgrade lte1
```

:ballot_box_with_check: обновление прошивки LTE-модема
```yaml
/interface lte firmware-upgrade lte1 upgrade=yes
```

:ballot_box_with_check: дефолтные настройки firewall
```yaml
/ip firewall filter
add action=accept chain=input comment="defconf: accept established,related,untracked" connection-state=established,related,untracked
add action=drop chain=input comment="defconf: drop invalid" connection-state=invalid
add action=accept chain=input comment="defconf: accept ICMP" protocol=icmp
add action=drop chain=input comment="defconf: drop all not coming from LAN" in-interface-list=!LAN
add action=accept chain=forward comment="defconf: accept in ipsec policy" ipsec-policy=in,ipsec
add action=accept chain=forward comment="defconf: accept out ipsec policy" ipsec-policy=out,ipsec
add action=fasttrack-connection chain=forward comment="defconf: fasttrack" connection-state=established,related
add action=accept chain=forward comment="defconf: accept established,related, untracked" connection-state=established,related,untracked
add action=drop chain=forward comment="defconf: drop invalid" connection-state=invalid
add action=drop chain=forward comment="defconf: drop all from WAN not DSTNATed" connection-nat-state=!dstnat connection-state=new in-interface-list=WAN
/ip firewall nat
add action=masquerade chain=srcnat comment="defconf: masquerade" ipsec-policy=out,none out-interface-list=WAN
```
```yaml
/ip firewall filter
add action=accept chain=input comment="defconf: accept established,related,untracked" connection-state=established,related,untracked
add action=drop chain=input comment="defconf: drop invalid" connection-state=invalid
add action=accept chain=input comment="defconf: accept ICMP" protocol=icmp
add action=accept chain=input comment="defconf: accept to local loopback (for CAPsMAN)" dst-address=127.0.0.1
add action=drop chain=input comment="defconf: drop all not coming from LAN" in-interface-list=!LAN
add action=accept chain=forward comment="defconf: accept in ipsec policy" ipsec-policy=in,ipsec
add action=accept chain=forward comment="defconf: accept out ipsec policy" ipsec-policy=out,ipsec
add action=fasttrack-connection chain=forward comment="defconf: fasttrack" connection-state=established,related
add action=accept chain=forward comment="defconf: accept established,related, untracked" connection-state=established,related,untracked
add action=drop chain=forward comment="defconf: drop invalid" connection-state=invalid
add action=drop chain=forward comment="defconf: drop all from WAN not DSTNATed" connection-nat-state=!dstnat connection-state=new in-interface-list=WAN
/ip firewall nat
add action=masquerade chain=srcnat comment="defconf: masquerade" ipsec-policy=out,none out-interface-list=WAN
```

:ballot_box_with_check: прячим сеть от ОпСоСа    
`1 Вариант: на выход дать ttl=64:`
```yaml
/ip firewall mangle
add action=change-ttl chain=postrouting new-ttl=set:65 out-interface=lte1 passthrough=yes
```
 
`2 Вариант: повысить приходящий ttl, мтс дает 1 на вход:`
```yaml
/ip firewall mangle
add action=change-ttl chain=prerouting in-interface=lte1 new-ttl=set:5 passthrough=no
```

:ballot_box_with_check: закрываем 53 порт для доступа из вне.
```yaml
/ip firewall filter
add chain=input action=drop protocol=udp in-interface=«провайдера» dst-port=53
```

:ballot_box_with_check: настройка NTP-клиента, сервер из России
```yaml
/system ntp client
set enabled=yes
```
```yaml
/system ntp client servers
add address=ntp1.vniiftri.ru
add address=ntp2.vniiftri.ru
add address=ntp3.vniiftri.ru
add address=ntp4.vniiftri.ru
```

:ballot_box_with_check: включаем NTP-сервер на MikroTik
```yaml
/system ntp server
set enabled=yes manycast=yes multicast=yes
```

:ballot_box_with_check: добавить статические записи для домена: safe.dot.dns.yandex.net
```yaml
/tool fetch url=https://curl.se/ca/cacert.pem
/certificate import file-name=cacert.pem
```
```yaml
/ip dns static
add address=77.88.8.88 comment="Yandex DNS (https://safe.dot.dns.yandex.net/dns-query)" name=safe.dot.dns.yandex.net
add address=77.88.8.2 name=safe.dot.dns.yandex.net
```
```yaml
/ip dns static
add address=45.90.28.0 comment="NextDNS (https://dns.nextdns.io/xxxxxx)" name=dns.nextdns.io
add address=45.90.30.0 name=dns.nextdns.io
```
:ballot_box_with_check: Redirect DNS queries to router:
```yaml
/ip firewall nat add chain=dstnat action=redirect protocol=tcp dst-port=53 
/ip firewall nat add chain=dstnat action=redirect protocol=udp dst-port=53 
```

:ballot_box_with_check: AntiZapret
https://ntc.party/t/mikrotik/666/175
