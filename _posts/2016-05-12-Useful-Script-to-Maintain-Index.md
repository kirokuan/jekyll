---
layout: post
title: "Useful Script to Maintain Index"
date: 2016-05-12 00:04:06
tags: mssql sql index
description: maintaining index 
---

I added lot of indexes to database for weeks, I noticed that the time to update index for some of query may account for great part. 

{% highlight SQL %}
SELECT OBJECT_NAME(S.[OBJECT_ID]) AS [OBJECT NAME], 
       I.[NAME] AS [INDEX NAME], 
       USER_SEEKS, 
       USER_SCANS, 
       USER_LOOKUPS, 
       USER_UPDATES 
FROM   SYS.DM_DB_INDEX_USAGE_STATS AS S 
       INNER JOIN SYS.INDEXES AS I ON I.[OBJECT_ID] = S.[OBJECT_ID] AND I.INDEX_ID = S.INDEX_ID 
WHERE  OBJECTPROPERTY(S.[OBJECT_ID],'IsUserTable') = 1
       AND S.database_id = DB_ID()
{% endhighlight %}
 

it lists all the index for the database, and for the indexes that seek, scans, lookup are 0, probably means that it is never used. 

The best cases for indexes is that the seeks is used most , then scans and lookup is used least.

{% highlight SQL %}
SELECT OBJECT_NAME(OBJECT_ID), index_id,index_type_desc,index_level,
avg_fragmentation_in_percent,avg_page_space_used_in_percent,page_count
FROM sys.dm_db_index_physical_stats
(DB_ID(N'DatabaseName'), NULL, NULL, NULL , 'SAMPLED')
ORDER BY avg_fragmentation_in_percent DESC
{% endhighlight %}

The sql can look up the whose fragmentation is high, but  it seemed that fragmentation doesn't matter so much.
So usually I only build the indexes regularly (maybe monthly), rather than rebuild them to improve the performance.
 