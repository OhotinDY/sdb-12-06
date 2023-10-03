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

![sql](https://github.com/OhotinDY/sdb-12-06/blob/main/sql4.png)

Запуск и проверка статуса mysql:

![sql](https://github.com/OhotinDY/sdb-12-06/blob/main/sql3.png)

Выполнение входа в mysql

```
mysql -u root -p
```

Создание пользователя для репликации

```
CREATE USER 'replica_user'@'%' IDENTIFIED WITH mysql_native_password BY 'REPpas';
```

Предоставление пользователю минимально необходимых привелегий

```
GRANT REPLICATION SLAVE ON *.* TO 'replica_user'@'%';
```

Обновление загруженных в память данных о привилегиях 

```
FLUSH PRIVILEGES;
```

Запуск команды, которая возвращает информацию о текущем состоянии бинарных файлов логов для source

```
SHOW MASTER STATUS;
```

![sql](https://github.com/OhotinDY/sdb-12-06/blob/main/sql5.png)

Запуск команды с настройками репликации

```
CHANGE REPLICATION SOURCE TO
SOURCE_HOST='10.129.0.13',
SOURCE_USER='replica_user',
SOURCE_PASSWORD='pass',
SOURCE_LOG_FILE='binlog.000002',
SOURCE_LOG_POS=1034;
```

Запуск команды для активации репликации

```
START REPLICA;
```

![sql](https://github.com/OhotinDY/sdb-12-06/blob/main/sql6.png)
