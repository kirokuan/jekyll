---
layout: post
title: "Find out unused stored-procedures"
date: 2017-1-30 00:00:00
tags: sql stored-procedures
description: 
---

Since we need to find out some old stored procedures what is unused any more, we can use "dm_exec_procedure_stats"

{% highlight sql %}

SELECT sc.name, p.name 
FROM sys.procedures AS p
INNER JOIN sys.schemas AS sc
  ON p.[schema_id] = sc.[schema_id]
LEFT OUTER JOIN sys.dm_exec_procedure_stats AS st
  ON p.[object_id] = st.[object_id]
WHERE st.[object_id] IS NULL
ORDER BY p.name;
        
{% endhighlight %}

and with sys.object, we can find what's unused:

{% highlight sql %}

SELECT name
FROM  sys.objects -- consider schemas
where type_desc='SQL_STORED_PROCEDURE'
and name not in (SELECT p.name 
FROM sys.procedures AS p
INNER JOIN sys.schemas AS sc
  ON p.[schema_id] = sc.[schema_id]
LEFT OUTER JOIN sys.dm_exec_procedure_stats AS st
  ON p.[object_id] = st.[object_id]
WHERE st.[object_id] IS NULL
) 


{% endhighlight %}