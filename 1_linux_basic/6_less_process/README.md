# Курс "Linux Базовый". 6-ой урок - Процессы, systemd.







### Пункт 1



Изменить конфигурационный файл службы SSH: /etc/ssh/sshd_config,  отключив аутентификацию по паролю PasswordAuthentication no. Выполните  рестарт службы systemctl restart sshd (service sshd restart), верните  аутентификацию по паролю, выполните reload службы systemctl reload sshd  (service sshd reload). В чём различие между действиями restart и reload?




Заходим в файл sshd_config



![скрин 1](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_1.jpg)



Раскомментируем строчку и выставим параметр "no"


![скрин 2](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_2.jpg)





Перезапускаем службу SSH с помощью команды *systemctl restart*. Это жесткий перезапуск, при нем процесс полностью завершается





![скрин 3](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_3.jpg)



Видим что процесс успешно перезапустился, аптайм 12 секунд





![скрин 4](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_4.jpg)





Вернем настройки





![скрин 5](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_5.jpg)





Перезапустим службу SSH с помощью команды *systemctl reload*. Данная команда вызывает только новое считывание файла конфига.



![скрин 6](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_6.jpg)





### Пункт 2





#### Установить программу mc с помощью apt-get install -y mc. Запустите  mc. Откройте вторую ssh сессию. Используя ps, найдите PID процесса mc,  завершите процесс, передав ему сигнал 9.



Устанавливаем пакте mc. Видим что он уже установлен. Открываем во второй ssh-сессии, данную программу



![скрин 7](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_7.jpg)





Запускаем команды ps -ef. Видим что процесс mc имеет PID 2261. Параллельно открываем вторую ssh-сессию.





![скрин 8](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_8.jpg)





Выполняем команду kill и посылаем по номеру PID системный вызов 9(принудительное завершение)





![скрин 9](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_9.jpg)





Видим во второй ssh-сессии, что мы вылетели из mc. И в консоль прилетело сообщение "Killed"



![тут скрин 10](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_10.jpg)





### Пункт 3





#### Создать юнит нового процесса с именем geekbrains, который при старте будет выполнять ping адреса 127.0.0.1. Сделать так, чтобы команда  запускалась сразу при старте операционной системы. Запустить сервис с  помощью systemctl, отследить pid процесса, остановить процесс с помощью  сигналов в top.





Создаем файл geekbrains_ping.sh и открываем его редактором nano.





![скрин 10_1](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_10_1.jpg)



Пишем в скрипт команду которая запускает постоянный пинг локалхоста 127.0.0.1. Сохраняем файл и выходим





![тут скрин 13](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_13.jpg)





Проверяем что такой файл создался. Меняем права доступа к файлу, выставляем возможность запускать скрипт от всех групп пользователей - *chmod ugo+x*.



![тут скрин 12](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_12.jpg)





Создаем юнит geekbrains_ping.service. Настраиваем там следующий конфиг:



![скрин 14](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_14.jpg)



Перезапускаем все  процессы-демон. Проверяем его статус и видим, что сервис неактивный.



![скрин 15](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_15.jpg)





Делеам команду systemctl start после чего проверяем ещё раз статус сервисы командой sustemctl ststus. Видим что процесс активный и в логах виден успешный пинг.





![скрин 16](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_16.jpg)





Включем режим запуска сервиса при каждом старте системы командой *systemctl enable* и дальше проверим статус данной настройки(вкл или выкл) с командой systemctl is-enabled





![тут скрин 17](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_17.jpg)





Перезагружаем виртуальную машину.





![скрин 18](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_18.jpg)





Видим, что процесс после перезагрузки запустился автоматически с системой без дополнительных команд. Так же находим PID процесса с помощью команды *ps -ef| grep ping*. Видим, что это PID 1212





![скрин 19](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_19.jpg)



Запускаем команду  top только по мониторингу процесса PID 1212



![скрин 20](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_20.jpg)





С помощью ключа "k" и PID 1212 и системного вызова 9 - принудительно завершаем процесс. Видим что он пропал.





![скрин 21](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_21.jpg)



Видим, что процесс был завершен 30 секунд назад





![скрин 22](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/6_less_process/screenshots/Screenshot_22.jpg)







### Конец.