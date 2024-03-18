## Tested codecs

- Alpary
- ArithYuv
- AVIzlib
- CamStudio GZIP
- CorePNG
- FastCodec
- FFV1
- Huffyuv
- Lagarith
- LOCO
- LZO
- MSU Lab
- PICVideo
- Snow
- x264
- YULS

## Comparison Content

The main goal of the performed comparison is getting answers on the following questions regarding lossless video codecs:

- What codec or codecs are best for video capture and video editing applications?
- What codec or codecs achieve the best compression ratio?
- What advantages multithreading gives to modern codecs supporting it?

Only absolutely lossless codecs were examined in this comparison. Only progressive test video sequences were used.

## Main conclusions

- In Video Capture and Video Editing Area the overall clear winner is `Lagarith`.
- In Maximum Compression area the overall winner is `YULS`.
- The most balanced and flexible codec is `FFV1`: relatively good speed and high compression for various presets.
- The comparison paper is 130 pages long and contains a lot of graphs to facilitate analysis like this one:
![alt text](<assets/Lossless Video Codecs Comparison ‘2007/image.png>)