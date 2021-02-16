# Установака Ubuntu Server LTS 16.04



1. Цель задания

   - создать виртуальную машину в VirtualBox

   - установить Ubuntu Server на виртуальную машину
   - настроить Ubuntu Server для работы
   - установить на Ubuntu необходимые пакеты mc, ssh-server, make, gcc, perl



2. Выполнение настройки



Виртуалку настроим следующим образом. Выберем сетевое подключение "мост"



![тут скринш 17](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/1_less_install/screenshots/Screenshot_17.jpg)



Выберем в CDROM скачанный нами образ Ubuntu_Server. Сделать это можно нажав на изображение диска



![тут скриншот 18](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/1_less_install/screenshots/Screenshot_18.jpg)



## LOGIN: user

## PASS: user



Запускаем виртуальную машину, он начинает загружаться с CDROM:



Пример распределния разделов/томов. 

- раздел /boot выделен в отдельный том, т.к. он нужен для загрузчика размеров 1ГБ
- создаим Volume Group
- создадим в VG новый LVM под swap размеров 2 ГБ
- создадим ещё один LVM под остальное неразмеченное пространство 7ГБ



![тут скриншот 5](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/1_less_install/screenshots/Screenshot_5.jpg)



Установим необходимые пакеты



![тут скриншот 6](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/1_less_install/screenshots/Screenshot_6.jpg)



Установим GRUB, т.к. на виртуалке это единственная Операционка



![тут скриншот 7](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/1_less_install/screenshots/Screenshot_7.jpg)



загрузимся в систему, проверим какая версия ОС у нас стоит командой ***uname -a***



![тут скриншот 8](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/1_less_install/screenshots/Screenshot_8.jpg)



установим пакет утилит для VirtBox



![тут скриншот 9](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/1_less_install/screenshots/Screenshot_9.jpg)



установим mc, make, gcc, perl. Пример запуска mc



![тут скрин 10](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/1_less_install/screenshots/Screenshot_10.jpg)



проверим что Ubuntu доступны интернет ресурсы



![тут скриншот 11](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/1_less_install/screenshots/Screenshot_11.jpg)



попробуем подключиться к Ubuntu по SSH через Putty. Для начала узнаем какой ip-адрессе получила машина по DHCP



![тут скриншот 14](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/1_less_install/screenshots/Screenshot_14.jpg)



введем это ip в Putty



![тут скриншот 15](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/1_less_install/screenshots/Screenshot_15.jpg)



подтвердим сертификат



![тут скриншот 12](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/1_less_install/screenshots/Screenshot_12.jpg)



введем пароль и видим что успешно авторизовались по SSH



![тут скриншот 13](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/1_less_install/screenshots/Screenshot_13.jpg)



перезагрузим систему и изменим порядок загрузи, чтобы каждый раз начиналась установка новой ОС с загрузочного образа с CDROM. Для этого вверх передвинем Жесткий Диск



![тут скришот 16](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/1_less_install/screenshots/Screenshot_16.jpg)





### Конец Задания.

