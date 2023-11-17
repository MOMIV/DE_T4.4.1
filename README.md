﻿# DE_T4.4.1
Задание 4.4.1
Что может быть прекраснее летнего корпоратива? Особенно если корпоратив пройдет в Таиланде! Да-да, отчетный период показал очень хорошую прибыль вашей компании — и не без вашего участия! Чтобы поощрить за труд и терпение, ваш начальник решил организовать корпоратив прямо на островах. По приезду на место первое, что попало на глаза вам и вашим коллегам, — это цирковое шоу слонов. Зрелище было замечательным, у вас возникла невольная ассоциация и вы воскликнули: «О, оживший Hadoop!». Что же, инициатива, как говорится, ложится на плечи инициатора. Теперь ваш начальник под двойным впечатлением поставил вам первоочередную задачу после отпуска — завести hadoop для задач вашей компании. 

 Беремся за дело — и уже по традиции ищем нужный образ docker. Находим его здесь: GitHub - tech4242/docker-hadoop-hive-parquet: Hadoop, Hive, Parquet and Hue in docker-compose v3.

Устанавливается он весьма просто:

1. git clone GitHub - tech4242/docker-hadoop-hive-parquet: Hadoop, Hive, Parquet and Hue in docker-compose v3 (в заранее созданной папке)

2. cmd → docker-compose up

Отлично, как только образ собран, а контейнер поднят — заходим по http://localhost:8888/hue и попадаем в HUE. Придумываем произвольную пару логина-пароля для будущей авторизации и приступаем к работе. 

В интерфейсе hue пока для нас мало интересного, переходим в раздел «files» и наблюдаем примерно такую картину:

