---
layout: post
title: "Set up the read-replica of gcloud sql"
date: 2019-08-13 00:00:00
tags: mysql replica gcloud cloudsql
description: Setup a read replica in external server.
---


To set up a external read-replica of Gcloud instance.

First of all, install the mysql instance in compute instance.

I tried to syncronize a brand new instance in cloudsql without any full backup.
This doesn't work, since the slave read the transaction to modify mysql.user table, which is not permissible. I couldn't modify `Executed_Gtid_Set` to skip the transaction either. I also tried recover the slave instance with `mysql` table, and all credentials were broken.

Therefore, setting up slave with full backup without credentials and other system info of master is necessary.

```

mysqldump --databases [DATABASE_NAME1, DATABASE_NAME2, ...] -h [INSTANCE_IP] -u [USERNAME] -p \
--master-data=1 --flush-privileges --hex-blob --skip-triggers --ignore-table [VIEW_NAME1] [...] \
--default-character-set=utf8mb4 > [SQL_FILE].sql

```

only list the database which app used. Because we use master to setup backup, so we need `--master-data=1`, if we use slave then should use `--slave-data=1`. --ignore-table is not necessary if there is no view. By default, the backup will contains Gtid which the slave use to identify the transaction.

Recover the slave db instance:

```

mysql --user=root --password <  mysqldump.sql

```

configure the instance to be read-only and use gtid

```
[mysqld]
server-id=[SERVER_ID]
gtid_mode=ON
enforce_gtid_consistency=ON
log_slave_updates=ON
replicate-ignore-db=mysql
binlog-format=ROW
log_bin=mysql-bin
expire_logs_days=1
read_only=ON
```

configure master info.

```

CHANGE MASTER TO MASTER_HOST='[MASTER_IP_ADDRESS]', MASTER_USER='[REPLICATION_USER]',
MASTER_PASSWORD='[REPLICATION_PASSWORD]', MASTER_AUTO_POSITION=1;

```

with `MASTER_AUTO_POSITION=1`, the slave can identify the transaction sequence.