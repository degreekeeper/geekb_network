# Курс "Linux Базовый". 8-ой урок - Настройка tftp, nginx, docker





### Пункт 1

Установить и настроить TFTP-сервер, загрузить и скачать оттуда текстовый файл с содержимым на свое усмотрение.



Установим пакет с tftp-сервером tftp-hpa



![скрин 1](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_1.jpg)



Добавим сервис tftp  в автозагрузку и откроем файл конфиг для редактирования





![скрин 2](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_2.jpg)



выставим флаг --create чтобы можно было загружать и скачивать файлы на сервер





![скрин 3](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_3.jpg)





Изменим владельца папки tftp-сервера





![скрин 4](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_4.jpg)





Создадим файл с произовольным содержанием file_for_tftp в папке сервера






![скрин 5](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_5.jpg)





законнектимся с другой ssh-сессии и подключимся к tftp, скачаем файл.





![скрин 6](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_6.jpg)





загрузим файл на сервер - файл vim_txt





![скрин 7](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_7.jpg)







### Пункт 2



Установить и настроить NGINX. Сделать так, чтобы по запросу http://<ip адрес>/main можно было скачать файл /srv/www/main, а по запросу http://<ip адрес>/main.cfg - файл /srv/www/conf/main.cfg. Содержимое файлов может быть произвольным.





добавим репозиторий с пакетом nginx в стандартные репозитории системы





![скрин 8](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_8.jpg)





После этого установим сам nginx





![скрин 9](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_9.jpg)





Запустим сервис nginx и проверим его статус





![скрин 10](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_10.jpg)





Проверим что по ip-адресу виртуалки открывается стартовая страница nginx





![скрин 11](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_11.jpg)





Создадим файл main в папке /srv/www/main, дадим права на его исполнение.





![скрин 12](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_12.jpg)







Создадим файл main.cfg  в папке /src/www/conf и так же дадим на него права на исполнение





![скрин 14](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_14.jpg)





Изменим файл конфига nginx - /etc/nginx/nginx.conf. Создадим правила для скачивания файлов main и main.cfg





![скрин 15](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_15.jpg)





Заставим nginx перечитать файл конфига, а так же проверить его на ошибки. После чего перезапустим сервис nginx. Все ОК.





![скрин 16](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_16.jpg)





Провеяем возможность скачать файлы main и main.cfg





![скрин 18](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_18.jpg)











### Пункт 3



Установить и настроить NGINX с помощью докера, пробросить на другой порт, отличный от 80, убедиться что по этому порту открывается другая страница (которая лежит внутри докер-контейнера)





Устанавливаем пакет docker





![скрин 19](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_19.jpg)





Устанавливаем пакет docker.io





![скрин 20](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_20.jpg)





скачиваем образ(имейдж) контейнера nginx из docker hub





![скрин 21](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_21.jpg)





Видим что он появился в нашем списке образов





![скрин 22](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_22.jpg)





Запускаем образ контейнера и делаем проброс с порта 64444 на 80 для веб-сервера nginx





![скрин 23](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_23.jpg)





Проверяем что контейнер успешно запустился, заходим в контенейр по оболочкой bash





![скрин 24](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_24.jpg)





Посмотрим корень каталога контейнера 





![скрин 25](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_25.jpg)





Выйдем из контейнера и попробуем открыть стартовую страничку nginx в контейнере, как 





![скрин 26](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/8_less_web_tftp/screenshots/Screenshot_26.jpg)





### Конец.