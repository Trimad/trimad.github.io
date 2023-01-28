---
title: youtube-dl
author: Tristan Madden
categories: [CMD, youtube-dl]
tags: [audio, video,🏴‍☠️]
date: 2022-04-16
---
youtube-dl is a command-line program that allows users to download videos from YouTube and other video-sharing websites. The program is open-source and can be used on a variety of operating systems, including Windows, Mac, and Linux. It can download videos in a variety of formats, including MP4, FLV, and 3GP, and can also extract audio from videos as MP3 or other audio formats. Additionally, youtube-dl can be used to download videos from other websites like Vimeo, Dailymotion, and more.

<h2>Examples</h2>
Download a YouTube video with the best available audio quality:

```Shell
youtube-dl -f bestaudio "https://www.youtube.com/watch?v=iik25wqIuFo"
```

Download an entire YouTube playlist:

```Shell
youtube-dl --yes-playlist "https://www.youtube.com/playlist?list=PLG49S3nxzAnmpdmX7RoTOyuNJQAb-r-gd"
```

Begin downloading from a playlist at a specific position:

```Shell
youtube-dl --yes-playlist --playlist-start 22 "https://www.youtube.com/watch?v=M5c9HdaQqh0&list=PLG49S3nxzAnmpdmX7RoTOyuNJQAb-r-gd"
```

<h2><a href="https://yt-dl.org/latest/youtube-dl.exe">Download the Windows executable</a></h2>