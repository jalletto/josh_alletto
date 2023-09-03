---
title: "Plexing and Torrenting on the Same Server: Let's Docker Network"
date: 2023-08-28T11:10:34-05:00
draft: true
---

One Christmas long ago my brother and I got a Sega Genesis. We were thrilled. But as soon as all the jumping around with joy had ended, my first impulse was not to run to the TV to set it up. What I really wanted to do was call my best friend Tom so I could tell him about it, because talking about a new toy is sometimes just as fun as playing with it. But my mom didn't want me calling and disturbing his family on a holiday, so I'd have to wait a hundred thousand years (24hrs) to tell him about it.

But as it turned out, a Christmas miracle was about to take place, because about an hour later, Tom ended up calling me. He didn't even say hi when I got to the phone. "Josh, guess what, I got a Sega!" he said. We both screamed and then I said, "Tom, guess what, I got one too!" and then more yelling.

All of this is to say that this post isn't a tutorial, it's just me screaming out to anyone who might possibly care, "I set up a new Plex server that also does torrents over VPN!" A bit more of a mouthful than "I got a sega", but for me, just as exciting.

## The Old Way

![Image From Max Headroom (1987)](max.jpg)

Until recently, I had been running my Plex media server on an old laptop. This had two significant downsides. First of all, I often wanted to use the laptop, but could not because it was sitting up on a shelf connected to ethernet and an external hard drive. Second, sometimes I wanted to use the laptop to download legal torrents of Linux distributions and only legal torrents of Linux distributions. For personal reasons that are not your business, I preferred that these distros were on the same machine as the Plex. The problem was that whenever I wanted to torrent something, I needed to turn on my VPN which made the Plex unusable. Quite the bummer when all you want to do while waiting for a torrent to download is watch another episode of Max Headroom.

So I needed a better solution. I had two criteria.

1. The new way needed to be cheap. I wasn't looking to buy an expensive new computer. Running the Plex isn't that demanding of a job, so I knew I could get away with a low-powered machine.

2. I wanted to be able to download torrents on the same machine, over VPN, without interrupting the Plex usage.

These two criteria had two simple solutions.

1. I would use an old used mini PC as a dedicated server, freeing up my laptop.
2. I would run everything in docker, which would allow me to download torrents over my VPN without interfering with my Plex setup (More on this in a bit).

I watched a lot of YouTube videos and read a lot of message boards to figure out a solution that worked for me. Here's what I learned and how my setup ended up looking in the end.

## A $70 Computer

![PI (1998)](pi.png)

First, I needed to choose a new computer. This is an area where I know very little. I've never built a PC and my hardware knowledge is embarrassing, at least when it comes to understanding how much of what hardware you need to run what software. But this was also a great opportunity to start learning.

I had tried once, briefly, to set up a Plex server on an old Raspberry Pi, but I didn't get too far. One problem was that I just found using the Pi to be too slow. It was an older model, and I had Raspbian on it, and it just seemed to take forever to do anything. I'm sure the new ones are better, but, since I was hoping for something that could run several applications at once, I wanted something a bit more powerful and a bit more customizable.

And also cheaper.

![HP EliteDesk 800](hp.jpg)

I landed on an HP EliteDesk 800 G3 35W Mini PC. It's got an Intel i5 and it came with 8 GB of RAM. I chose this mainly because of [this great video](https://www.youtube.com/watch?v=amVP96OYfUg&list=WL&index=12&t=206s), and also because it was small and the price was right. I found one for $70 on eBay that came with the power adapter, 8 gigs of RAM, and nothing else. This was fine. I keep all my Plex media on an external hard drive, so I just added a 500GB SSD for around $40. [^2]

### My $80 Mistake

I wish I could say I set this whole thing up, all in, for $110, but eventually, I ended up upgrading the RAM to 32 GB. This turned out to be completely unnecessary. Why did I do this? Well, I had noticed that watching videos worked fine with 8 GB of RAM, but it sometimes took close to a minute for the video to load, and initially, even on my local network. I assumed, **based on nothing**, that more RAM would help. This didn't turn out to be true. In fact, I think the latency is probably just due to my external drive taking time to spin up. Now that I've been paying more attention, it seems like the machine never uses more than a few gigs, even when downloading several large torrent files at once. If I had this to do again, I'd stick with the 8 GB it came with. Maybe I'd buy an additional 8 just to be safe, as I do intend on spinning up more docker services in the future, but as it stands right now, upgrading to 32 seems to have been massive overkill.

## Docker Networking With a VPN

Probably the most interesting part of my setup, to me anyway, was that I got to learn a lot more about Docker Networking. In particular, that you can set a container's network to be another container, which is pretty kick-ass.

### Plex

