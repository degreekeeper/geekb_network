### Урок 2. Policy-Based routing, VRF



![topology](https://github.com/degreekeeper/geekb_network/blob/main/4_routing_basic/2_less_PBR_VRF/screenshots/topology.jpg)

1. Назначте IP адреса согласно схемы.
2. VPC6 должен пинговать VPC7
3. VPC5 должен пинговать VPC8
4. VPC8 должен пинговать VPC7
5. Не забываем о том, что "стандартная таблица маршрутизации" называется global
6. Внимательно изучаем возможность роут-мапа с точки зрения действия set, понимаем, что трафик ходит в оба направления, и, благодоря ip policy может покидать vrf либо оказываться в нем





### Концепция:

В данном ДЗ мы не можем использовать ничего кроме PBR и VRF для маршрутизации.


### Команды:



***set ip vrf xxx next-hop y.y.y.y*** - сразу перенаправляет на next-hop в определенном vrf без поиска в таблице маршрутизации

***set vrf xxx*** - кладет трафик в таблицу маршрутизации определенного vrf, дальше происходит поиск нужного маршрута в таблице данного vrf

***set global*** - кладет трафик в глобальную таблицу маршрутизации(как бы убирает vrf если изначально в нем был интерфейс), дальше происходит поиск нужного маршрута в таблице 

***set ip global*** -  сразу перенаправляет на next-hop в глобальную таблицу(GRT) без поиска в таблице маршрутизации



### Настройки R1



VPC5 и VPC6 имеют адреса из одной сети 10.0.0.0/24. Но при этом подключены к разным портам одного и того роутера R1. Для этого оба эти интерфейсы  eth0/0 и eth0/1 будут настроены в разных vrf - vrf VPC6 и VPC5. Соответственно eth0/3 так же будет в vrf VPC6, eth0/2 будет в VPC5. Не забываем, что vrf надо создать:

```
!
ip vrf VPC5
!
ip vrf VPC6
!
interface Ethernet0/0
 ip vrf forwarding VPC5
 ip address 10.0.0.1 255.255.255.0
 ip policy route-map RM_VPC5_to_VPC8
!
interface Ethernet0/1
 ip vrf forwarding VPC6
 ip address 10.0.0.1 255.255.255.0
 ip policy route-map RM_VPC6_to_VPC7
!
interface Ethernet0/2
 ip vrf forwarding VPC5
 ip address 172.16.14.1 255.255.255.0
 ip policy route-map RM_VPC8_to_VPC7
!
interface Ethernet0/3
 ip vrf forwarding VPC6
 ip address 172.16.13.1 255.255.255.0
 ip policy route-map RM_VPC7_to_VPC8
!
```



Так на R1 нет маршрутов до сетей VPC7 и VPC8. Будем делать PBR:



Создаем ACL - *ACL_VPC6_to_VPC7* чтобы отлавливать(фильтровать) трафик именно от VPC6 о VPC7. Создаем route-map -*RM_VPC6_to_VPC7* который только трафик от VPC6 о VPC7 отлавливает(фильтрует) и перенаправляет сразу на next-hop 172.16.13.2 на R3. Навешиваем данный route-map на входящий интерфейс eth0/1 за которым находится VPC6.



```
!
ip access-list extended ACL_VPC6_to_VPC7
 permit ip host 10.0.0.6 host 192.168.3.7
!
route-map RM_VPC6_to_VPC7 permit 10
 match ip address ACL_VPC6_to_VPC7
 set ip next-hop 172.16.13.2
!
interface Ethernet0/1
 ip vrf forwarding VPC6
 ip address 10.0.0.1 255.255.255.0
 ip policy route-map RM_VPC6_to_VPC7
!
```



Создаем ACL - *ACL_VPC5_to_VPC8* чтобы отлавливать(фильтровать) трафик именно от VPC5 до VPC8. Создаем route-map - *RM_VPC5_to_VPC8* который только трафик от VPC5 о VPC8 отлавливает(фильтрует) и перенаправляет сразу на next-hop 172.16.14.2 на R4. Навешиваем данный route-map на входящий интерфейс eth0/0 за которым находится VPC5



```
!
ip access-list extended ACL_VPC5_to_VPC8
 permit ip host 10.0.0.5 host 192.168.4.8
!
route-map RM_VPC5_to_VPC8 permit 10
 match ip address ACL_VPC5_to_VPC8
 set ip next-hop 172.16.14.2
!
interface Ethernet0/0
 ip vrf forwarding VPC5
 ip address 10.0.0.1 255.255.255.0
 ip policy route-map RM_VPC5_to_VPC8
!
```



Дальше нам надо выполнить пункт 4. Перенаправить трафик в обоих направлениях между VPC7 и VPC8 в нужные нам интерфейсы eth0/2 и eth0/3 с помощьюу PBR, плюс положить их в нужные vrf т.к. эти интерфейсы находятся в разных vrf.



Делаем сначала ACL - *ACL_VPC7_VPC8* и *ACL_VPC8_to_VPC7* - которые отлавливают трафик в обоих направлениях.



```
!
ip access-list extended ACL_VPC7_VPC8
 permit ip host 192.168.3.7 host 192.168.4.8
!
ip access-list extended ACL_VPC8_to_VPC7
 permit ip host 192.168.4.8 host 192.168.3.7
!
```



Далее создаем route-map: *RM_VPC8_to_VPC7* и  *RM_VPC7_to_VPC8* на основании этих ACL. Которые перенаправляют трафик сразу на нужный next-hop сразу в обход таблицы маршрутизаци+ подменяют vrf.





```
!
route-map RM_VPC8_to_VPC7 permit 10
 match ip address ACL_VPC8_to_VPC7
 set ip vrf VPC6 next-hop 172.16.13.2
!
route-map RM_VPC7_to_VPC8 permit 10
 match ip address ACL_VPC7_VPC8
 set ip vrf VPC5 next-hop 172.16.14.2
!
```





Вешаем их на нужные нам интерфейсы.





```
!
interface Ethernet0/2
 ip vrf forwarding VPC5
 ip address 172.16.14.1 255.255.255.0
 ip policy route-map RM_VPC8_to_VPC7
!
interface Ethernet0/3
 ip vrf forwarding VPC6
 ip address 172.16.13.1 255.255.255.0
 ip policy route-map RM_VPC7_to_VPC8
!
```



#### Выводы команд на R1

![](https://github.com/degreekeeper/geekb_network/blob/main/4_routing_basic/2_less_PBR_VRF/screenshots/R1_ACL_PBR.jpg)



![](https://github.com/degreekeeper/geekb_network/blob/main/4_routing_basic/2_less_PBR_VRF/screenshots/R1_routing_vrfs.jpg)



### Настройки на R3


После того как трафик от VPC6 и VPC8 дойдет до VPC7 и вернется на R3. Маршрута до VCP6 и VPC8 на R3 не будет. Для этого настраиваем PBR.



Сначала создаем ACL для отлавливания(фильрации) трафика VPC7->VPC6 и VPC7->VCP8



```
!
ip access-list extended ACL_VPC7_to_VPC6
 permit ip host 192.168.3.7 host 10.0.0.6
!
ip access-list extended ACL_VPC7_to_VPC8
 permit ip host 192.168.3.7 host 192.168.4.8
!
```



Т.к. обратный трафик заходит в один интерфейс делаем route-map с двумя clauses(правилами) для обоих ACL



```
!
route-map VPC7_to_VPC6_and_VPC8 permit 10
 match ip address ACL_VPC7_to_VPC6
 set ip next-hop 172.16.13.1
!
route-map VPC7_to_VPC6_and_VPC8 permit 20
 match ip address ACL_VPC7_to_VPC8
 set ip next-hop 172.16.13.1
!
```



Применяем route-map на интерфейсе

```
!
interface Ethernet0/1
 ip address 192.168.3.1 255.255.255.0
 ip policy route-map VPC7_to_VPC6_and_VPC8
!
```



#### Выводы команд на R3



![](https://github.com/degreekeeper/geekb_network/blob/main/4_routing_basic/2_less_PBR_VRF/screenshots/R3.jpg)



### Настройки на R4



Ситуация аналогичная R3



После того как трафик от VPC5 и VPC7 дойдет до VPC8 и вернется на R4 маршрута до VCP5 и VPC7 на R4 не будет. Для этого настраиваем PBR.




Сначала создаем ACL для отлавливания(фильрации) трафика VPC8->VPC5 и VPC8->VCP7

```
!
ip access-list extended ACL_VPC8_to_VPC6
 permit ip host 192.168.4.8 host 10.0.0.5
!
ip access-list extended ACL_VPC8_to_VPC7
 permit ip host 192.168.4.8 host 192.168.3.7
!
```



Т.к. обратный трафик заходит в один интерфейс делаем route-map с двумя clauses(правилами) для обоих ACL

```
!
route-map RM_VPC8_to_VPC5 permit 10
 match ip address ACL_VPC8_to_VPC6
 set ip next-hop 172.16.14.1
!
route-map RM_VPC8_to_VPC5 permit 20
 match ip address ACL_VPC8_to_VPC7
 set ip next-hop 172.16.14.1
!
```



Применяем route-map на интерфейсе



```
!
interface Ethernet0/1
 ip address 192.168.4.1 255.255.255.0
 ip policy route-map RM_VPC8_to_VPC5
!
```





#### Выводы команд на R4



![](https://github.com/degreekeeper/geekb_network/blob/main/4_routing_basic/2_less_PBR_VRF/screenshots/R4.jpg)



### Резултаты пингов (проверка выполнения условий ДЗ)



#### VPC5

![](https://github.com/degreekeeper/geekb_network/blob/main/4_routing_basic/2_less_PBR_VRF/screenshots/VPC5.jpg)



#### VPC6

![](https://github.com/degreekeeper/geekb_network/blob/main/4_routing_basic/2_less_PBR_VRF/screenshots/VPC6.jpg)



#### VPC7

![](https://github.com/degreekeeper/geekb_network/blob/main/4_routing_basic/2_less_PBR_VRF/screenshots/VPC7.jpg)



#### VPC8

![](https://github.com/degreekeeper/geekb_network/blob/main/4_routing_basic/2_less_PBR_VRF/screenshots/VPC8.jpg)





### Конец.