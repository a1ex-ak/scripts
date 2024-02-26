
[Sertik`s collection](https://forummikrotik.ru/viewtopic.php?f=14&t=13947)

[Habr Sertik's](https://habr.com/ru/users/Sertik13/)

```yaml
#--------------------------------------------------------
# Скрипт проверки/восстановления работоспособности LTE
#                     by Sertik 02/12/2021
#--------------------------------------------------------

:local lteIs [:toarray ""]
:local ltenum 0

# Находим перебором все lte-интерфесы и помещаем их имена в массив $lteIs
:foreach counter in=[/interface lte find] do={
:set ($lteIs->$ltenum) [/interface lte get $counter name];
:set $ltenum ($ltenum+1)
}

:if ([:len $lteIs]!=0) do={
   :log info ""
    :log warning ("Роутер "."$[/system identity get name] "."lte-модем: "."$[:len $lteIs]")
    :local ltecmd
    :local Ltestatus
    :local typepar "registration-status";
#:local ltenum [:len $lteIs]
    :local ltename
         :while ($ltenum!=0) do={
#:set $ltename ("lte"."$[:tostr $ltenum]")
              :set $ltename [pick $lteIs ($ltenum-1)];: log warning ("Проверка интерфейса "."$ltename "."....")
                   :if ([/interface lte get $ltename disabled]) do={:log warning "Lte-интерфейс $ltename был отключен. Включаем его для проверки ..."; [/interface lte enable $ltename];
                      :log warning "Lte-интерфейс $ltename включен. Ожидаем инициализации модема ..."; :delay 20s;}
                           :if ([/ping 8.8.8.8 interface=$ltename count=5]>2) do={:log info "Модем активен и подключен. Есть выход в Интернет через $ltename"; :set $ltenum ($ltenum-1);}
                           :if ([/ping 8.8.8.8 interface=$ltename count=5]=0) do={
                                :if (![/interface lte get $ltename disabled]) do={
                                  # get Lte modem status
                                  :set ltecmd [/interface lte info $ltename once as-value]
                                  :set Ltestatus ($ltecmd->$typepar);
                                  :do {:delay 1s; :set Ltestatus ([/interface lte info $ltename once as-value]->"$typepar")} while=([:len ([/interface lte info $ltename once as-value]->"$typepar")]=0)
                                       :if (($Ltestatus="registered") && ([/ping 8.8.8.8 interface=$ltename count=5]>2)) do={:log warning "Появление доступа в интернет через $ltename"} else={ 
                                            :if (($Ltestatus="registered") or ($Ltestatus="unknown") or ($Ltestatus="not searching") or ($Ltestatus="denied")) do={
                                            :log error "Lte интерфейс $ltename активен, но не пингует 8.8.8.8 - отключаем его ...."
                                                :if ([/ping 8.8.8.8 interface=$ltename count=5]>2) do={:log warning "Появление доступа в интернет через $ltename. Оставляем включенным"} else={
                                                [/interface lte disable $ltename];}
                 :log info "Скрипт отключен интерфейс $ltename"} else={:log warning "Lte интерфейс $ltename активен, его статус не определен, возможно идет настройка соединения ... ";}
                                     }
                                 } else={log info "Lte интерфейс $ltename в системе отключен"}
:set $ltenum ($ltenum-1);}
               }
 } else={:log warning "LTE-интерфейсы не найдены"}

:log warning "Проверка LTE-интерфейсов окончена"
```