![Image alt](https://github.com/MOMIV/DE_T4.4.1/raw/main/pic/01.png)

Это ваша личная папка в файловой системе hadoop(hdfs), и сейчас мы будем учиться с ней работать!

• На время забываем про hue и вспоминаем уроки литературы — переходим на следующий ресурс и скачиваем все доступные тома произведения «Война и мир» Л.Н. Толстого: Лев Николаевич Толстой - Информация об авторе, биография, дата и место рождения, род деятельности, награды и премии, годы творчества.

• Далее подключаемся к контейнеру «datanode-1», создаем внутри папку и переносим в нее скачанные файлы.

• Файлы предварительно «схлопываем» в один.

• Загружаем полученный файл на hdfs в вашу личную папку.

• Если все пройдет удачно, то по возвращению в hue вас ждет сюрприз: вы увидите полное произведение «Война и мир» на hdfs (не забудьте обновить страницу). На самом деле интерфейс HUE поддерживает возможность переноса небольших(!) файлов с помощью drag&drop — но для нас это слишком просто.

• Возвращаемся в терминал и продолжаем изучать hdfs — попробуйте выполнить команду, которая выводит содержимое  вашей личной папки. Обратите внимание на права доступа к вашим файлам — их явно недостаточно, если вы решите поделиться столь важной книгой с вашим коллегой — давайте изменим права доступа к нашему файлу. Установите режим доступа, который дает полный доступ для владельца файла, а для сторонних пользователей возможность читать и выполнять.

• Попробуйте заново использовать команду для вывода содержимого папки и обратите внимание как изменились права доступа к файлу.

• Теперь попробуем вывести на экран информацию о том, сколько места на диске занимает наш файл. Желательно, чтобы размер файла был удобочитаемым.

• На экране вы можете заметить 2 числа. Первое число — это фактический размер файла, а второе — это занимаемое файлом место на диске с учетом репликации — измените фактор репликации на 2.

• Повторите команду, которая выводит информацию о том, какое место на диске занимает файл и убедитесь, что изменения произошли.

• И финальное — напишите команду, которая подсчитывает количество строк в произведении «Война и мир».

Решение:

1. «Схлопываем» файлы:
K:\DE_Projects\docker-hadoop-hive-parquet>copy *.txt vim.txt
voyna-i-mir-tom-1.txt
voyna-i-mir-tom-2.txt
voyna-i-mir-tom-3.txt
voyna-i-mir-tom-4.txt
Скопировано файлов:         1.
2. Подключаемся к контейнеру «datanode-1»
K:\DE_Projects\docker-hadoop-hive-parquet>docker ps
CONTAINER ID   IMAGE                                                    COMMAND                  CREATED          STATUS                    PORTS                                          NAMES
dbc9d0e06b55   gethue/hue:4.6.0                                         "./startup.sh"           54 minutes ago   Up 54 minutes             0.0.0.0:8888->8888/tcp                         docker-hadoop-hive-parquet-hue-1
a863ff5aaf4b   bde2020/hadoop-datanode:2.0.0-hadoop2.7.4-java8          "/entrypoint.sh /run…"   54 minutes ago   Up 54 minutes (healthy)   0.0.0.0:62149->50075/tcp                       docker-hadoop-hive-parquet-datanode-1
6bad240a2021   bde2020/hive-metastore-postgresql:2.3.0                  "/docker-entrypoint.…"   54 minutes ago   Up 54 minutes             0.0.0.0:5432->5432/tcp                         docker-hadoop-hive-parquet-hive-metastore-postgresql-1
f7745c3201fe   bde2020/hive:2.3.2-postgresql-metastore                  "entrypoint.sh /bin/…"   54 minutes ago   Up 54 minutes             0.0.0.0:10000->10000/tcp, 10002/tcp            docker-hadoop-hive-parquet-hive-server-1
c65a37d1b4ae   bde2020/hive:2.3.2-postgresql-metastore                  "entrypoint.sh /opt/…"   54 minutes ago   Up 54 minutes             10000/tcp, 0.0.0.0:9083->9083/tcp, 10002/tcp   docker-hadoop-hive-parquet-hive-metastore-1
2c1cf7035986   bde2020/hadoop-namenode:2.0.0-hadoop2.7.4-java8          "/entrypoint.sh /run…"   54 minutes ago   Up 54 minutes (healthy)   0.0.0.0:62151->50070/tcp                       docker-hadoop-hive-parquet-namenode-1
393158bc6be9   postgres:12.1-alpine                                     "docker-entrypoint.s…"   54 minutes ago   Up 54 minutes             0.0.0.0:62152->5432/tcp                        docker-hadoop-hive-parquet-huedb-1
7a7d93c21127   bde2020/hadoop-resourcemanager:2.0.0-hadoop2.7.4-java8   "/entrypoint.sh /run…"   54 minutes ago   Up 54 minutes (healthy)   8088/tcp                                       docker-hadoop-hive-parquet-resourcemanager-1

K:\DE_Projects\docker-hadoop-hive-parquet>docker exec -it a863ff5aaf4b bash
root@a863ff5aaf4b:/# ls
bin  boot  dev  entrypoint.sh  etc  hadoop  hadoop-data  home  lib  lib64  media  mnt  opt  proc  root  run  run.sh  sbin  srv  sys  tmp  usr  var
3. Создаем внутри «datanode-1» папку 
root@a863ff5aaf4b:/# mkdir /vim
root@a863ff5aaf4b:/# ls
bin  boot  dev  entrypoint.sh  etc  hadoop  hadoop-data  home  lib  lib64  media  mnt  opt  proc  root  run  run.sh  sbin  srv  sys  tmp  usr  var  vim
4. Переносим в паку наш файл
K:\DE_Projects\docker-hadoop-hive-parquet>docker cp K:\DE_Projects\docker-hadoop-hive-parquet\vim.txt a863ff5aaf4b:vim
Successfully copied 3.05MB to a863ff5aaf4b:vim

K:\DE_Projects\docker-hadoop-hive-parquet>docker exec -it a863ff5aaf4b bash
root@a863ff5aaf4b:/# cd vim
root@a863ff5aaf4b:/vim# ls
vim.txt
5. Загружаем файл на hdfs в свою личную папку
root@a863ff5aaf4b:/vim# hadoop fs -put  vim.txt /user/momiv

![Image alt](https://github.com/MOMIV/DE_T4.4.1/raw/main/pic/1.png)


6. Выводим содержимое  личной папки
root@a863ff5aaf4b:/vim# hadoop fs -ls /user/momiv
Found 1 items
-rw-r--r--   3 root momiv    3048009 2023-11-17 19:40 /user/momiv/vim.txt
7. Установим режим доступа, который дает полный доступ для владельца файла, а для сторонних пользователей возможность читать и выполнять.
root@a863ff5aaf4b:/vim# hadoop fs -chmod 755 /user/momiv/vim.txt
8. Выводим содержимое  личной папки повторно
root@a863ff5aaf4b:/vim# hadoop fs -ls /user/momiv
Found 1 items
-rwxr-xr-x   3 root momiv    3048009 2023-11-17 19:40 /user/momiv/vim.txt
9. Выводим на экран информацию о том, сколько места на диске занимает наш файл
root@a863ff5aaf4b:/vim# hadoop fs -du -s -h /user/momiv/vim.txt
2.9 M  /user/momiv/vim.txt
10. Изменим фактор репликации на 2
root@a863ff5aaf4b:/vim# hadoop fs -setrep 3 /user/momiv/vim.txt
11. Выводим на экран информацию о том, сколько места на диске занимает наш файл
root@a863ff5aaf4b:/vim# hadoop fs -ls /user/momiv
Found 1 items
-rwxr-xr-x   2 root momiv    3048009 2023-11-17 19:40 /user/momiv/vim.txt
root@a863ff5aaf4b:/vim# hadoop fs -du -h /user/momiv/vim.txt
2.9 M  /user/momiv/vim.txt
12. Подсчитаем количество строк в произведении «Война и мир»
root@a863ff5aaf4b:/vim# hadoop fs -cat /user/momiv/vim.txt | wc -l
10272


