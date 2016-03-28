ffmpeg docker image
###################

Full featured ffmpeg image with all needed libraries for webvideo and decklink capturing.

    --enable-decklink --enable-librtmp --extra-libs=-ldl --enable-gpl 
    --enable-libass --enable-libfdk-aac --enable-libmp3lame --enable-libopus 
    --enable-libtheora --enable-libvorbis --enable-libvpx --enable-libx264 
    --enable-nonfree --enable-libx265

the container is built using ansible and the [dorftv.ffmpeg](https://galaxy.ansible.com/dorftv/ffmpeg/)  ansible role. If you want to compile on bare metal or add features this is a good starting point.  
FFmpeg 

The images are built on the automated docker hub and comes in different flavors:
[dorftv/ffmpeg](https://hub.docker.com/r/dorftv/ffmpeg/) exposes ffmpeg 
[dorftv/ffprobe](https://hub.docker.com/r/dorftv/ffprobe/) exposes ffprobe
[dorftv/ffserver](https://hub.docker.com/r/dorftv/ffserver/) exposes ffserver (TODO)
[dorftv/ffmpeg-core](https://registry.hub.docker.com/u/dorftv/ffmpeg-core) for the configuration and compilation part.

Examples for dorftv/ffmpeg
##########################

dorftv/ffmpeg for transcoding files
-----------------------------------

you need to make sure that the directory holding your videos is available in the container. Use the
-v option in docker to mount your directory. 

``` 
alias ffmpeg="docker run --rm -v /srv/assets:/srv/assets dorftv/ffmpeg"  

ffmpeg -i /srv/assets/input.avi -vcodec libx264 -preset slow -crf 20 -acodec libfdk_aac -b:a 256k 
-movflags faststart  -threads 0 /srv/assets/output.mp4
```


dorftv/ffmpeg for decklink capturing
------------------------------------

FFmpeg supports capturing of Blackmagic devices ( Decklink SDI, and Intensity Pro). You can consult 
the [FFmpeg Help Page](https://www.ffmpeg.org/ffmpeg-devices.html#decklink) for detailed Information on how to use the Decklink consumer.
For ffmpeg to find your capture device you need to install the [Blackmagic Desktopvideo driver ](https://www.blackmagicdesign.com/at/support/family/capture-and-playback)
package on your host system and add **--device /dev/blackmagic/dv0** to docker. You also need to mount **/usr/lib/libDeckLinkAPI.so** into the container.
If you want to write files make sure you mount your data directory. You don't need that if you intend to feed a streaming server.

``` 
alias ffmpeg="docker run -it --rm --device /dev/blackmagic/dv0 -v \
/usr/lib/libDeckLinkAPI.so:/usr/lib/libDeckLinkAPI.so \
-v /srv/assets:/srv/assets dorftv/ffmpeg"  

# Get a list of devices.
ffmpeg -f decklink -list_devices 1 -i dummy
[decklink @ 0x3ec7480] Blackmagic DeckLink devices:
[decklink @ 0x3ec7480]  'DeckLink SDI'

# Get a list of formats for that device.
ffmpeg -f decklink -list_formats 1 -i 'DeckLink SDI'
[decklink @ 0x3c24480] Supported formats for 'DeckLink SDI':
[decklink @ 0x3c24480]  1       720x486 at 30000/1001 fps (interlaced, lower field first)
[decklink @ 0x3c24480]  2       720x486 at 24000/1001 fps
[decklink @ 0x3c24480]  3       720x576 at 25000/1000 fps (interlaced, upper field first)
[decklink @ 0x3c24480]  4       1920x1080 at 24000/1001 fps
[decklink @ 0x3c24480]  5       1920x1080 at 24000/1000 fps
[decklink @ 0x3c24480]  6       1920x1080 at 25000/1000 fps
[decklink @ 0x3c24480]  7       1920x1080 at 30000/1001 fps
[decklink @ 0x3c24480]  8       1920x1080 at 30000/1000 fps
[decklink @ 0x3c24480]  9       1920x1080 at 25000/1000 fps (interlaced, upper field first)
[decklink @ 0x3c24480]  10      1920x1080 at 30000/1001 fps (interlaced, upper field first)
[decklink @ 0x3c24480]  11      1920x1080 at 30000/1000 fps (interlaced, upper field first)
[decklink @ 0x3c24480]  12      1280x720 at 50000/1000 fps
[decklink @ 0x3c24480]  13      1280x720 at 60000/1001 fps
[decklink @ 0x3c24480]  14      1280x720 at 60000/1000 fps

# Select a device@format combination and capture. 
ffmpeg -f decklink -i 'DeckLink SDI@3' \
-c:v libx264 -framerate 25  -b:v 300k -s 320x180 -x264opts keyint=50 -g 25 -pix_fmt yuv420p \
-c:a libfdk_aac -strict -2 -b:a 64k /srv/assets/capture.mp4

```
