FROM tiangolo/nginx-rtmp

COPY nginx.conf /etc/nginx/nginx.conf
COPY index.html /www/
RUN apt update
RUN apt install ffmpeg -y