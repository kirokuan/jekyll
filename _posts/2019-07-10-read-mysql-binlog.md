---
layout: post
title: "Read/Analysis from binlog of MySQL"
date: 2019-07-10 00:00:00
tags: mysql binlog
description: Analysis of table transactions from binlog
---

use the native tool from mysql `mysqlbinlog`.
To print normal log, just 

``` 
# SQL: list all binlog physical files.
SHOW BINARY LOGS; 
```

```
mysqlbinlog -R --host <host_ip> --user <user> --password  --base64-output=decode-rows -vv <filename> 
# -vv is verbose
```
redirect the output to local file to further analysis. but the file is quite big.

```
cat log | grep -i -e "###\sUPDATE" -e "INSERT" -e "DELETE" |cut -c1-100| tr '[A-Z]' '[a-z]' | sed -e "s/\t/ /g;s/\`//g;s/(.*$//;s/ set .*$//;s/ as .*$//" | sed -e "s/ where .*$//" | sort | uniq -c | sort -nr | head -30
```

this output the data like

```
 471440 ### delete from table_a
  27274 ### update table_b
   6804 ### update table_c
   1303 ### update table_d
   1051 ### insert into table_a
    418 ### insert into table_e
    ...
```

the count here is not the number of transactions, but the number of infected count.
So if you run a statement of delete from table_a, the 10 rows are deleted so the 10 is added that count of `delete from table_a`


