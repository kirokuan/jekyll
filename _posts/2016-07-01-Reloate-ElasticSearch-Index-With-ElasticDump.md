---
layout: post
title: "Reloating ElasticSearch Index, Type with ElasticDump"
date: 2016-07-01 00:04:06
tags: ElasticSearch ElasticDump Powershell
description: Using ElasticSearch to move index 
---

Since I needed to modify some type in indexes. If I don't re-create the index , all the data type remained old.

So I use elasticdump to deal with the reindex problem. Installation is quite easy as their website indicated.

{% highlight bash %}
    npm install elasticdump -g
{% endhighlight %}

To Use it, at least 3 parameters required: --input=, --output= and --type=.

{% highlight bash %}
    elasticdump --input=http://localhost:9200/index_old/  --output=http://localhost:9200/index_new/ --type=data --limit=10000 
{% endhighlight %}

limit is how many data can be operate once, the less it is, the slower it move, but the greater memory it used.
If the command fail, we can try to resume it with *--offset=1000*, use the number it finally printed. 

Today I encountered another problem I need to rename my type. It seemed elasticdump doesn't support it directly, although you can specify the type in input wuth --input-index=index/type. But when outputing the data, it still uses the same type.
However, it can dump the data to file, and restore to the server. so we can dump to server to replace it and restore the data to the server.

{% highlight bash %}
$ScriptBlock = {
   param($filename,$type) 
        elasticdump --limit=10000 --type=data  --input=http://localhost:9200/ --input-index=my_index/$type --output=$filename
        $newfilename= ($filename+".new")
        $newnew= ($newfilename+".json")
        (cat $filename) -replace "$type",($type+"_new")  > $newfilename
        [IO.File]::WriteAllLines($newnew,( gc $newfilename)) --only for remoing BOM here
        elasticdump  --bulk=true  --input=$newnew --output=http://localhost:9200/my_index
        Remove-Item $newfilename
        Remove-Item $newnew
        Remove-Item $filename  
}
{% endhighlight %}

Here just move "type" to "type_new", and $filename is just a intermediate temporary file. 