So there are two parts to my setup. First, the Plex. Luckily, Plex has an [official docker image](https://hub.docker.com/r/plexinc/pms-docker/), so setting it up was extremely simple. For now, I have the Plex using my host network. I've seen a lot of people saying you should never use the host network. From what I can tell, the biggest reason is to avoid conflicts with other services that might need to use the same ports. Since I don't have anything else running at the moment that conflicts, this was just the fastest and easiest way to get up and running quickly, so that's what I'm using right now. Eventually, I'm going to switch to the Macvlan Networking setup that Plex suggests using, but for now, it is what it is.

### Torrents

The other part of my setup is the VPN + torrenting part. Originally, I had thought maybe I'd have to create my own container, possibly pulling from an existing VPN container or something like that. Later, I figured I could create my own docker network using the VPN, and have the torrent container connect to it.

The actual solution was even easier once I realized you can set a container's network to just be another container. Literally, `docker run --network container:nameOfVPNContainer`. Pretty cool.

I use transmission for torrents. [LinuxServer.io](https://www.linuxserver.io/) has a very popular and well-maintained [image](https://hub.docker.com/r/linuxserver/transmission), making this part a breeze.

I tried running a dedicated NordVPN container, but I had some issues with it that I already forgot about and so can not share with you. What I can tell you is that, eventually, I found [gluetun](https://hub.docker.com/r/qmcgaw/gluetun), and everything just fell into place after that. Gluetun is an all-purpose VPN client. It works with dozens of VPN providers. All you need to do is provide it with some credentials, and Gluetun sets everything else up for you. So far it's been an excellent solution for me. There's also a great getting started video right on the GitHub page.

The only small issue I had was related to NordVPN. **You can't use your normal username and password to log in remotely, which is a good thing.** Instead, you need to create a special service token and password. That's not really an issue. The issue was that it wasn't super clear on their site that you needed to do this, or even how you are supposed to do this [^1]. I had to read about it on Stackoverflow. In fact, NordVPN surprisingly didn't have much about running their VPN in a docker container on the site. The most I could find was this [brief page](https://support.nordvpn.com/Connectivity/Linux/1507838432/How-to-build-the-NordVPN-Docker-image.htm) about running the CLI, but nothing related to what I wanted to do, which was to have other containers be able to connect to the VPN.

Anyway, once all this was set up everything started working exactly as I'd hoped. I could download Legal Linux torrents with ease, over VPN, and watch 90's bullshit Sci-Fi TV at the same time. Dreams really do come true. 

And, bonus, I've freed my laptop, and am currently using it to write this post. I'll let you decide if that's a good thing or not.

Here's my completed Docker Compose file for the torrent + VPN part, if you are interested.

```yml
version: "3.8"

services:
  gluetun:
    container_name: gluetun
    image: qmcgaw/gluetun
    cap_add:
      - NET_ADMIN
    devices:
      - /dev/net/tun:/dev/net/tun
    ports:
      - 9091:9091 # Web UI port
      - 51413:51413 # Torrent port (TCP)
      - 51413:51413/udp # Torrent port (UDP)
    volumes:
      - ~/gluetun:/gluetun
    environment:
      - CITY="Add Some Cities Here"
      - VPN_SERVICE_PROVIDER=nordvpn
      - VPN_TYPE=openvpn
      - OPENVPN_USER=replace with your user
      - OPENVPN_PASSWORD=replace with your password
  transmission:
    image: lscr.io/linuxserver/transmission
    container_name: transmission
    network_mode: container:gluetun
    environment:
      - PUID=1000 # User id
      - PGID=1000 # Group id
      - TZ=GMT-5# Your current timezone
    volumes:
      - /transmission/docker/config/Transmission:/config # Change this to your docker config folder
      - /transmission/downloads:/downloads/complete # Change this to your download folder
    restart: unless-stopped # This makes sure that the application restarts when it crashes
```

## What I Learned

Honestly, what I learned most about setting this all up is that I have a lot to learn. I'd love to do a deep dive into how the Gluetun image works. I don't really understand OpenVPN on any kind of deep level, nor do I have more than a basic understanding of Docker networking. I certainly know more than I did before starting this project, but really it's just sparked my curiosity to learn more.

[^1]: If you're in the same boat, you can easily create the token on the Nord website. In **Services > NordVPN** scroll all the way to the bottom to the section **Manual Setup**. Click on **Setup NordVPN Manually**. Here you can create a new token and password that will allow you to log into NordVPN via the Gluetun docker image.

[^2]: A quick note on this: I had issues getting the machine to recognize my M.2 SSD, which was frustrating. At first, I thought maybe I'd bought a faulty drive, or that it just wasn't compatible. Thankfully I found an answer on [this message board](https://forum.manjaro.org/t/can-t-see-new-m-2-ssd/34035) which suggested turning off all of the HP security settings in the BIOS. After I did this, the drive worked perfectly and I was able to install Linux without any issues. Were these security settings important? I'll let you know if I ever find out.