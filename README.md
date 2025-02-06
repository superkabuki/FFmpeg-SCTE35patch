# FFmpegSCTE-35patch
Patch ffmpeg to let  SCTE-35 streams pass through as SCTE-35
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
3. Run configure
4. make all install

## Use
works just like unpatched ffmpeg, encode what you want, but keep the timestamps and copy the SCTE-35 stream.
* original file
![image](https://github.com/user-attachments/assets/b8816336-37a8-439e-87a1-d904f2815d7c)

* encode
![image](https://github.com/user-attachments/assets/a975de6e-fb93-4ccf-8551-997fc651cdb2)
