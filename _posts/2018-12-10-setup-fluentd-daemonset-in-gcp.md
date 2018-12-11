---
layout: post
title: "Set Up Fluentd Daemon Set in GCP"
date: 2018-12-10 00:00:00
tags: GCP kubernetes fleuntd
description: 
---

I built up a bashbaord recently, and still trying to use ELK(ElasticSearch, Logstash, Kibana) stack.

However, I want to change to use Fluentd this time, since LogStash costs much more resource than Fluentd do.

To use fleuntd with ElasticSearch, there is ready docker in [others' reposity](https://github.com/fluent/fluentd-kubernetes-daemonset).

Originally, I think use fluentd as sidecar container is a good idea and easy. With `emptyDir`, we can share the directory between different containers.

But there are some backdraw to use sidecar:

- every pod own its own log collector docker, they can't be shared.
- one of container down in the pod cause the whole pod to restart
- diffult to maintain for different environment, ex: you may not collect log from dev or staging.

So I decided to use independant daemon set to collect log, since I can create it and maintain the pod as small as possible, and even the misconfiguration in log collector I don't need to worry that it take the main application down.

And that's how GKE collect the server data to stackdrive with fluentd. 

{% highlight yaml %}

apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: fluentd
  namespace: kube-system
  labels:
    k8s-app: fluentd-logging
    version: v1
    kubernetes.io/cluster-service: "true"
spec:
  template:
    metadata:
      labels:
        k8s-app: fluentd-logging
        version: v1
        kubernetes.io/cluster-service: "true"
    spec:
      tolerations:
      - key: node-role.kubernetes.io/master
        effect: NoSchedule
      containers:
      - name: fluentd
        image: fluent/fluentd-kubernetes-daemonset:elasticsearch
        env:
          - name:  FLUENT_ELASTICSEARCH_HOST
            value: "elasticsearch-logging"
          - name:  FLUENT_ELASTICSEARCH_PORT
            value: "9200"
          - name: FLUENT_ELASTICSEARCH_SCHEME
            value: "http"

        volumeMounts:
        - name: varlog
          mountPath: /var/log
        - name: varlibdockercontainers
          mountPath: /var/lib/docker/containers
          readOnly: true
      terminationGracePeriodSeconds: 30
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
      - name: varlibdockercontainers
        hostPath:
          path: /var/lib/docker/containers

{% endhighlight %}


The point here is how it shares the directory with other deployment. It use `hostPath`, which is the directory of node. Therefore, in the main application, we also should mount the directory onto it.