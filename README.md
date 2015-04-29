#ffmpeg docker container

ffmpeg is built with support for:

```--enable-librtmp --extra-libs=-ldl --enable-gpl --enable-libass --enable-libfdk-aac --enable-libmp3lame --enable-libopus --enable-libtheora --enable-libvorbis --enable-libvpx --enable-libx264 --enable-nonfree --enable-libx265 ```

the container is built using ansible and the [dorftv.fmpeg](https://github.com/dorftv/ansible-role-ffmpeg.git)  ansible role.

The image is build on the [automated docker hub](https://registry.hub.docker.com/u/dorftv/ffmpeg/). you can simple run it with

``` 
alias ffmpeg="docker run --rm dorftv/ffmpeg" 
ffmpeg --help 
```
