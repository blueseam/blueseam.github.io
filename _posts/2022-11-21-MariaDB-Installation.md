---
layout: post
title:  "MariaDB Installation & Replication "
categories: [ blueseam, tutorial ]
image: assets/images/mariadb.png
---

# MariaDB Installation & Replication Notes


### 1. MariaDB - Replication
##### 1.1 Preferences information
```
Host : mariadb01(Master), mariadb02(Slave)
IP : 172.16.25.100, 172.16.25.101
GW : 172.16.25.2
DNS : 8.8.8.8
```

##### 1.2 System update(dnf update)
```
# dnf makecache

# dnf -y update

# dnf install -y epel-release
```

##### 1.3 Firewall disable
```
# systemctl stop firewalld && systemctl disable firewalld
```

### 2. Mariadb repository setup
```
# curl -LsS https://r.mariadb.com/downloads/mariadb_repo_setup | sudo bash
```

### 3. Mariadb installation
```
# yum list mariadb*

# yum install -y MariaDB-server MariaDB-client MariaDB-backup

# systemctl start mariadb

# systemctl enable mariadb

# systemctl status mariadb
```

### 4. Mariadb configuration
```
# mariadb-secure-installation 

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] 
Enabled successfully!
Reloading privilege tables..
 ... Success!


You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] 
New password: Passw0rd 
Re-enter new password: Passw0rd
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] 
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] 
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] 
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] 
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```



```
# mysql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 15
Server version: 10.9.3-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> use mysql;

MariaDB [mysql]> grant all privileges on*.* to root@'%' identified by 'Passw0rd';
Query OK, 0 rows affected (0.009 sec)

MariaDB [mysql]> flush privileges;
Query OK, 0 rows affected (0.002 sec)

# vi /etc/my.cnf.d/client.cnf
…
[client]
# default-character-set=utf8
…

# vi /etc/my.cnf.d/mysql-clients.cnf
…
[mysql]
# default-character-set=utf8
…
[mysqldump]
# default-character-set=utf8
…
# vi /etc/my.cnf.d/server.cnf
…
[mysqld]
# default-character-set=utf8
init-connect=‘SET NAMES utf8’
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake

lower_case_table_names=1
…

# systemctl restart mariadb

# mysql
[MariaDB [(none)]> show variables like 'c%';
+----------------------------------+----------------------------+
| Variable_name                    | Value                      |
+----------------------------------+----------------------------+
| character_set_client             | utf8mb3                    |
| character_set_connection         | utf8mb3                    |
| character_set_database           | utf8mb3                    |
| character_set_filesystem         | binary                     |
| character_set_results            | utf8mb3                    |
| character_set_server             | utf8mb3                    |
| character_set_system             | utf8mb3                    |
| character_sets_dir               | /usr/share/mysql/charsets/ |
| check_constraint_checks          | ON                         |
| collation_connection             | utf8mb3_unicode_ci         |
| collation_database               | utf8mb3_unicode_ci         |
| collation_server                 | utf8mb3_unicode_ci         |
| column_compression_threshold     | 100                        |
| column_compression_zlib_level    | 6                          |
| column_compression_zlib_strategy | DEFAULT_STRATEGY           |
| column_compression_zlib_wrap     | OFF                        |
| completion_type                  | NO_CHAIN                   |
| concurrent_insert                | AUTO                       |
| connect_timeout                  | 10                         |
| core_file                        | OFF                        |
+----------------------------------+----------------------------+
```

