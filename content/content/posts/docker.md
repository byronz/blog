---
title: "Docker Network"
date: 2018-01-18T22:48:39-05:00
draft: true
tags: [container, network]
---

## Host 

`docker run –d –-name nginx-1 –net=host nginx`


![host](https://mesosphere.com/wp-content/uploads/2017/03/networking-docker-containers-1-v2.png)

pros:
    
- no extra cost on NAT (Network Address Translation)
- no special config or maintenance

cons:

- ports conflict
- share host network namespace

## Bridge (default mode)

```
docker run –d –-name nginx-1 -p 10000:80 nginx
docker run –d –-name nginx-2 -p 10001:80 nginx
```
virtual router is done via iptable
![bridge](https://mesosphere.com/wp-content/uploads/2017/03/networking-docker-containers-2.png)

## Virtual Network 