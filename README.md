# FFmpegSCTE-35patch
## Tired of having your SCTE-35 Streams (__0x86__) turned into bin data ( __0x06__) by ffmpeg? This will fix it. 
Patch ffmpeg to let  SCTE-35 streams pass through as SCTE-35

## What does it do?

* The patch allow for SCTE-35 streams to be passed through when encoding mpegts with ffmpeg.

## What doesn't it do?

* It won't fix your SCTE-35.

## How does it work?

* The patch is only nine lines of code, it allows you copy a SCTE-35 stream over as SCTE-35, when you're encoding with ffmpeg.
* Everything else works just like unpatched ffmpeg.

## How to use:

### These are all super important. 

* map the SCTE-35 stream to the output file like  `-map 0` or maybe `-map 0:d` etc..
* Set the SCTE-35 stream to copy like `-dcodec copy` or `-c copy` etc..
* Use `-copyts` if you want your SCTE-35 and PTS to stay aligned 
* Use `-muxpreload 0` and  `-muxdelay 0` to avoid the 1.4 second start bump

## Example Usage:

```js
 ffmpeg -copyts -i  ~/mpegts2/mpegts/nmx.ts -map 0 -c:v libx265 -c:a aac -c:d copy -muxpreload 0 -muxdelay 0 -y  passed.ts
``` 

## Applies to 
```sh
ffmpeg version N-118437-g4d9cdf82ee Copyright (c) 2000-2025 the FFmpeg developers
```

## Install 
1. Clone [ffmpeg repo version N-118437-g4d9cdf82ee](https://github.com/superkabuki/FFmpeg)
2. Apply patch
```js
patch --directory=FFmpeg/ -strip=1 < ff-scte35.patch
```
3. Build ffmpeg  ( _do this with whichever options you like_ )

# Examples


## Example 1:  Re-encode video to H265 and copy over the SCTE-35


* original file
![image](https://github.com/user-attachments/assets/b8816336-37a8-439e-87a1-d904f2815d7c)

* ffmpeg command
![image](https://github.com/user-attachments/assets/3c0190b0-479e-40ce-9c2e-9168919489a8)

* new file
![image](https://github.com/user-attachments/assets/2b76b386-814f-431b-a07a-a6eaa7001a12)

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
* The patch also adds the SCTE-35 Descriptor __(CUEI / 0x49455543)__ , just to be fancy.

