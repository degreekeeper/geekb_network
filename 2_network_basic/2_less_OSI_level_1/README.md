# Урок 2. Первый уровень модели OSI. Витая пара, оптика, twinax, ограничения первого уровня модели OSI





### Задание:



В eve-ng собрать схему из коммутатора (Switch) и 3-х компьютеров (VPC), подключенных к коммутатору.
 При подключении к одному компьютеру используйте автосогласование  сокрости и дуплекса, ко второму компьютеру выставьте скорость 10 мегабит в секунду и half-duplex, к третьему 100 мегабит в секунду и  автосогласование дуплекса.
 В качестве ответа сдаем скришот с выводом show interfaces status с коммутатора







### Топология





![скрин 0](https://github.com/degreekeeper/geekb_network/blob/main/2_network_basic/2_less_OSI_level_1/screenshots/Screenshot_0.jpg)





### Настройки на SW1




```
!
interface GigabitEthernet0/1
 description PC1
 no negotiation auto
!
interface GigabitEthernet0/2
 description PC2
 speed 10
 duplex half
 no negotiation auto
!
interface GigabitEthernet0/3
 description PC3
 speed 100
 no negotiation auto
!
```



Вывод команды ***show interfaces status***




![скрин 2](https://github.com/degreekeeper/geekb_network/blob/main/2_network_basic/2_less_OSI_level_1/screenshots/Screenshot_2.jpg)



### Настройки на PC





```
set pcname PC1
ip 192.168.10.1 24
```





```
set pcname PC2
ip 192.168.10.2 24
```





```
set pcname PC3
ip 192.168.10.3 24
```





#### Проверка доступности между PC 





![скрин 1](https://github.com/degreekeeper/geekb_network/blob/main/2_network_basic/2_less_OSI_level_1/screenshots/Screenshot_1.jpg)







### Конец.