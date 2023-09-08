---
title: "Can You Run Docker Containers With Systemd?"
date: 2023-08-17T18:12:55-05:00
draft: true
---

A few months ago I wrote an [article about Systemd Unit files](https://earthly.dev/blog/systemd/). While doing research for that article I came across a few different people talking about using Systemd to manage Docker containers, including this [youtube video](https://www.youtube.com/watch?v=8w8Y4GSEunA&t=306s)

Having just learned about Systemd and all it could do, I was initially intrigued. At the time I was working on a [home server project]() and I thought maybe 


```toml
[Unit]
Description=plex
After=docker.service
Requires=docker.service

[Service]
TimeoutStartSec=0
Restart=always
ExecStartPre=-/usr/bin/docker stop %n
ExecStartPre=-/usr/bin/docker rm %n
ExecStartPre=/usr/bin/docker pull plexinc/pms-docker
ExecStart=/usr/bin/docker run \
    -d \
    --name plex \
    --network=host \
    -e TZ="UTC-5" \
    -e PLEX_CLAIM="claim-XXXXXXXXXX" \
    -v /plex/plex/database>:/config \
    -v /plex/transcode/:/transcode \
    -v /plex/media:/data \
    plexinc/pms-docker

[Install]
WantedBy=multi-user.target
```

[A youtube video about using systemd to start Docker containers](https://www.youtube.com/watch?v=8w8Y4GSEunA&t=306s)

[Configure Systemd to run the docker daemon](https://docs.docker.com/config/daemon/systemd/)

More about using systemd with docker https://www.redhat.com/sysadmin/container-systemd-persist-reboot

Reddit thread on this subject https://www.reddit.com/r/devops/comments/yqj70x/need_opinion_why_running_container_within_systemd/