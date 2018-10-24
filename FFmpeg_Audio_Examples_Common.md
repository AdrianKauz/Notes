# FFmpeg Examples
- If FFmpeg is a part of a pipe, you have to organise the audio channel-mapping on your own. <br>Otherwise: Output order = Input Order
- Depending on the input, sometimes you have to use the "BL" & "BR" Channels instead "SL" & "SR". <br>But it doesn't matter if you use "...c3=SL+BL|c4=SR+BR...".
- If you lower the bitdepth from 24bit to 16bit, use dither.
- "[-map](https://trac.ffmpeg.org/wiki/Map)" let you select the required audiostream. <br>

#### 1. Extract a 16bit, 5.1 audiostream from a m2ts-File to 16bit WAV:
	"ffmpeg.exe" -i "input.m2ts" -c:a pcm_s16le -map 0:a:4 -ac 6 -rf64 auto "output.wav"

#### 2. Extract a 24bit, 7.1 audiostream from a m2ts-File to 24bit WAV:
	"ffmpeg.exe" -i "input.m2ts" -c:a pcm_s24le -map 0:a:2 -ac 8 -rf64 auto "output.wav"

#### 3. Extract a 24bit 192kHz, 7.1 audiostream from a m2ts-File to 16bit 48kHz WAV:
	"ffmpeg.exe" -i "input.m2ts" -c:a pcm_s16le -af "aresample=resampler=soxr:osr=48000:dither_method=shibata,aformat=sample_fmts=s16" -map 0:a:1 -ac 8 -rf64 auto "output.wav"

#### 4. Normalise a 5.1 audiofile to EBU R128:
**First Pass:** Analyze audiofile and write results down to "result.txt"

	"ffmpeg.exe -nostats -i "input.flac" -af loudnorm=I=-23:TP=-2:LRA=7:print_format=json -f null - > result.txt 2>&1"
Now replace the XXX-Values with the values from the result file. Because *loudnorm* resamples the audiostream up to 192kHz, we have to use a resampler to get back the original sample rate. In this example: 48kHz.

**Second Pass:**

	"ffmpeg.exe" -i "input.flac" -af loudnorm=I=-23:TP=-2:LRA=7:measured_I=XXX:measured_LRA=XXX:measured_tp=XXX:measured_thresh=XXX:offset=XXX,aresample=resampler=soxr -ar 48000 -ac 6 output.wav