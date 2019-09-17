---
title: "Installing and Configuring: XScreenSaver"
categories:
  - Install
  - Configure
  - X11
  - Screen Saver
  - Linux Mint
  - XFCE
  - Corporate Branding
tags:
  - install
  - configure
  - x11
  - screensaver
  - linux
  - mint
  - xfce
  - corporate
  - branding
---

NOTICE! Everything herein is presented purely as purely hypothetical--a work of fiction.



During your computer's idle time, there are myriad ways of "utilizing" your CPU/GPU, RAM, disk, etc., including:
  - Boinc
  - distributed.net
  - cpushare ?
  - XScreenSaver (i.e., the topic of this post)

InnovAnon, Inc. will be learning about Deep Mind technology in the near future.
AlphaGo is an application of Deep Mind technology.
After watching this YouTube video https://www.youtube.com/watch?v=6rB2cYOeppQ, the author felt inspired to undertake this project.

Following this guide https://www.linux-mint-czech.cz/2015/02/xscreensaver/, we derive this command (n.b., nowadays, we use `apt` insstead of `apt-get`):
sudo apt install libxss-dev libxss1-dbg libxss1 kdelibs-bin kdelibs5-data kdelibs5-plugins unicode-screensaver xscreensaver* xfishtank goban-ss goban-original-games mpv rss-glx

At this point for the author, running the XScreenSaver configuration utility failed to render the GoBan-SS configuration options. To fix this, open in a text editor the ~/.xscreensaver configuration file. Add a line such as:
GL: goban -root -game-dir /usr/share/goban \n\

There's also a warning about not being able to load libgnome.so. That can be mitigated by installing:
sudo apt install libgnomeui-dev

Next, download the AlphaGo *.sgf game data files (the download links are toward the bottom of the page at the time of writing):
https://deepmind.com/alphago-games-english
https://deepmind.com/alphago-vs-alphago
http://gokifu.com/player/Alphago

Now copy/move the downloaded files to the previously-specified GoBan-SS game data directory (remove any duplicates before this):
sudo mv -v ~/Downloads/*.sgf  /usr/share/goban

Finally, start the XScreenSaver daemon and run the configuration utility:
xscreensaver &
xscreensaver-demo

Salt and pepper to taste, so to speak: tweak the GoBan-SS settings to suit your particular aesthetic preferences.

On to another brilliant point raised by our comrade(s) who wrote that Czech blog post: in the XScreenSaver configuration utility:
  - If this terminal is used for accessing sensitive materials:
      - In the "Image Manipulation" panel,
      - Switch to the "Display Modes" tab
          - Enable "Lock Screen After"
          - Configure the delay before screen locking activates
      - Switch to the "Advanced" tab
          - Disable "Grab Desktop Images"
  - Configure the screen saver to pull from your organization's branding:
      - In the "Image Manipulation" panel,
      - Switch to the "Advanced" tab
          - Enable "Choose Random Image"
          - Input your organization's RSS feed URL for screen saver artwork
      - In the "Text Manipulation" panel,
      - Select the "URL" radio button
      - Input your organization's RSS feed URL for news feeds

We'll further configure some screen savers:
  - Sonar:
      - enable the suid bit on the executable:
        chmod +s /usr/lib/xscreensaver/sonar
      - configure sonar to ping the hosts you'd like to monitor
  - WebCollage, given that your workstation is in a position where running `driftnet` would yield interesting results:
      - Run some commands, as per https://www.linuxtutorial.co.uk/tcpdump-eth0-you-dont-have-permission-to-capture-on-that-device/:
        sudo groupadd pcap
        sudo usermod -a -G pcap `whoami`
        sudo chgrp pcap /usr/bin/driftnet
        sudo chmod 0755 /usr/bin/driftnet
        sudo setcap cap_net_raw,cap_net_admin=eip /usr/bin/driftnet
      - X Run some more commands:
        X sudo chmod +s /usr/bin/driftnet
      - Open the XScreenSaver configuration utility
      - Switch to the "Display Modes" tab
      - Find Web Collage
      - Press the "Advanced" button
      - Adjust the command line to run driftnet to source images for the screen saver:
        webcollage -root -driftnet 'sudo driftnet -a'
      - X If you ran the additional commands, then use:
        X webcollage -root -driftnet 'driftnet -a'

DISCLAIMER:
The information contained within this article is purely hypothetical,
and intended for educational purposes
(i.e., to demonstrate how a red team organization may be structured and operate).
