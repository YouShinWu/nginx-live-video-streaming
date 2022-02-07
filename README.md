
## Live Video Streaming Server based on nginx and Node.js with ABR Streaming

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

## ABR STREAMING (Adaptive bit-rate streaming)

OBS: Open OBS and in settings set the server to `rtmp://140.118.107.174:1935/src` and the stream key is `mmlab?key=mmlab`  `{User}?key={password}`--> User is the stream name

rtmp://140.118.107.174:1935/src/mmlab

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
1.Docker image: https://github.com/tiangolo/nginx-rtmp-docker 
2.https://github.com/arut/nginx-rtmp-module/wiki/Directives
3.Video.js: https://github.com/videojs/http-streaming

## FOR 4k

HEVC -> flv not compatible
But someone modify flv standard (12bit -> 16bit)
Not flv standard.

1. h264 4k --> Need big bandwidth     HEVC :24 mb AVC:110 mb
2. Av1

## ABOUT AV1 codec ([libaom-av1 @ 0x557f4ff153c0] v2.0.1)
Need lots of cobing time!!!
https://www.winxdvd.com/video-transcode-tips/av1-hardware-support.htm

ffmpeg av1 : https://ffmpeg.org/ffmpeg-codecs.html#libaom_002dav1
https://trac.ffmpeg.org/wiki/Encode/AV1

## FOR 120fps
dataset:
http://ultravideo.fi/#testsequences

## NVIDIA ENCODER GPU
https://www.nvidia.com/en-us/geforce/guides/broadcasting-guide/
https://developer.nvidia.com/video-encode-and-decode-gpu-support-matrix-new#Encoder

## YOUTUBE STANDARD
https://support.google.com/youtube/answer/2853702?hl=zh-Hant#zippy=%2Ckp-fps%2Cp-fps%2Cp
https://support.google.com/youtube/answer/1722171?hl=zh-Hant#zippy=%2C%E9%9F%B3%E8%A8%8A%E8%BD%89%E7%A2%BC%E5%99%A8aac-lc%2C%E5%BD%B1%E7%89%87%E8%BD%89%E7%A2%BC%E5%99%A8h%2C%E5%AE%B9%E5%99%A8mp%2C%E5%BD%B1%E6%A0%BC%E9%80%9F%E7%8E%87%2C%E4%BD%8D%E5%85%83%E7%8E%87




## DVR function



 ffmpeg -re -f lavfi -i testsrc -c:v libx264 -b:v 1600k -preset ultrafast -b 900k -c:a libfdk_aac -b:a 128k -s 1920×1080 -x264opts keyint=50 -g 25 -pix_fmt yuv420p -f flv “rtmp://p.ep246802.i.akamaientrypoint.net/EntryPoint flashver=FMLE/3.020(compatible;20FMSc/1.0) live=true pubUser=123456 pubPasswd=789123 playpath=dclive_1_1@246802”

 ffmpeg -re -i /h265/h264-4k_Batman.00003.mp4 -c copy -f flv rtmp://140.118.107.174:1935/live/mmlab


 ffmpeg -i input
       -vf "settb=AVTB,
            setpts='trunc(PTS/1K)*1K+st(1,trunc(RTCTIME/1K))-1K*trunc(ld(1)/1K)',
            drawtext=text='%{localtime}.%{eif\:1M*t-1K*trunc(t*1K)\:d}'"
       -f flv out

## RTSP Server


## Convert HEVC to HLS

#convert to hdr10

ffmpeg -y -i .\h265-ToyStory4.00002.mp4 -c:v libx265 ^
    -tag:v hev1 -pix_fmt yuv420p10le -s 1280x720 ^
    -x265-params "colorprim=bt2020:transfer=smpte2084:colormatrix=bt2020nc:bitrate=4000:keyint=120:strict-cbr" ^
    -c:a copy .\hevc\HDR10.mp4

ffmpeg -y -i .\h265-ToyStory4.00002.mp4 -c:v libx265 ^
    -tag:v hvc1 -pix_fmt yuv420p10le -s 1280x720 ^
    -x265-params "colorprim=bt2020:transfer=smpte2084:colormatrix=bt2020nc:bitrate=4000:keyint=120:strict-cbr" ^
    -c:a copy .\hevc\HDR10.mp4



ffmpeg -y -i .\Test_4k_hevc.mp4 -c copy ^
    -hls_segment_type mpegts -hls_time 6 -hls_list_size 10 ^
    -hls_flags delete_segments+append_list+split_by_time ^
    -hls_playlist_type vod .\hevc\mmlab.m3u8


## ffmpeg merge video

..\ffmpeg.exe -f concat -safe 0 -i Text.txt -c copy Test_4k_hevc.mp4