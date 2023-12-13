# otus-pgsql-hw-lesson-3

создать ВМ с Ubuntu 20.04/22.04 или развернуть докер любым удобным способом
Работаю с Centos 7.0 так как использовал корпоративный стандартный образ. Ubuntu запрещён в компании.
поставить на нем Docker Engine

## 1.   Установка

Установил в соответствии from official docks https://docs.docker.com/engine/install/centos/

    [root@mck-network-test ~]# systemctl start docker
    [root@mck-network-test ~]# docker --version
    Docker version 24.0.7, build afdd53b
    [root@mck-network-test ~]#


done

## 2.    сделать каталог /var/lib/postgres

        [root@mck-network-test ~]# cd /var/lib/postgres
        [root@mck-network-test postgres]# ls -l
        total 0
        [root@mck-network-test postgres]#

done

## 3.    развернуть контейнер с PostgreSQL 15 смонтировав в него /var/lib/postgresql

        [root@mck-network-test data]# docker network create pg-net
        91f346ca3515cf9ebf7fe1d54a5fb8c633971c5dad31ffd38a09a7f95fbccaff
        [root@mck-network-test data]#


        [root@mck-network-test data]# docker run --name pg-server --network pg-net -e POSTGRES_PASSWORD=postgres -d -p 5433:5432 -v /var/lib/postgres:/var/lib/postgresql/data postgres:15
        Unable to find image 'postgres:15' locally
        15: Pulling from library/postgres
        1f7ce2fa46ab: Pull complete
        90fbd05a6c64: Pull complete
        c107a494d63c: Pull complete
        8b1f93317ded: Pull complete
        b4e0db51dfb9: Pull complete
        ffcd2018dc44: Pull complete
        bb8fb522edb2: Pull complete
        d83898b9620b: Pull complete
        7c74348418e9: Pull complete
        a56b24ebf993: Pull complete
        aa48b46b9cdb: Pull complete
        be71efae0c17: Pull complete
        22902c020f47: Pull complete
        Digest: sha256:cc6dcc9ec987ce2887ac746114a72b288e490ba14f4658731c2822600aab8d00
        Status: Downloaded newer image for postgres:15
        23676e74f5081814126abc8b7fd99dd365ccb74c55f5ea696a61a18f91555a14

        [root@mck-network-test data]# docker ps -a
        CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                  PORTS                                       NAMES
        23676e74f508   postgres:15   "docker-entrypoint.s…"   17 seconds ago   Up 15 seconds           0.0.0.0:5433->5432/tcp, :::5433->5432/tcp   pg-server
        2fce7f112590   hello-world   "/hello"                 8 days ago       Exited (0) 8 days ago                                               cranky_mayer
        [root@mck-network-test data]#




        [root@mck-network-test lib]# cd ./postgres/
        [root@mck-network-test postgres]#
        [root@mck-network-test postgres]# ls -l
        total 0


        [root@mck-network-test postgres]# ls -l
        total 64
        drwx------. 5 polkitd input    33 Dec 13 01:26 base
        drwx------. 2 polkitd input  4096 Dec 13 01:28 global
        drwx------. 2 polkitd input     6 Dec 13 01:26 pg_commit_ts
        drwx------. 2 polkitd input     6 Dec 13 01:26 pg_dynshmem
        -rw-------. 1 polkitd input  4821 Dec 13 01:26 pg_hba.conf
        -rw-------. 1 polkitd input  1636 Dec 13 01:26 pg_ident.conf
        drwx------. 4 polkitd input    68 Dec 13 01:31 pg_logical
        drwx------. 4 polkitd input    36 Dec 13 01:26 pg_multixact
        drwx------. 2 polkitd input     6 Dec 13 01:26 pg_notify
        drwx------. 2 polkitd input     6 Dec 13 01:26 pg_replslot
        drwx------. 2 polkitd input     6 Dec 13 01:26 pg_serial
        drwx------. 2 polkitd input     6 Dec 13 01:26 pg_snapshots
        drwx------. 2 polkitd input     6 Dec 13 01:26 pg_stat
        drwx------. 2 polkitd input     6 Dec 13 01:26 pg_stat_tmp
        drwx------. 2 polkitd input    18 Dec 13 01:26 pg_subtrans
        drwx------. 2 polkitd input     6 Dec 13 01:26 pg_tblspc
        drwx------. 2 polkitd input     6 Dec 13 01:26 pg_twophase
        -rw-------. 1 polkitd input     3 Dec 13 01:26 PG_VERSION
        drwx------. 3 polkitd input    60 Dec 13 01:26 pg_wal
        drwx------. 2 polkitd input    18 Dec 13 01:26 pg_xact
        -rw-------. 1 polkitd input    88 Dec 13 01:26 postgresql.auto.conf
        -rw-------. 1 polkitd input 29525 Dec 13 01:26 postgresql.conf
        -rw-------. 1 polkitd input    36 Dec 13 01:26 postmaster.opts
        -rw-------. 1 polkitd input    94 Dec 13 01:26 postmaster.pid
        [root@mck-network-test postgres]#



