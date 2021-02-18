# Курс "Linux Базовый". 3-ий урок - Работа с текстом.



### Пункт 1



Делаем следующим набором команд создание трех разных файлов с наполнением с помощью vim, mcedit, nano:


![](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/3_less_text/screenshots/Screenshot_0.jpg)



пример из vim:



![скрин 1](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/3_less_text/screenshots/Screenshot_1.jpg)



пример из mcedit:



![скрин 2](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/3_less_text/screenshots/Screenshot_2.jpg)



пример из nano:



![скрин 3](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/3_less_text/screenshots/Screenshot_3.jpg)



### Пункт 2



Выводим ошибки из /etc/ в файл с помощью данной команды: 


```
cat /etc/* 2> err_from_etc
```



Проверяем что файл создался и там находятся все ошибки STDERR:



![тут скриншот 4](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/3_less_text/screenshots/Screenshot_4.jpg)



С помощью данной команды считаем кол-во строчек с ошибкой в файле (их получилось 99 шт.):



```
wc -l < err_from_etc
```



![тут скрин 5](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/3_less_text/screenshots/Screenshot_5.jpg)



### Пункт 3



данными командами делаем все сортировки в директории /home/user и смотрим результат:



![тут скрин 6](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/3_less_text/screenshots/Screenshot_6.jpg)



### Пункт 4



Данными командами смотрим все скрытые файлы в директории и считаем их кол-во:


![тут скрин 7](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/3_less_text/screenshots/Screenshot_7.jpg)



### Пункт 5



Пример настройки авторизации на сервере Ubuntu через Putty по RSA-ключам:




![тут скрин 8](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/3_less_text/screenshots/Screenshot_8.jpg)




![тут 9](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/3_less_text/screenshots/Screenshot_9.jpg)



![тут 10](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/3_less_text/screenshots/Screenshot_10.jpg)







### Конец.