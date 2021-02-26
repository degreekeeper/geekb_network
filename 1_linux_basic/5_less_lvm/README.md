# Курс "Linux Базовый". 5-ый урок - Работа с разделами, ссылками, LVM.







### Пункт 1



Задание:



 Добавить к серверу второй жесткий диск размером 300 Мегабайт

- a. Разбить диск на 2 раздела по 150 Мб
- b. На первом разделе создать файловую систему Ext4 и примонтировать к папке /mnt/ext4
- c. Добавить точку монтирования /mnt/ext4 в /etc/fstab



Создаем еще один виртуальный SATA жесткий диск в VirtualBox - TESTdisk



![скрин 1](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_1.jpg)




Смотрим с помощью lsblk, что он появился в системе



![скрин 2](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_2.jpg)




Начинаем создавать раздел с помощью команды parted для HDD - sdb



![скрин 2_1](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_2_1.jpg)


размечаем его как GPT


![скрин 3](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_3.jpg)


Создаем раздел размера 150мб, ФС ext4, название PART1, проверяем что он успешно создался


![скрин 4](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_4.jpg)


Форматируем его в файловую систему ext4


![скрин 5](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_5.jpg)





Монтируем новый раздел /dev/sdb1 в созданную специально папку /mnt/ext4/, проверяем что точка монитрования появилась




![скрин 6](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_6.jpg)




Добавляем запись о точке монтирования в файл /etc/fstab, чтобы точка не удалялась после перезагрузки



![скрин 7](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_7.jpg)


Проверяем работу записи в etc/fstab. Перезагружаем виртуальную машину.


![скрин 8](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_8.jpg)


проверяем с помощью lsblk  и cat что точка монтирования осталась.


![скрин 9](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_9.jpg)






### Пункт 2



Задание:


На втором разделе создать локальный том (LVM) размером 100 Мб. Создать файловую систему XFS  и примонтировать к папке /mnt/xfs.



По аналогии с пунктом 1 - через parted создаем ещё один физический раздел размером 100МБ, ФС xfs, названием PART-XFS. Проверяем, что он создался


![скрин 10](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_10.jpg)


Включаем для тома номер 2(это наш новый PART-XFS) возможность использовать LVM


![скрин 10_1](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_10_1.jpg)



Создаем новую Physical Volume Group и после чего новую Volume Group в разделе sdb2



![скрин 10_2](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_10_2.jpg)



Создаем LVM с названием xfs, размером 90Мб, в VG которую мы создали в предыдущем подпункте


![скрин 10_3](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_10_3.jpg)



Проверяем что создалась как VG, так и LVM


![скрин 10_4](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_10_4.jpg)



Форматируем LVM в файловую систему xfs


![скрин 10_5](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_10_5.jpg)



создаем папку */mnt/xfs* и монитируем туда LVM, проверяем, что она смонитровалась с помощью *lsblk*




![скрин 10_6](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_10_6.jpg)









### Пункт 3


Задание:


Создать файл /mnt/ext4/file1 и наполнить его произвольным  содержимым. Скопировать его в file2. Создать символическую ссылку file3  на file1. Создать жёсткую ссылку file4 на file1. Посмотреть, какие inode у файлов. Удалить file1. Что стало с остальными созданными файлами?  Попробовать вывести их на экран.



Создаем файл file1 в папке /mnt/ext4/ и заполняем его текстовой информацией, смотрим что ему присвоился inode 12 и размер его 82байта


![скрин 13](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_13.jpg)



Копируем file1 в file2, проверяем что новый файл имеет другой inode 13


![скрин 14](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_14.jpg)



Делаем жесткую ссылку file4 на file1. Проверяем и видим, что у них одинаковые inode, т.е. фактически это один и тот же файл


![скрин 15](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_15.jpg)


Удаляем file1 и видим что оба файла и file2 и file4 открываются. Потому что файл доступен, пока на него ссылается хотя бы одна жесткая ссылка



![скрин 16](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_16.jpg)









### Пункт 4



Задание:



Создать ссылку вида /mnt/xfs/link , по которой будет доступен /mnt/ext4/file4.



Делаем символьную ссылку /mnt/xfs/link на file4. Проверяем что файл открывается по этой ссылке с помощью cat.



![скрин 17](https://github.com/degreekeeper/geekb_network/blob/main/1_linux_basic/5_less_lvm/screenshots/Screenshot_17.jpg)



### Конец