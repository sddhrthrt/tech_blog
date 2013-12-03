---
layout: post
title: "Screencasting in Ubuntu - Ultimate Solution"
date: 2013-05-07 04:00
comments: true
categories:  howto linux ubuntu
---

While doing a couple of screencasts that we had to do as a part of our course, I
went through the process of learning how to create a screencast with just the
basic [ffmpeg][ffmpeg] tool. After facing a couple of hurdles and several iterations of
juggling with settings later, I finally came up with the right recipe: It works
smooth as a charm.

<!-- more -->

Without further ado, here is the final combo that worked:

``` bash Screencasting recipe
    ffmpeg -f alsa -ac 2 -i pulse -acodec alac -ab 128k -f x11grab 
    -s `xdpyinfo | grep 'dimensions:'|awk '{print $2}'` -r 25 -i :0.0 -sameq
    final.mkv
```

Here, `ffmpeg` is the tool that comes with the linux toolkit - it's used for
recording/transcoding video. This can do almost everything you want it to do in
terms of video/audio encoding/transcoding/cropping/merging or whatever. 

The options:

* `-f alsa` means use format [`alsa`][alsa]. It's an audio format, and when you
  mention an audio format, `ffmpeg` knows that it should use the format for the
  audio channel.

* `-ac 2` tells `ffmpeg` to record 2 channels of audio with the video.
* `-i pulse` - mind you, many tutorials out there suggest you to use `-i hw:0,0`
  or something similar. This did not work great for me, in fact, it worked much
  worse. Audio was crackling, video lagged and slowly audio and video lost sync
  along with time.

* `-acodec alac` asks `ffmpeg` to use a particular codec, and it worked well.
* `-ab 128k` asks `ffmpeg` to use 128kbit/s audio bitrate. This is good enough
  for most applications.
* `-f x11grab` is the next format we are going to use - it's the screengrabbing
  format. 
* `` -s `xdpyinfo | grep 'dimensions:'|awk '{print #2}'` `` - this piece of code
  here actually fetches your screen size! It gives the screen width and height
  as input to the `-s` option, as the size of the screen to be captured.
* `-r 25` denotes the frame rate. 25 should usually work, unless your screen has
  a specific application running in a particular framerate and you need to
  capture it in that particular framerate. You could actually experiment around
  it to get good results.
* `-i :0.0` asks `ffmpeg` to take video input from the [:0.0][screens] - that's
  the first screen connected to the computer. 
* `-sameq` modifier tells ffmpeg to keep the same quality as input - that's
  kinda making the solution easier.


### Important things to keep in mind during a screencast:

* Never use the laptop keyboard if your recording rig is a laptop - the laptop
  key clicks get recorded in the mic. Apparently [Thinkpad is bringing a new
  solution](noiseremoval) for this problem. 

* Sit in a place without much wind directly hitting your mic. Clean the mic and
  the hole where it's hosted - helps improve the sound.

* You could use vlc/smplayer and open up a window with webcam on the screen, so
  you could have yourself on the screencasting. Here's how to do it for
  smplayer:


``` bash Showing webcam in mplayer
  mplayer -tv driver=v4l2:gain=1:width=640:height=480:device=/dev/video0:fps=10:outfmt=rgb16 tv://
```

* Alright, after you record in the aforementioned way, you might want to convert
  the mkv to avi or some other lighter format - actually the bitrate is quite
  for the video because of the `-sameq` modifier. If you actually mention a
  lower bitrate for the video part, the size may decrease. I got 190MB for a 15
  minute video.

Happy Screencasting!

[ffmpeg]: http://www.ffmpeg.org/
[screens]: http://www.x.org/archive/X11R6.8.1/doc/X.7.html
[noiseremoval]: http://blog.lenovo.com/news/a-portfolio-of-thinkpad-innovations
[alsa]: https://wiki.archlinux.org/index.php/Advanced_Linux_Sound_Architecture
