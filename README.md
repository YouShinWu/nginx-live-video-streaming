## How to run
Live Video Streaming Server based on nginx and Node.js

flv --> hls

## UPLOAD Stream
(flv)
1.Open OBS and in settings set the server to `rtmp://localhost:1935/live` and the stream key to `mmlab?key=mmlab`  `{User}?key={password}`
2.Use ffmpeg
ffmpeg -re -i INPUT_FILE_NAME -c copy -f flv rtmp://localhost/live/STREAM_NAME
ex: ffmpeg -re -i /ToyStory4/ToyStory4.00001.mp4 -c copy -f flv rtmp://140.118.107.174:1935/live/mmlab?key=mmlab

(hls)
1.Open OBS and in settings set the server to `rtmp://localhost:1935/live` and the stream key to `mmlab?key=mmlab`  `{User}?key={password}`
2.Use ffmpeg
ffmpeg -re -i INPUT_FILE_NAME -c copy -f flv rtmp://localhost/live/STREAM_NAME
ex: ffmpeg -re -i /ToyStory4/ToyStory4.00001.mp4 -c copy -f flv rtmp://140.118.107.174:1935/live/mmlab?key=mmlab

## PULL Stream
(flv)
Open VLC and Open a Network Stream and set the url to `rtmp://localhost:1935/live/mmlab`
You should see your live stream now!

(hls)
use this link on chrome(need to download Native HLS Playback first: https://chrome.google.com/webstore/detail/native-hls-playback/emnphkkblegpebimobpbekeedfgemhof)

http://140.118.107.174:7080/hls/mmlab.m3u8 