## FFmpeg and Opus Audio Codec (16bit, 48kHz)
- Opus is a 48kHz-only format. Other input-samplerates will be internaly converted, using a *Speex* resampler.
-  Input bitdepth for Opus is 16bit.
- Channel-Mapping is the same like Vorbis.
- FFmpeg has an [internal](https://www.ffmpeg.org/ffmpeg-codecs.html#opus) Opus encoder. You can use this if you want.

#### 1. Convert a 16bit 5.1 audiofile to ~512kbps Opus with "opusenc.exe":
	"ffmpeg.exe" -i "input.ac3" -af "pan=0x3f|c0=FL|c1=FC|c2=FR|c3=SL+BL|c4=SR+BR|c5=LFE" -f s16le -ac 6 - | "opusenc.exe" - --raw --raw-chan 6 --raw-bits 16 --vbr --bitrate 512 --comp 10 --expect-loss 0 "output.opus"

#### 2. Convert a 24bit 5.1 audiofile to ~512kbps Opus with "opusenc.exe":
	"ffmpeg.exe" -i "input.dts" -af "aresample=resampler=soxr:dither_method=shibata,aformat=sample_fmts=s16,pan=0x3f|c0=FL|c1=FC|c2=FR|c3=SL+BL|c4=SR+BR|c5=LFE" -f s16le -ac 6 - | "opusenc.exe" - --raw --raw-chan 6 --raw-bits 16 --vbr --bitrate 512 --comp 10 --expect-loss 0 "output.opus"

#### 3. Convert a 24bit 7.1 audiofile to ~512kbps Opus with "opusenc.exe":
	"ffmpeg.exe" -i "input.dts" -af "aresample=resampler=soxr:dither_method=shibata,aformat=sample_fmts=s16,pan=0x63f|c0=FL|c1=FC|c2=FR|c3=SL|c4=SR|c5=BL|c6=BR|c7=LFE" -f s16le -ac 8 - | "opusenc.exe" - --raw --raw-chan 8 --raw-bits 16 --vbr --bitrate 512 --comp 10 --expect-loss 0 "output.opus"