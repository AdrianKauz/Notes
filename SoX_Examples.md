
# SoX Examples
#### 1.Change pitch of a WAV-file and save it as FLAC with compression level 8:
	"sox.exe" --multi-threaded --input-buffer 32768 -t wav "input.wav" -t flac -C 8 "output.flac" pitch 45 120 30 24
This example works very well for correcting the german audio tracks on some Blu-Ray releases, if the voices are to deep.

pitch **[shift]** _[segment] [search] [overlap]_:  
* **[shift]** A positive or negative value in 'cents' (100ths of a semitone)
* [segment] Optional: Selects the algorithm’s segment size in milliseconds.
* [search] Optional: Gives the audio length in milliseconds over which the algorithm will search for overlapping points.
* [overlap] Optional: Gives the segment overlap length in milliseconds.