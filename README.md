# FFmpegSCTE-35patch
## Tired of having your SCTE-35 Streams (__0x86__) turned into bin data ( __0x06__) by ffmpeg? This will fix it. 
Patch ffmpeg to let  SCTE-35 streams pass through as SCTE-35

## What does it do?

* The patch allow for SCTE-35 streams to be passed through when encoding mpegts with ffmpeg.

## What doesn't it do?

* It won't fix your SCTE-35.

## How does it work?

* The patch is only nine lines, it just allows you copy a SCTE-35 stream over when you're encoding with ffmpeg.
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
1. Clone ffmpeg repo version N-118437-g4d9cdf82ee
2. Apply patch
```js
patch --directory=FFmpeg/ -strip=1 < ff-scte35.patch
```
3. Run configure ( _do this with whichever options you like_ )
4. make all install

## Use

* original file
![image](https://github.com/user-attachments/assets/b8816336-37a8-439e-87a1-d904f2815d7c)

* encode
![image](https://github.com/user-attachments/assets/3c0190b0-479e-40ce-9c2e-9168919489a8)

* new file
![image](https://github.com/user-attachments/assets/8d7f317c-5e98-4e19-94eb-ad1bcf0461e1)

