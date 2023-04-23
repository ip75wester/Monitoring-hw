# Домашнее задание к занятию "`10.1. Keepalived/vrrp`" - `Илья Попов`


### Задание 1

Разверните топологию из лекции и выполните установку и настройку сервиса Keepalived.

vrrp_instance test {

state "name_mode"

interface "name_interface"

virtual_router_id "number id"

priority "number priority"

advert_int "number advert"

authentication {

auth_type "auth type"

auth_pass "password"

}

unicast_peer {

"ip address host"

}

virtual_ipaddress {

"ip address host" dev "interface" label "interface":vip

}

}

### Требования к результату

В качестве решения предоставьте:
- рабочую конфигурацию обеих нод, оформленную как блок кода в вашем md-файле;

![1 Нода](https://github.com/ip75wester/Monitoring-hw/blob/main/zad1.PNG)

![2 Нода](https://github.com/ip75wester/Monitoring-hw/blob/main/zad2.PNG)

- скриншоты статуса сервисов, на которых видно, что одна нода перешла в MASTER, а вторая в BACKUP state.

![1 Нода](https://github.com/ip75wester/Monitoring-hw/blob/main/zad3.PNG)

![2 Нода](https://github.com/ip75wester/Monitoring-hw/blob/main/zad4.PNG)


---
