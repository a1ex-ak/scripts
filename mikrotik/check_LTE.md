
[Sertik`s collection](https://forummikrotik.ru/viewtopic.php?f=14&t=13947)

[Habr Sertik's](https://habr.com/ru/users/Sertik13/)

```yaml
#--------------------------------------------------------
# Test/repair script LTE
#  by Sertik 02/12/2021
#--------------------------------------------------------

:local lteIs [:toarray ""]
:local ltenum 0

# We search through all lte interfaces and place their names in an array $lteIs
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
              :set $ltename [pick $lteIs ($ltenum-1)];: log warning ("Interface check "."$ltename "."....")
                   :if ([/interface lte get $ltename disabled]) do={:log warning "Lte-interface $ltename was disabled. Let's turn it on to check ..."; [/interface lte enable $ltename];
                      :log warning "Lte-interface $ltename included. We are waiting for the modem to initialize ..."; :delay 20s;}
                           :if ([/ping 8.8.8.8 interface=$ltename count=5]>2) do={:log info "The modem is active and connected. There is Internet access via $ltename"; :set $ltenum ($ltenum-1);}
                           :if ([/ping 8.8.8.8 interface=$ltename count=5]=0) do={
                                :if (![/interface lte get $ltename disabled]) do={
                                  # get Lte modem status
                                  :set ltecmd [/interface lte info $ltename once as-value]
                                  :set Ltestatus ($ltecmd->$typepar);
                                  :do {:delay 1s; :set Ltestatus ([/interface lte info $ltename once as-value]->"$typepar")} while=([:len ([/interface lte info $ltename once as-value]->"$typepar")]=0)
                                       :if (($Ltestatus="registered") && ([/ping 8.8.8.8 interface=$ltename count=5]>2)) do={:log warning "The advent of Internet access through $ltename"} else={ 
                                            :if (($Ltestatus="registered") or ($Ltestatus="unknown") or ($Ltestatus="not searching") or ($Ltestatus="denied")) do={
                                            :log error "Lte-interface $ltename active, but does not ping 8.8.8.8 - disable it ...."
                                                :if ([/ping 8.8.8.8 interface=$ltename count=5]>2) do={:log warning "The advent of Internet access through $ltename. Leave it on"} else={
                                                [/interface lte disable $ltename];}
                 :log info "Script disabled interface $ltename"} else={:log warning "Lte-interface $ltename active, its status is not defined, the connection may be being configured ... ";}
                                     }
                                 } else={log info "Lte-interface $ltename disabled in the system"}
:set $ltenum ($ltenum-1);}
               }
 } else={:log warning "LTE-interfaces no found"}

:log warning "Checking LTE-interfaces is completed"
```