### 5. Replication configuration
```
[mariadb01] # mysql
…
MariaDB [mysql]> grant replication slave on *.* to repl_user@'%' identified by 'Passw0rd';

MariaDB [mysql]> flush privileges;
…


[mariadb01] # vi /etc/my.cnf.d/server.cnf
…
[mysqld]
…
# Replication
log-bin=mysql-bin
server-id=100
binlog_format=MIXED
max_binlog_size=100M
expire_logs_days=30
…

[root@mariadb01 ~]# systemctl restart mariadb

[root@mariadb01 ~]# mysql
…
MariaDB [(none)]> show variables like 'server_id';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| server_id     | 100   |
+---------------+-------+
1 row in set (0.002 sec)

MariaDB [(none)]> show master status;
+------------------+----------+--------------+------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+------------------+----------+--------------+------------------+
| mysql-bin.000001 |      328 |              |                  |
+------------------+----------+--------------+------------------+
1 row in set (0.001 sec)

MariaDB [(none)]> show variables like '%log_bin%';
+---------------------------------+--------------------------------+
| Variable_name                   | Value                          |
+---------------------------------+--------------------------------+
| log_bin                         | ON                             |
| log_bin_basename                | /var/lib/mysql/mysql-bin       |
| log_bin_compress                | OFF                            |
| log_bin_compress_min_len        | 256                            |
| log_bin_index                   | /var/lib/mysql/mysql-bin.index |
| log_bin_trust_function_creators | OFF                            |
| sql_log_bin                     | ON                             |
+---------------------------------+--------------------------------+
…

[root@mariadb01 ~]# cd /var/lib/mysql
[root@mariadb01 mysql]# ls -al
합계 123340
drwxr-xr-x.  5 mysql mysql      4096 10월 25 15:15 .
drwxr-xr-x. 23 root  root       4096 10월 25 11:42 ..
-rw-rw----.  1 mysql mysql    417792 10월 25 15:15 aria_log.00000001
-rw-rw----.  1 mysql mysql        52 10월 25 15:15 aria_log_control
-rw-rw----.  1 mysql mysql         9 10월 25 15:15 ddl_recovery.log
-rw-rw----.  1 mysql mysql       946 10월 25 15:15 ib_buffer_pool
-rw-rw----.  1 mysql mysql 100663296 10월 25 12:44 ib_logfile0
-rw-rw----.  1 mysql mysql  12582912 10월 25 12:44 ibdata1
-rw-rw----.  1 mysql mysql  12582912 10월 25 15:15 ibtmp1
-rw-rw----.  1 mysql mysql         6 10월 25 15:15 mariadb01.pid
-rw-rw----.  1 mysql mysql         0 10월 25 11:43 multi-master.info
drwx------.  2 mysql mysql      4096 10월 25 11:42 mysql
-rw-rw----.  1 mysql mysql       328 10월 25 15:15 mysql-bin.000001 
-rw-rw----.  1 mysql mysql        19 10월 25 15:15 mysql-bin.index
srwxrwxrwx.  1 mysql mysql         0 10월 25 15:15 mysql.sock
-rw-r--r--.  1 mysql mysql        14 10월 25 11:42 mysql_upgrade_info
drwx------.  2 mysql mysql        20 10월 25 11:42 performance_schema
drwx------.  2 mysql mysql      8192 10월 25 11:42 sys



[mariadb02] # vi /etc/my.cnf.d/server.cnf
…
[mysqld]
…
# Replication
server-id=101
slave-skip-errors=all
read_only=1
…

[root@mariadb02 ~]# systemctl restart mariadb

[root@mariadb02 ~]# mysql
…
MariaDB [(none)]> show variables like 'server_id';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| server_id     | 101   |
+---------------+-------+
1 row in set (0.002 sec)

MariaDB [(none)]> change master to master_host='172.16.25.100',
    -> master_user='repl_user',
    -> master_password='Passw0rd',
    -> master_port=3306,
    -> master_log_file='mysql-bin.000001',
    -> master_log_pos=328,
    -> master_connect_retry=10;
Query OK, 0 rows affected (0.010 sec)

MariaDB [(none)]> start slave;
Query OK, 0 rows affected (0.001 sec)

MariaDB [(none)]> show slave status\G;
*************************** 1. row ***************************
                Slave_IO_State: Waiting for master to send event
                   Master_Host: 172.16.25.100
                   Master_User: repl_user
                   Master_Port: 3306
                 Connect_Retry: 10
               Master_Log_File: mysql-bin.000001
           Read_Master_Log_Pos: 328
                Relay_Log_File: mariadb02-relay-bin.000002
                 Relay_Log_Pos: 555
         Relay_Master_Log_File: mysql-bin.000001
              Slave_IO_Running: Yes
             Slave_SQL_Running: Yes
               Replicate_Do_DB: 
           Replicate_Ignore_DB: 
            Replicate_Do_Table: 
        Replicate_Ignore_Table: 
       Replicate_Wild_Do_Table: 
   Replicate_Wild_Ignore_Table: 
                    Last_Errno: 0
                    Last_Error: 
                  Skip_Counter: 0
           Exec_Master_Log_Pos: 328
               Relay_Log_Space: 868
               Until_Condition: None
                Until_Log_File: 
                 Until_Log_Pos: 0
            Master_SSL_Allowed: No
            Master_SSL_CA_File: 
            Master_SSL_CA_Path: 
               Master_SSL_Cert: 
             Master_SSL_Cipher: 
                Master_SSL_Key: 
         Seconds_Behind_Master: 0
 Master_SSL_Verify_Server_Cert: No
                 Last_IO_Errno: 0
                 Last_IO_Error: 
                Last_SQL_Errno: 0
                Last_SQL_Error: 
   Replicate_Ignore_Server_Ids: 
              Master_Server_Id: 100
                Master_SSL_Crl: 
            Master_SSL_Crlpath: 
                    Using_Gtid: No
                   Gtid_IO_Pos: 
       Replicate_Do_Domain_Ids: 
   Replicate_Ignore_Domain_Ids: 
                 Parallel_Mode: optimistic
                     SQL_Delay: 0
           SQL_Remaining_Delay: NULL
       Slave_SQL_Running_State: Slave has read all relay log; waiting for more updates
              Slave_DDL_Groups: 0
Slave_Non_Transactional_Groups: 0
    Slave_Transactional_Groups: 0
1 row in set (0.000 sec)
```




### Master status check
```
[root@mariadb01 mysql]# mysql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 8
Server version: 10.9.3-MariaDB-log MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show processlist\G;
…
*************************** 3. row ***************************
      Id: 7
    User: repl_user
    Host: 172.16.25.101:56422
      db: NULL
 Command: Binlog Dump
    Time: 203
   State: Master has sent all binlog to slave; waiting for more updates
    Info: NULL
Progress: 0.000
…
4 rows in set (0.001 sec)
```

