# Курс "Linux Базовый". 7-ой урок - Работа с сетью.







### Пункт 1 и 2


С помощью утилиты netplan повторить имеющиеся в системе параметры, но задать их статически.


Запустить ping до сайта geekbrains.ru, проверить доступность.



Домашнее задание выполнялось на Ubuntu версии 16.04, установить туда netplan не получилось.



Поэтому изменил задание, будем менять DNS сервер в файле /etc/resolv.conf и проверять, что DNS-запросы теперь идут к указанному нами серверу





Настройки сети можно посмотреть и изменить в файле /etc/network/interfaces



![скрин 1](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/7_less_network/screenshots/Screenshot_1.jpg)



Посмотреть текущие DNS можно в файле /etc/resolv.conf

***ВНИМАНИЕ ИЗМЕНЕНИЯ В ДАННОМ ФАЙЛЕ ДЕЙСТВУЮТ ТОЛЬКО ДО ПЕРЕЗАГРУЗКИ!***





![скрин 2](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/7_less_network/screenshots/Screenshot_2.jpg)



Попробуем в одном терминале запустить tcpdump по порту 53 и одновременно запустить пинг до geekbrains.ru. Видим что сервер обратился к DNS прописанному в файле /etc/resolv.conf и получил его ip-адрес для A-записи 178.248.232.209.





![скрин 2_1](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/7_less_network/screenshots/Screenshot_2_1.jpg)



Выберем какой-нибудь публичный DNS - например 77.88.8.3 от Яндекса. Найти их можно по ссылке:



[]: https://public-dns.info/nameserver/ru.html

https://public-dns.info/nameserver/ru.html



![тут скрин 3_1](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/7_less_network/screenshots/Screenshot_3_1.jpg)



Удалим все текущие DNS и оставим только новый, сохраним и закроем файл.





![тут скрин 3_2](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/7_less_network/screenshots/Screenshot_3_2.jpg)







Аналогично запустим в одном терминале tcpdump, в другом пинг скажем до сайта google.com. Видим, что пинг успешный, и запросы DNS идут на новый 77.88.8.3



![тут скрин 3_3](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/7_less_network/screenshots/Screenshot_3_3.jpg)





### Пункт 3


 Не останавливая ping, открыть отдельное окно терминала и вывести на экран только запросы в сторону (echo request) в сторону сайта  geekbrains.ru.



Запустим в одном терминале пинг до geekbrains.ru в другом запустим tcpdump со следующими ключами. Данные ключи будут отлавливать только трафик протокола icmp и только icmp-request





```
sudo tcpdump -n -i enp0s3 icmp[icmptype] = icmp-echo
```





![тут скрин 4](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/7_less_network/screenshots/Screenshot_4.jpg)




### Пункт 4



С помощью утилиты nc проверить доступность основных портов (53,80,443) у сайта geekbrains.ru.



Проверить доступность портов на ресурсе или ip-адресе можно командой





```
nc -vzw1 geekbrains.ru 80
```



Проверка портов 53, 80, 443 для ресурса geekbrains.ru






![тут скрин 5](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/7_less_network/screenshots/Screenshot_5.jpg)






### Конец.