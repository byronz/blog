---
title: "Docker"
date: 2018-01-18T22:48:39-05:00

tags: [container, docker]
---

## Namespaces

- CGROUP
- IPC
- NET
- NS (MNT)
- PID
- USER
- UTS

### PID 

```
$ ps axul | sort -n -k 2 | head -n 10                                                                                                                              
USER               PID  %CPU %MEM      VSZ    RSS   TT  STAT STARTED      TIME COMMAND            UID  PPID CPU PRI NI WCHAN
root                 1   0.0  0.1  4372056  12232   ??  Ss   15Jan18  54:02.49 /sbin/launchd        0     0   0  37  0 -
root                38   0.0  0.0  4351128   1756   ??  Ss   15Jan18   0:49.25 /usr/sbin/syslog     0     1   0   4  0 -
root                39   0.0  0.0  4382588   6952   ??  Ss   15Jan18   0:52.69 /usr/libexec/Use     0     1   0  37  0 -
root                41   0.0  0.0  4325236    488   ??  Ss   15Jan18   0:17.96 /System/Library/     0     1   0  20  0 -
root                42   0.0  0.1  4398964  17132   ??  Ss   15Jan18   0:41.24 /usr/libexec/kex     0     1   0  37  0 -
root                43   0.0  0.0  5274696   6592   ??  Ss   15Jan18   2:17.38 /System/Library/     0     1   0  50  0 -
root                45   0.0  0.0  4378132   5136   ??  Ss   15Jan18   0:10.03 /System/Library/     0     1   0   4  0 -
_appleevents        47   0.0  0.0  4377940   2676   ??  Ss   15Jan18   0:03.84 /System/Library/    55     1   0   4  0 -
root                48   0.0  0.0  4381824   4440   ??  Ss   15Jan18   1:14.73 /usr/sbin/system     0     1   0   4  0 -
```



### Net

![diagram](https://img.draveness.me/2017-11-30-docker-network.png)
![bridge](https://img.draveness.me/2017-11-30-docker-network-topology.png)

use iptables
![forward](https://img.draveness.me/2017-11-30-docker-network-forward.png)

https://github.com/docker/libnetwork/blob/master/docs/design.md

###  NS (MNT)

`rootfs` ![mnt](https://img.draveness.me/2017-11-30-libcontainer-filesystem.png)

### CGROUP

linux implementation of sharing hardware and I/O 

### UTS
```
FROM ubuntu:15.04
COPY . /app
RUN make /app
CMD python /app/app.py
```
![dockelayer](https://img.draveness.me/2017-11-30-docker-container-layer.png)

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



