---
layout: post
title:  "OBS and Virtual Cam (Linux)"
date:   2020-09-10 011:54:29 +0200
tags: [teaching, dissemination, OBS, video recording, camera, linux]
---

As part of now remote conferences or courses, you may need to show many things: not only your slides, but also your terminal, your browser, or you!
There are many (proprietary) systems out there for sharing your screen live: BigBlueButton, Jitsi, Google meet, Zoom, Skype, Teams, etc.
My recent experience is that it does not work so well. Hence an idea is to create a **virtual camera** and project what you want through it.
That is, you create this fake camera and then broadcast to any conferencing system (you "just" have to select the virtual cam).
Some details below about I setup everything to achieve this on Linux with [OBS](https://obsproject.com/), [v4l2loopback](https://github.com/umlaeute/v4l2loopback), [obs-v4l2sink](https://github.com/CatxFish/obs-v4l2sink), [ffmpeg](https://ffmpeg.org/), etc.


Olivier Barais, a colleague of mine, shared this idea of "fake camera" a few months ago.
He mentionned this nice article [https://blog.jbrains.ca/permalink/using-obs-studio-as-a-virtual-cam-on-linux]("Broadcast from OBS Studio To Everything In Linux"), but I didn't get much attention at that time.
But recently, I had some variants-troubles with Teams: basically the screensharing was very limited or simply not supported (aka crash).
So I started to follow some tutorials.

### v4l2loopback

There are at least two subtilities:
 * don't forget to install linux headers, on the right version... otherwise you won't be able to build it. I suggest this line: `dnf install "kernel-devel-uname-r == $(uname -r)"`
 * `sudo modprobe v4l2loopback card_label="Virtual Cam"` might not be sufficient. The camera will be created but not recognized by third party applications like OBS or Web browsers. I suggest to use this line: `sudo modprobe v4l2loopback devices=1 video_nr=10 card_label="OBS Cam" exclusive_caps=1`

For checking: `v4l2-ctl --all` and `v4l2-ctl -d /dev/video10 --list-formats-ext` as well as `ls -al /dev | grep video` (don't forget to install `sudo dnf install v4l-utils`). 
At this step, some (hopefully all) applications might recognize your virtual camera. A useful link is https://webrtc.github.io/samples/src/content/devices/input-output/ to test whether your camera is recognized. 
But there is nothing to broadcast.

### vl42sink

It's an OBS plugin. I have impression there is some default plugins in the Windows version of OBS that simplifies your life.
On Linux, it's not really standardized (yet?). 
You can use [this repo](https://github.com/CatxFish/obs-v4l2sink.git) or [this one](https://github.com/AndyHee/obs-v4l2sink), I don't remember the actual differences.

I highly recommend the reading of [this article](https://spot.livejournal.com/327990.html).
On Fedora at least, you should change some lines in `external/FindLibObs.cmake`
```
@@ -95,7 +95,7 @@ if(LIBOBS_FOUND)
 
        set(LIBOBS_INCLUDE_DIRS ${LIBOBS_INCLUDE_DIR} ${W32_PTHREADS_INCLUDE_DIR})
        set(LIBOBS_LIBRARIES ${LIBOBS_LIB} ${W32_PTHREADS_LIB})
-       include(${LIBOBS_INCLUDE_DIR}/../cmake/external/ObsPluginHelpers.cmake)
+       include(/usr/lib64/cmake/LibObs/ObsPluginHelpers.cmake)
```
otherwise it won't build.
Also `cmake -DLIBOBS_INCLUDE_DIR="/usr/include/obs" -DCMAKE_INSTALL_PREFIX=/usr ..` is the right approach.
And finally `sudo mv /usr/lib/obs-plugins/v4l2sink.so /usr/lib64/obs-plugins/v4l2sink.so`

### OBS

The plugin should now work on OBS, that is, under the "Tools" menu there should be now "V4L2 Video Output".
Some strange things may happen with the "format", but it's OK

### Misc

I still have some issues for sharing the content of *some* applications that are not listed within OBS (eg I can't share the terminal content).
But it's basically the same limitations with other systems. 
It's due to issues with Wayland/X11. An OBS workaround is `obs-xdg-portal`, this issue was quite useful https://gitlab.gnome.org/feaneron/obs-xdg-portal/-/issues/4
note: you can also experiment with `ffmpeg` and redirect the streaming of OBS.


A bit chaotic, but well it's also a good opportunity to learn many things about video, build, Linux, etc. 
Now you can compose scenes in OBS, and broadcast through the virtual cam in Jitsi, Zoom, Teams, etc.
This virtual camera can be seen as an independent way to share whatever you want and whatever the technology is... And you can leverage the power of OBS.













 














