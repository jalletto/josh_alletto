---
title: "Plex Setup"
date: 2023-08-28T11:10:34-05:00
draft: true
---

Until recently, I had been running my Plex server on an old laptop. This had several downsides. One, I often wanted to use the laptop, but could not because it was sitting up on a shelf connected to ethernet and an external hard drive. Two, sometimes I wanted to use the laptop to download legal torrents of linux distributions and only Linux distributions. For personal reasons that are not your business, I preferred that these distros where on the same machine as the plex. But whenever I wanted to torrent something, I would turn on my VPN and this made the Plex unusable.

So I needed a better solution. I had two criteria.

1. The new solution needed to be cheap. I wasn't looking to buy an expensive new computer. Running the Plex isn't that demanding of a job, so I knew I could get away with low powered machine.

2. I wanted to be able to download torrents on the same machine, over VPN, without interrupting the Plex usage.

These two criteria had two simple solutions.

1. I would use an old used mini pc as a dedicated server, freeing my laptop.
2. I would run everything in docker, which would allow me to download torrents with my VPN without interfering with my Plex set up (More on this in a bit).

I watched a lot of youtube videos and read a lot of message boards to figure out a solution that worked for me. Here's what a I learned and how my setup ended up looking in the end.

## A $70 Computer

This is an area where I know very little. I've never built a PC and my hardware knowledge is embarrising. But this was also a great opportunity to start learning.

I've seen a bunch of videos in the last year with titles like "Forget Raspberry Pi", "Raspberry Pi is dead" etc. These videos often turn you toward using mini pcs instead. I had tried once, briefly, to set up a Plex server on an old raspberry pi, but I didn't get too far. One problem was that I just found using the Pi to be too slow. It was an older model, and I had raspbian on it, and it just seemed to take forever to do anything. I'm sure the new ones are better, but, since I was hoping for something that could run possibly several applications at once, I wanted something a bit more powerful and a bit more customizable.

I landed on an HP EliteDesk 800 G3 35W Mini PC. It's got an Intel i5-6500T 2.5Ghz and it came with 8gb of RAM. I chose this mainly because of [this](youtube.com) video, and also because it was small and the price was right. I found one for $70 on ebay that came with the power adapter and nothing else. This was fine. I keep all my Plex media on an external hard drive, so I just added a a 500GB SSD ($40).

A quick note on this: I had issues getting the machine to recognize the my M.2 SSD, which was frustrating. At first I thought maybe I'd bought a falty drive, or that it just wasn't compatible. But thankfully I found and answer on [this message board](https://forum.manjaro.org/t/can-t-see-new-m-2-ssd/34035) which suggested turning off all of the hp security settings in the BIOS. After I did this, the drive worked perfectly and I was able to install Linux without any issue. Were these security settings important? I'll let you know if I ever find out.

Eventually, I upgraded the RAM to 32gb. This turned out to be completely unessesary. Why did I do this? The short answer is, inexperience. I had noticed that watching videos worked fine on with 8gigs of RAM, but it often took close to a minute for the video to load and start initially, even on my local network. I assumed, based on nothing, that more RAM would help, but it did not, and truthfully the machine never uses more that 2 gigs, even when downloading serveral large torrent files at once. If I had this to do again, I'd stick with the 8gigs it came with. MAybe I'd buy an additional 8gig stick just to be safe, as I do intend on spinning up more docker serverices in the future.

## Docker Networking With a VPN

I'm using Ubuntu. I know that's not super cool but it's what I'm familiar with and it works; so there. Probably the most interesting part of my set up, to me anyway, was that I got to learn a lot more about Docker Networking. In particular, that you can set a containers network to be another container, which is pretty kick ass.

So there are two parts to my setup. First, the Plex. Luckily, Plex has an [official docker image](https://hub.docker.com/r/plexinc/pms-docker/), so setting it up was extremely simple. For now I have the Plex using my host network. I've seen a lot people saying you should never use the host network, some saying there's only a few instance when you should use it. From what I can tell, the biggest reason is to avoid conflicts with other services that might need to use the same ports. Since I don't have anything else running at the moment, this was just the fastest and easiest way to get up and running quickly, so that's what I'm using right now.

The other part of my setup is the VPN + Torrenting part. I tried a few different things here and a few different setups. Eventually I found [gluetun](https://hub.docker.com/r/qmcgaw/gluetun). After that everything was really easy to set up. The only issues I had were with Nord VPN. You can't use your normal username and password in this case, which is probably a good thing. Instead you need to craete a special service token and password. This wasn't super clear on their site. In fact, they suprising didn't have much about running their VPN in a docker contianer. Either way, if you're in the same boat, you can easily create the token. In Services > NordVPN scroll all the way to the bottom to the section Manual setup. Here you can click on Setup NordVPN Manually. Here you can create a new token and password that will allow you to log into NordVPN via the Gluetun docker image. There's a great tutorial embedded on the github page that helped me get setup quickly.