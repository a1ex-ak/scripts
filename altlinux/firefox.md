:ballot_box_with_check: about:config
```yaml
extensions.pocket.enabled = false
```

```yaml
privacy.resistFingerprinting = true
```
Изменение значения Fingerprinting на «True» поможет сделать «след» Firefox в сети менее заметным.
Примечание. Есть множество факторов, которые влияют на размер отпечатка браузера и способность с точностью идентифицировать вас. Это всего лишь один из способов уменьшить свой «след» в сети.

```yaml
security.OCSP.require = true
```
Запретить подключение к веб-сайту, если невозможно выполнить проверку OCSP.

https://www.securitylab.ru/blog/personal/bezmaly/351054.php
