From dorftv/ffmpeg-core

ENV DEBIAN_FRONTEND noninteractive
ENV PATH $PATH:/usr/local/nginx/sbin
ENV NGINX_VERSION=1.9.14
ENV NGINX_RTMP_VERSION=1.1.7.10



RUN apt-get install -y libpcre3-dev zlib1g-dev libssl-dev unzip && \
    mkdir /src && mkdir /config && mkdir /logs && mkdir /data && mkdir /static && cd /src && \
    wget http://nginx.org/download/nginx-$NGINX_VERSION.tar.gz && tar -xzvf nginx-$NGINX_VERSION.tar.gz && \
    wget https://github.com/sergey-dryabzhinsky/nginx-rtmp-module/archive/v$NGINX_RTMP_VERSION.zip && unzip -e v$NGINX_RTMP_VERSION.zip && \
    cd nginx-$NGINX_VERSION && ./configure --add-module=/src/nginx-rtmp-module-$NGINX_RTMP_VERSION --conf-path=/config/nginx.conf --error-log-path=/logs/error.log --http-log-path=/logs/access.log && \
    make -j5 && make install && rm -rf /src && apt-get clean && apt-get autoremove

ADD static /static
ADD nginx.conf /config/nginx.conf

CMD "nginx"


