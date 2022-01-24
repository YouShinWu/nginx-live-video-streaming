## How to run
Live Video Streaming Server based on nginx and Node.js with Authientication

flv --> hls/dash

## UPLOAD Stream
(flv)
1.Open OBS and in settings set the server to `rtmp://localhost:1935/live` and the stream key is `mmlab?key=mmlab`  `{User}?key={password}`--> User is the stream name
2.Use ffmpeg
ffmpeg -re -i INPUT_FILE_NAME -c copy -f flv rtmp://localhost/live/STREAM_NAME
ex: ffmpeg -re -i /ToyStory4/ToyStory4.00001.mp4 -c copy -f flv rtmp://140.118.107.174:1935/live/mmlab?key=mmlab

(hls)
1.Open OBS and in settings set the server to `rtmp://localhost:1935/live` and the stream key is `mmlab?key=mmlab`  `{User}?key={password}`
2.Use ffmpeg
ffmpeg -re -i INPUT_FILE_NAME -c copy -f flv rtmp://localhost/live/STREAM_NAME
ex: ffmpeg -re -i /ToyStory4/ToyStory4.00001.mp4 -c copy -f flv rtmp://140.118.107.174:1935/live/mmlab?key=mmlab

(dash)
Same as hls.

## PULL Stream
(flv)
Open VLC Player and Open a Network Stream and set the url to 

rtmp://localhost:1935/live/mmlab

You should see your live stream now!

(hls)
use this link on chrome(need to download Native HLS Playback first: https://chrome.google.com/webstore/detail/native-hls-playback/emnphkkblegpebimobpbekeedfgemhof)

http://140.118.107.174:7080/hls/mmlab.m3u8 

(dash)
download the MSE(Media Streaming Extention) https://chrome.google.com/webstore/detail/native-mpeg-dash-%2B-hls-pl/cjfbmleiaobegagekpmlhmaadepdeedn/related

http://140.118.107.174:7080/dash/mmlab.mpd


hls and dash can also use on VLC player.

##ABR STREAMING (Adaptive bit-rate streaming)
OBS: Open OBS and in settings set the server to `rtmp://140.118.107.174:1935/src` and the stream key is `mmlab?key=mmlab`  `{User}?key={password}`--> User is the stream name

            exec ffmpeg -i rtmp://localhost:1935/src/$name
              -c:a aac -b:a 32k  -c:v libx264 -b:v 128K -f flv rtmp://localhost:1935/live/$name_low
              -c:a aac -b:a 64k  -c:v libx264 -b:v 256k -f flv rtmp://localhost:1935/live/$name_mid
              -c:a aac -b:a 128k -c:v libx264 -b:v 512K -f flv rtmp://localhost:1935/live/$name_hi;

            hls_nested on;

            hls_variant _low BANDWIDTH=160000;
            hls_variant _mid BANDWIDTH=320000;
            hls_variant _hi  BANDWIDTH=640000;

(hls)
http://140.118.107.174:7080/hls/mmlab.m3u8 


(dash)
http://140.118.107.174:7080/dash/mmlab_low.mpd
http://140.118.107.174:7080/dash/mmlab_mid.mpd
http://140.118.107.174:7080/dash/mmlab_hi.mpd

## Reference 
https://github.com/arut/nginx-rtmp-module/wiki/Directives