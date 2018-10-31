
# FFmpeg and Opus Audio Codec (16bit, 48kHz)
- Opus is a 48kHz-only format. Other input-samplerates will be internaly converted, using an internal *Speex* resampler.
-  Input bitdepth for Opus is 16bit.
- FFmpeg has an [internal](https://www.ffmpeg.org/ffmpeg-codecs.html#opus) Opus encoder. You can use this if you want.
- If FFmpeg is a part of a pipe, you have to organise the audio channel-mapping on your own. <br>Otherwise: Output order = Input Order
- Depending on the input, sometimes you have to use the "BL" & "BR" Channels instead "SL" & "SR". <br>But it doesn't matter if you use "...c3=SL+BL|c4=SR+BR...".
- Channel Description:
  - **FL**: Front Left, **FC**: Front Center, **FR**: Front Right
  - **SL**: Side Left, **SR**: Side Right
  - **BL**: Back Left, **BR**: Back Right (Also known as "Rear" instead of "Back") 
  - **SL**: Side Left, **SR**: Side Right
  - **LFE**: Low Frequency Effects
- Channel Mappings
    - 1 channel: monophonic (mono).
    - 2 channels: stereo (FL, FR).
    - 3 channels: linear surround (FL, FC, FR).
    - 4 channels: quadraphonic (FL, FR | BL, BR).
    - 5 channels: 5.0 surround (FL, FC, FR | BL, BR).
    - 6 channels: 5.1 surround (FL, FC, FR | BL, BR | LFE).
    - 7 channels: 6.1 surround (FL, FC, FR | SL, SR | BC | LFE).
    - 8 channels: 7.1 surround (FL, FC, FR | SL, SR | BL, BR | LFE).
  - [Source](https://tools.ietf.org/html/rfc7845#section-5.1.1.2)

#### 1. Convert a 16bit 5.1 audiofile to ~512kbps Opus with "opusenc.exe":
	"ffmpeg.exe" -i "input.ac3" -af "pan=0x3f|c0=FL|c1=FC|c2=FR|c3=SL+BL|c4=SR+BR|c5=LFE" -f s16le -ac 6 - | "opusenc.exe" - --raw --raw-chan 6 --raw-bits 16 --vbr --bitrate 512 --comp 10 --expect-loss 0 "output.opus"

#### 2. Convert a 24bit 5.1 audiofile to ~512kbps Opus with "opusenc.exe":
	"ffmpeg.exe" -i "input.dts" -af "aresample=resampler=soxr:dither_method=shibata,aformat=sample_fmts=s16,pan=0x3f|c0=FL|c1=FC|c2=FR|c3=SL+BL|c4=SR+BR|c5=LFE" -f s16le -ac 6 - | "opusenc.exe" - --raw --raw-chan 6 --raw-bits 16 --vbr --bitrate 512 --comp 10 --expect-loss 0 "output.opus"

#### 3. Convert a 24bit 6.1 audiofile to ~512kbps Opus with "opusenc.exe":
	"ffmpeg.exe" -i "input.dts" -af "aresample=resampler=soxr:dither_method=shibata,aformat=sample_fmts=s16,pan=6.1(back)|c0=FL|c1=FC|c2=FR|c3=SL+BL|c4=SR+BR|c5=BC|c6=LFE" -f s16le -ac 7 - | "opusenc.exe" - --raw --raw-chan 7 --raw-bits 16 --vbr --bitrate 512 --comp 10 --expect-loss 0 "output.opus"

#### 4. Convert a 24bit 7.1 audiofile to ~512kbps Opus with "opusenc.exe":
	"ffmpeg.exe" -i "input.dts" -af "aresample=resampler=soxr:dither_method=shibata,aformat=sample_fmts=s16,pan=0x63f|c0=FL|c1=FC|c2=FR|c3=SL|c4=SR|c5=BL|c6=BR|c7=LFE" -f s16le -ac 8 - | "opusenc.exe" - --raw --raw-chan 8 --raw-bits 16 --vbr --bitrate 512 --comp 10 --expect-loss 0 "output.opus"