## 4.    развернуть контейнер с клиентом postgres

        [root@mck-network-test bin]# docker ps -a
        CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                  PORTS                                       NAMES
        f83b90224e56   postgres:15   "docker-entrypoint.s…"   3 minutes ago    Up 3 minutes            5432/tcp                                    pg-client
        23676e74f508   postgres:15   "docker-entrypoint.s…"   20 minutes ago   Up 20 minutes           0.0.0.0:5433->5432/tcp, :::5433->5432/tcp   pg-server
        2fce7f112590   hello-world   "/hello"                 8 days ago       Exited (0) 8 days ago                                               cranky_mayer
        [root@mck-network-test bin]#


## 5.    подключится из контейнера с клиентом к контейнеру с сервером и сделать таблицу с парой строк

        [root@mck-network-test postgres]# docker run -it --rm --network pg-net --name pg-client postgres:15 psql -h pg-server -U postgres
        Password for user postgres:
        psql (15.5 (Debian 15.5-1.pgdg120+1))
        Type "help" for help.
        
        postgres=#



        postgres=#
        postgres=# create table persons(id serial, first_name text, second_name text); insert into persons(first_name, second_name) values('ivan', 'ivanov'); insert into persons(first_name, second_name) values('petr', 'petrov'); commit;
        CREATE TABLE
        INSERT 0 1
        INSERT 0 1
        WARNING:  there is no transaction in progress
        COMMIT
        postgres=#
        postgres=# select * from persons;
         id | first_name | second_name
        ----+------------+-------------
          1 | ivan       | ivanov
          2 | petr       | petrov
        (2 rows)
        
        postgres=#




## 6.    подключится к контейнеру с сервером с ноутбука/компьютера извне инстансов GCP/ЯО/места установки докера

Подключился Из PgAdmin и убедился что есть созданная таблица


![PgAdmin](https://github.com/olegrovenskiy/otus-pgsql-hw-lesson-3/blob/main/1.png)


## 7.    удалить контейнер с сервером

        [root@mck-network-test ~]# docker ps -a
        CONTAINER ID   IMAGE         COMMAND                  CREATED             STATUS                  PORTS                                                  NAMES
        f83b90224e56   postgres:15   "docker-entrypoint.s…"   47 minutes ago      Up 47 minutes           5432/tcp                                               pg-client
        23676e74f508   postgres:15   "docker-entrypoint.s…"   About an hour ago   Up About an hour        0.0.0.0:5433->5432/t           cp, :::5433->5432/tcp   pg-server
        2fce7f112590   hello-world   "/hello"                 8 days ago          Exited (0) 8 days ago                                                          cranky_mayer
        [root@mck-network-test ~]#
        [root@mck-network-test ~]#
        [root@mck-network-test ~]#
        [root@mck-network-test ~]# docker stop 23676e74f508
        23676e74f508
        [root@mck-network-test ~]# docker ps -a
        CONTAINER ID   IMAGE         COMMAND                  CREATED             STATUS                     PORTS      NAMES
        f83b90224e56   postgres:15   "docker-entrypoint.s…"   49 minutes ago      Up 49 minutes              5432/tcp   pg-cli           ent
        23676e74f508   postgres:15   "docker-entrypoint.s…"   About an hour ago   Exited (0) 3 seconds ago              pg-ser           ver
        2fce7f112590   hello-world   "/hello"                 8 days ago          Exited (0) 8 days ago                 cranky           _mayer
        [root@mck-network-test ~]# docker rm 23676e74f508
        23676e74f508
        [root@mck-network-test ~]#
        [root@mck-network-test ~]# docker ps -a
        CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                  PORTS      NAMES
        f83b90224e56   postgres:15   "docker-entrypoint.s…"   49 minutes ago   Up 49 minutes           5432/tcp   pg-client
        2fce7f112590   hello-world   "/hello"                 8 days ago       Exited (0) 8 days ago              cranky_mayer
        [root@mck-network-test ~]#


postgres=# select * from persons;
FATAL:  terminating connection due to administrator command
server closed the connection unexpectedly
        This probably means the server terminated abnormally
        before or while processing the request.
The connection to the server was lost. Attempting reset: Failed.
The connection to the server was lost. Attempting reset: Failed.
!?>
!?>
!?>




## 8.    создать его заново

        [root@mck-network-test ~]# docker ps -a
        CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                  PORTS                                       NAMES
        bc417b9b3103   postgres:15   "docker-entrypoint.s…"   4 seconds ago    Up 4 seconds            0.0.0.0:5433->5432/tcp, :::5433->5432/tcp   pg-server
        f83b90224e56   postgres:15   "docker-entrypoint.s…"   52 minutes ago   Up 52 minutes           5432/tcp                                    pg-client
        2fce7f112590   hello-world   "/hello"                 8 days ago       Exited (0) 8 days ago                                               cranky_mayer
        [root@mck-network-test ~]#





## 8.    подключится снова из контейнера с клиентом к контейнеру с сервером

        [root@mck-network-test postgres]# docker run -it --rm --network pg-net --name pg-client postgres:15 psql -h pg-server -U postgres
        Password for user postgres:
        psql (15.5 (Debian 15.5-1.pgdg120+1))
        Type "help" for help.
        
        postgres=#

## 9.    проверить, что данные остались на месте

        postgres=# select * from persons;
         id | first_name | second_name
        ----+------------+-------------
          1 | ivan       | ivanov
          2 | petr       | petrov
        (2 rows)
        
        postgres=#


## 10.    оставляйте в ЛК ДЗ комментарии что и как вы делали и как боролись с проблемами

Задание делал на базе полученных коман с занятий, всё прошло штатно

