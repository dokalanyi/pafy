PAFY
====

Python API for YouTube
by nagev


Features:
---------

 - Retreive metadata such as viewcount, duration, rating, author, thumbnail, keywords
 - Download video at requested resolution / format / filesize
 - Retrieve the URL to stream the video in a player such as vlc or mplayer
 - Select highest quality stream for download or streaming
 - Works with age-restricted videos and non-embeddable videos
 - Small, standalone, single importable module file.
 - Works with Python 2.7 and 3.x
 - No dependencies
 - Command line tool (ytdl) for downloading directly from the command line


Usage Examples:
---------------

Here is how to use the module in your own python code.  For command line tool
(ytdl) instructions, see further below:

```python

>>> import pafy


    # create a video instance from a YouTube video url
    
>>> url = "http://www.youtube.com/watch?v=cyMHZVT91Dw"
>>> video = pafy.new(url)


    # get certain attributes
    
>>> video.title
u'Rick Astley Sings Live - Never Gonna Give You Up - This Morning'


>>> video.rating
4.93608852755

>>> video.length
355

    # display video metadata
    
>>> print video
Title: Rick Astley Sings Live - Never Gonna Give You Up - This Morning
Author: Ryan915
ID: cyMHZVT91Dw
Duration: 00:05:55
Rating: 4.93608852755
Views: 672583
Thumbnail: https://i1.ytimg.com/vi/cyMHZVT91Dw/default.jpg
Keywords: Rick, Astley, Sings, Live, on, This, Morning, Never, Gonna, Gunna, Give, You,...


    # show all formats for a video:
    
>>> streams = video.streams
>>> for s in streams:
>>>     print s.resolution, s.extension

480x854 webm
480x854 flv
360x640 webm
360x640 flv
360x640 mp4
240x400 flv
320x240 3gp
144x176 3gp


    # show all formats, file-sizes and their download url:

>>> for s in streams:
>>>     print s.resolution, s.extension, s.get_filesize(), s.url

480x854 webm 56858674 http://r12--sn-aoh8kier.c.youtube.com/videoplayback?expire=1369...
480x854 flv 53066081 http://r11---sn-aoh8kier.c.youtube.com/videoplayback?expire=1369...
360x640 webm 34775366 http://r11---sn-aoh8kier.c.youtube.com/videoplayback?expire=1369...
360x640 flv 32737100 http://r11---sn-aoh8kier.c.youtube.com/videoplayback?expire=1369...
360x640 mp4 25919932 http://r11---sn-aoh8kier.c.youtube.com/videoplayback?expire=1369...
240x400 flv 14341366 http://r11---sn-aoh8kier.c.youtube.com/videoplayback?expire=1369...
320x240 3gp 11083585 http://r11---sn-aoh8kier.c.youtube.com/videoplayback?expire=1369...
144x176 3gp 3891135 http://r11---sn-aoh8kier.c.youtube.com/videoplayback?expire=1369...


    # get best resolution regardless of file format:
    
>>> best = video.getbest()
>>> best.resolution, best.extension
('480x854', 'webm')


    # get best resolution for a particular file format:
    # (mp4, webm, flv or 3gp)
    
>>> best = video.getbest(preftype="mp4")
>>> best.resolution, best.extension
('360x640', 'mp4')


    # get best resolution for a particular file format, or return
    # a different format if it has the best resolution
    
>>> best = video.getbest(preftype="mp4", ftypestrict=False)
>>> best.resolution, best.extension
('480x854', 'webm')


    # get url, for download or streaming in mplayer / vlc etc
    
>>> best.url
'http://r12---sn-aig7kner.c.youtube.com/videoplayback?expire=1369...


    # Download video and show progress:
    
>>> best.download(quiet=False)
-Downloading 'Rick Astley Sings Live - Never Gonna Give You Up - This Morning.webm' [56,858,674 Bytes]
  56,858,674 Bytes [100.00%] received. Rate: [ 720 kbps].  ETA: [0 secs]    
Done


    # Download video, use specific filepath:
    
>>> myfilename = "/tmp/" + best.title + "." + best.extension
>>> best.download(filepath=myfilename)
-Downloading 'Rick Astley Sings Live - Never Gonna Give You Up - This Morning.webm' [56,858,674 Bytes]
Done
```


Command Line Tool (ytdl) Usage:
===============================

```shell

usage: ytdl [-h] [-i] [-s] [-f {webm,mp4,3gp,flv}] [-r NNNxNNN] [-n N] [-b]
            url

YouTube Download Tool

positional arguments:
  url                   YouTube video URL to download

optional arguments:
  -h, --help            show this help message and exit
  -i                    Display video info
  -s                    Display available streams
  -n N                  Specify stream to download by stream number (use -s to
                        list, ignores -f and -r)
  -b                    Download the best quality video (ignores -n, -f and
                        -r)

format and quality:
  Specify the stream to download by resolution and file format

  -f {webm,mp4,3gp,flv}
                        format of the video to download
  -r NNNxNNN            resolution of the video to download

Examples:

# Download best available resolution (-b):

    ytdl "http://www.youtube.com/watch?v=cyMHZVT91Dw" -b

# get video info (-i):

    ytdl "http://www.youtube.com/watch?v=cyMHZVT91Dw" -i

# list available download formats (-s):

  ytdl "http://www.youtube.com/watch?v=cyMHZVT91Dw" -s
  
    Stream  Format  Resolution  Size
    ------  ------  ----------  ----
    1   webm    480x854      54 MB
    2   flv     480x854      50 MB
    3   webm    360x640      33 MB
    4   flv     360x640      31 MB
    5   mp4     360x640      24 MB
    6   flv     240x400      13 MB
    7   3gp     320x240      10 MB
    8   3gp     144x176       3 MB

# Download mp4 360x640, ie. stream number 4 (-n4)

    ytdl "http://www.youtube.com/watch?v=cyMHZVT91Dw" -n 4

# Download flv at 240x400 (-f flv -r 240x400)
 
    ytdl "youtube.com/watch?v=cyMHZVT91Dw" -f flv -r 240x400 

```


