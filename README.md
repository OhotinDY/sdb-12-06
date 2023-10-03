# Домашнее задание к занятию «Репликация и масштабирование. Часть 1»

### Задание 1

На лекции рассматривались режимы репликации master-slave, master-master, опишите их различия.

*Ответить в свободной форме.*

---

Ответ:
- В случае репликации в режиме master-slave данные с сервера-источника реплицируются на сервер-реплику в одном направлении.
- В случае репликации в режиме master-master оба сервера являются одновременно источником и репликой, т.е изменения в данных на одном источнике будут реплицированы в другой и наоборот.

### Задание 2

Выполните конфигурацию master-slave репликации, примером можно пользоваться из лекции.

*Приложите скриншоты конфигурации, выполнения работы: состояния и режимы работы серверов.*

---

Создание ВМ в Yandex Cloude:

![sql](https://github.com/OhotinDY/sdb-12-06/blob/main/sql1.png)

![sql](https://github.com/OhotinDY/sdb-12-06/blob/main/sql2.png)

Конфигурационный файл: 

```
nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

```
[mysqld]
pid-file	= /var/run/mysqld/mysqld.pid
socket		= /var/run/mysqld/mysqld.sock
datadir		= /var/lib/mysql
log-error	= /var/log/mysql/error.log

bind-address	= 0.0.0.0
server-id	= 1
log_bin		= /var/log/mysql/mysql-bin.log
```
