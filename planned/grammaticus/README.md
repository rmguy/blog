FFMPEG commands 
```
ffmpeg -i ~/src/github.com/bprashanth/blog/planned/grammaticus/assets/moon_practical_2.mp4 \
-f lavfi -i "anullsrc=channel_layout=mono:sample_rate=22050:d=30" \
-filter_complex "[0:v]setpts=1.5*PTS,scale=208:176:force_original_aspect_ratio=increase,crop=208:176,setsar=1,format=yuv420p,tpad=stop_mode=add:stop_duration=1[v]" \
-map "[v]" -map "1:a" \
-c:v amv -r 21 -c:a adpcm_ima_amv -block_size 1050 \
-f amv -shortest -write_index 0 -map_metadata -1 -fflags +bitexact \
-max_interleave_delta 1 PRACTICAL_2.AMV

ffmpeg -i craters1.mp4 \
-filter_complex "[0:v]setpts=0.125*PTS,scale=208:176:force_original_aspect_ratio=increase,crop=208:176,setsar=1,format=yuv420p,tpad=stop_mode=add:stop_duration=2[v];[0:a]atempo=2.0,atempo=2.0,atempo=2.0[a]" \
-map "[v]" -map "[a]" \
-c:v amv -r 21 -c:a adpcm_ima_amv -ar 22050 -ac 1 -block_size 1050 \
-f amv -t 12 -write_index 0 -map_metadata -1 -fflags +bitexact \
CRATERS1.AMV
```
