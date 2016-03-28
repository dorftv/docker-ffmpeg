#ffprobe docker image
based on [dorftv/ffmpeg-core]()

The image is built on the [automated docker hub](https://registry.hub.docker.com/u/dorftv/ffmpeg/). You can simple run it with

``` 
alias ffprobe="docker run --rm dorftv/ffprobe" 
ffmpeg --help 
```
