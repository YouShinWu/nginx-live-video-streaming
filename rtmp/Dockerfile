FROM tiangolo/nginx-rtmp

ENV TZ=Asia/Taipei
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
COPY nginx.conf /etc/nginx/nginx.conf
COPY ./www /www/
# COPY ./hevc_hls /tmp/hevc/
# RUN apt update
# RUN apt install ffmpeg -y