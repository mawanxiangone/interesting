# 🏂FFmpeg教程

## 🚴‍♀️提取视频

### 🚣‍♀️时间戳提取

```bash
ffmpeg.exe -i F:\12344.mp4 -ss 00:00:00 -t 00:20:37 output.mp4
```

### 🚣‍♀️帧数提取

```bash

```

### 🚣‍♀️合成视频

```bash

```

## 🚴‍♀️去除视频水印

### 🚣‍♀️使用delogo滤镜

```bash

```

### 🚣‍♀️使用裁剪

```bash


```

## 🚴‍♀️音频

### 🚣‍♀️音视频分离

```bash
# 提取音频
ffmpeg -i input.mp4 -vn output.mp3
# 提取音频,保留原始质量
ffmpeg -i VID_20240714_202331.mp4 -c:a copy -vn output_audio.mp3


## 分离 略微降低视频质量
ffmpeg -i input.mp4 -an output.mp4

ffmpeg -i VID_20240714_202331.mp4 -c:v copy -an output_silent_video.mp4
# -c:v copy:ffmpeg 直接从输入文件复制视频流到输出文件,而无需重新编码.这将保留原始视频质量.
# -an:effectively removes all audio tracks, resulting in a silent video.

```

### 🚣‍♀️音视频合成

```bash 
ffmpeg -i output_silent_video.mp4 -i 3669198660.flac -shortest output_video_with_audio.mp4
```