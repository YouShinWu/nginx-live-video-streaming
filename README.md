## How to run

## UPLOAD
1.Open OBS and in settings set the server to `rtmp://localhost:1935/live` and the stream key to `abdi?key=mmlab`
2.Use ffmpeg
ffmpeg -re -i INPUT_FILE_NAME -c copy -f flv rtmp://localhost/live/STREAM_NAME
ex: ffmpeg -re -i /ToyStory4/ToyStory4.00001.mp4 -c copy -f flv rtmp://140.118.107.174:1935/live/abdi?key=mmlab


## DOWNLOAD
Open VLC and Open a Network Stream and set the url to `rtmp://localhost:1935/live/abdi`
You should see your live stream now!
