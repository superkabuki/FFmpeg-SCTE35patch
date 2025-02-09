# SuperKabuki SCTE35 patch for FFmpeg

 FFmpeg changes mpegts SCTE-35 Streams (__0x86__) into bin data Streams (__0x06__).  
<br> This is a patch for FFmpeg to let SCTE-35 mpegts Streams copy over as SCTE-35 Streams.

---

 To make sure we are all on the same page, [This repo](https://github.com/superkabuki/FFmpeg_SCTE35) has the ffmpeg source with the patch applied. I just sync'ed it up about an hour ago. (_Sunday, February 09 2025 11:25pm UTC_)


## ...why?
If you work with SCTE-35 and you use ffmpeg,<br> having the SCTE-35 stream type 0x86 changed to bin data 0x06, makess life hard.<br> <br>
After reading the mpegts.c mpegtsenc.c files in FFmpeg,<br>
I realized that I could fix the 0x86 to 0x06 issue with just a few lines of code.<br> By few, I mean nine, and I think one or two lines may be redundant.<br>
<br>


## How does it work?

* The patch is only nine lines of code, it allows you copy a SCTE-35 stream over as SCTE-35, when you're encoding with ffmpeg.
* The patch also adds the SCTE-35 Descriptor __(CUEI / 0x49455543)__ , just to be fancy.
* Everything else works just like unpatched ffmpeg.
---


## Install 


1.    `git clone https://github.com/superkabuki/FFmpeg_SCTE35.git`

2.    `cd FFmpeg_SCTE35`

3.    `./configure --enable-shared --extra-version=-SuperKabuki-patch` ( you can customize this as needed )

4.    `make all`

5.    `sudo make install`

---

## How to use:

### These are all super important. 

* map the SCTE-35 stream to the output file like  `-map 0` or maybe `-map 0:d` etc..
* Set the SCTE-35 stream to copy like `-dcodec copy` or `-c copy` etc..
* Use `-copyts` if you want your SCTE-35 and PTS to stay aligned 
* Use `-muxpreload 0` and  `-muxdelay 0` to avoid the 1.4 second start bump
---
* This a minimal example command.
```
ffmpeg -copyts -i input.ts -map 0 -c:d copy -muxpreload 0 -muxdelay 0 output.ts
```
---

# Examples


## Example 1:  Re-encode video to H265 and copy over the SCTE-35


* original file

![image](https://github.com/user-attachments/assets/b8816336-37a8-439e-87a1-d904f2815d7c)

* ffmpeg command

![image](https://github.com/user-attachments/assets/3c0190b0-479e-40ce-9c2e-9168919489a8)

* new file

![image](https://github.com/user-attachments/assets/2b76b386-814f-431b-a07a-a6eaa7001a12)


* Notice the start PTS are still pretty much in sync, even after encoding to H.265.

---

## Example 2:  Copy all streams, including SCTE-35, and cut the first 200 seconds.


* original file

![image](https://github.com/user-attachments/assets/30d88882-0814-4609-92fc-53ef29e77bae)

* ffmpeg command

 ![image](https://github.com/user-attachments/assets/21b1b49a-c9a2-4e8b-8322-2b4f5755a51e)

* new file

![image](https://github.com/user-attachments/assets/f2cf31c6-90a4-428c-97bd-4ca82823fc71)


> Notice the start time and duration have both changed by ~200 seconds.

* old
```js
 Duration: 00:04:56.30, start: 72667.595200, bitrate: 962 kb/s
```
* new
```js
  Duration: 00:01:37.75, start: 72866.141867, bitrate: 962 kb/s
```
---

