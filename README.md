# otus-pgsql-hw-lesson-3

создать ВМ с Ubuntu 20.04/22.04 или развернуть докер любым удобным способом
Работаю с Centos 7.0 так как использовал корпоративный стандартный образ. Ubuntu запрещён в компании.
поставить на нем Docker Engine

Установил в соответствии from official docks https://docs.docker.com/engine/install/centos/

    [root@mck-network-test ~]# systemctl start docker
    [root@mck-network-test ~]# docker --version
    Docker version 24.0.7, build afdd53b
    [root@mck-network-test ~]#


done

сделать каталог /var/lib/postgres

        [root@mck-network-test ~]# cd /var/lib/postgres
        [root@mck-network-test postgres]# ls -l
        total 0
        [root@mck-network-test postgres]#

done

развернуть контейнер с PostgreSQL 15 смонтировав в него /var/lib/postgresql

развернуть контейнер с клиентом postgres

подключится из контейнера с клиентом к контейнеру с сервером и сделать таблицу с парой строк

подключится к контейнеру с сервером с ноутбука/компьютера извне инстансов GCP/ЯО/места установки докера

удалить контейнер с сервером

создать его заново

подключится снова из контейнера с клиентом к контейнеру с сервером

проверить, что данные остались на месте

оставляйте в ЛК ДЗ комментарии что и как вы делали и как боролись с проблемами
