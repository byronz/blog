---
title: "Docker Dev"
date: 2019-12-02T22:12:43-05:00
draft: true
---

# commons installs

```
RUN export DEBIAN_FRONTEND=noninteractive && \
    apt update && apt install -y --no-install-recommends \
    sudo curl iputils-ping gcc make build-essential git vim
    openssh-client ca-certificates wget curl sudo gnupg vim iputils-ping && \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false

RUN mkdir -p /root/.ssh && \
    ssh-keygen -A && \
    chmod 0700 /root/.ssh && \
    ssh-keyscan github.com > /root/.ssh/known_hosts
    chmod 600 /root/.ssh/id_rsa

RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN groupadd --gid 999 docker && \
    useradd -u 1000 -g users -G sudo,docker dev

```
