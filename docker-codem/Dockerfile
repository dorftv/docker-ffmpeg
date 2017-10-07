FROM dorftv/ffmpeg-core

RUN cd /tmp && \
    apt-get update && apt-get install -y wget && \
    wget https://codem-downloads.s3.amazonaws.com/codem-scheduler_0.0.0_amd64.deb --no-check-certificate && \
    dpkg -i codem-scheduler_0.0.0_amd64.deb

WORKDIR /opt/codem-scheduler

cmd    ["/opt/codem-scheduler/run.sh"]
