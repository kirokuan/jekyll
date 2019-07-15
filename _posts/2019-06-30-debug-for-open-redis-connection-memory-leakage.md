---
layout: post
title: "Debug why redis connection increased and memory leakage in Redis  "
date: 2019-06-30 00:00:00
tags: GCP redis memory-leakage netstat
description: 
---

Recently, we migrated the redis from other 3rd-party to memorystore. 

After the migration, the memory kept growing until we restarted the servers(kill the pods).
And the connection count keep increasing as well.
Since there is multiple processes in our container, it's not easy to identify which process cause this problem.
The other problem I encountered is that there is no netstat in our container, in the node there are multuple containers running concurrently.I used some redis command to identify the problem is not about dataset storage.

It's about the client-ouput-buffer. In that case, if we hosted Redis by ourselves, we can just configure it to close the connection when it come to hard-limit, the connection will be closed forcefully.

Finally I went to the node, I use

{% highlight bash %}

docker ps | grep "server" # the name of our container to find the id
docker inspect <id> #find the pid the docker use
nsenter -t <pid> -n # switch to the c-group I need
# start to use netstat -ap to identify the problem..

{% endhighlight %}


After listing all connections. I found something that the connection different from those of normal connection is `RECV-Q`

So my speculation is that our container create a pubsub but never unsubscribe it, so those connection remains and whenever the server publish new message to it. The message is left in RECV-Q, and that's why the output-limit is enlarged graduately.

The difference between the 3rd-party and memorystore may be that memorystore may set unlimited for client-ouput-buffer pubsub while the default value of client-ouput-buffer pubsub is not unlimited.