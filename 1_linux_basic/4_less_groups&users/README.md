# Курс "Linux Базовый". 4-ый урок - Работа с пользователями и группами.







### Пункт 1



Задание:

#### Управление пользователями:

a. создать пользователя, используя утилиту useradd; 
 b. удалить пользователя, используя утилиту userdel;




Удаляем и создаем пользователя




![скрин 1](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_1.jpg)

![скрин 2](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_2.jpg)



### Пункт 2


Задание:

#### Управление группами:

a. создать группу с использованием утилит; 
 b. попрактиковаться в смене групп у пользователей;
 c. удалить пользователя из группы.




Создаем группу test_group

![скрин 3](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_3.jpg) 


добавляем пользователя user в группу test_group


![скрин 4](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_4.jpg)


удаляем пользователя user из группы test_group

![скрин 5](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_5.jpg)



### Пункт 3


Задание:

#### Добавить пользователя, имеющего право выполнять команды/действия  от имени суперпользователя.  Сделать так, чтобы sudo не требовал пароль  для выполнения команд.


с помощью команды visudo редактируем файл /etc/sudoers

Вставляем туда строчку %sudo ALL=(ALL) NOPASWD:ALL. После чего команда sudo не будет запрашивать пароль каждый раз

![скрин 6](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_6.jpg)

проверяем так ли это и видимо, что пароль запрошен не был

![скрин 7](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_7.jpg)



### Пункт 4


Задание:

#### 4 Создать два произвольных файла. Первому присвоить права на чтение и запись для владельца и группы, только на чтение — для всех. Второму  присвоить права на чтение и запись только для владельца. Сделать это в  численном и символьном виде.



Создаем два файла file1 и file2

![скрин 8](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_8.jpg)


меня права file1 командой chmod с символьным способом



![скрин 9](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_9.jpg)



меняем права file2 командой chmod с цифровым способом



![скрин 10](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_10.jpg)



### Пункт 5*



Задание:


Создать группу developer и нескольких пользователей, входящих в неё. Создать директорию для совместной работы. Сделать так, чтобы созданные  одними пользователями файлы могли изменять другие пользователи этой  группы.



Создаем группу *developer*, создаем двух пользователей *dev1* и *dev2*


![скрин 11](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_11.jpg)

создаем пароли для этих пользователей, чтобы под ними можно было залогиниться

![скрин 13](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_13.jpg)


добавляем так же пользователя user в группу developer

![скрин 17](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_17.jpg)


Проверяем что все наши три пользователя есть в группе developer


![скрин 18](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_18.jpg)


Создаем директорию dir_for_developers и выставляем бит SGID для директории


![скрин 12](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_12.jpg)


меняем группу для директории  dir_for_developers группу на developer


![скрин 15](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_15.jpg)







логинимся под каждым из пользователей и создаем файлы


![скрин 19](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/4_less_groups%26users/screenshots/Screenshot_19.jpg)





Видим, что у всех файлов общая группа, не смотря на то, что владелец разный. Что в совокупности с битом SGID даст редактировать и испольнять файлы всем пользователям группы SGID в этой папке, не смотря на то что эти могут быть созданы другими пользователями из этой группы.



### Конец

