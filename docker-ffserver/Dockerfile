FROM dorftv/ffmpeg-core
MAINTAINER Stefan Hageneder <stefan.hageneder@dorftv.at>

copy ffserver.conf /root/
CMD           ["-f", "/root/ffserver.conf"]
ENTRYPOINT    ["ffserver"]
