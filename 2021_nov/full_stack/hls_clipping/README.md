# HLS Clipping (FFMPEG Internals Replica)

## Evaluative Goals
The goal of this task is to evaluate how you break down the problem, including its edge cases, using clean, well-structured, and documented code. Hence, you may use any language of your choice.

## Recommended Prerequisite Reading
- m3u8 file format
- FFmpeg

## Problem Description

To design a script that can be executed from the command line that inputs a .m3u8 and outputs a clipped version .m3u8 from a specified `Start Time` and `End Time`.

**Use Case Scenario**: If a user needs to create a 1 min highlight(e.g goal) from 90 min football match stream. 

### Approach

FFMPEG does support this natively. You may run the below command on your machine to get a better sense of the output it generates in `./out`

```bash
# -ss -> start time of the clip in seconds
# -to -> end time of the clip in seconds
# Note: ignore protocol_whitelist arg, its only for ffmpeg
ffmpeg -protocol_whitelist https,file,tls,tcp -i /some/path/original.m3u8 -ss 12 -to 34 ./out/out.m3u8
```

But, to meet the `Evaluative Goals` of this task, you need to design a script(any language) that has the same behavior as the above FFmpeg example.

```bash
# e.g submission script if you were to use node

# -ss -> start time of the clip in seconds
# -to -> end time of the clip in seconds
node my_script.js -i /some/path/original.m3u8 -ss 12 -to 34 ./out/out.m3u8

```

The script you design will analyze the input times(`-ss` and `-to`), select the appropriate .ts segments, chop them if necessary, and then generate an m3u8 that contains the desired clip.

### Procedure
- Read `-ss`, `-to`, and the output path as specified from the command line to your script.
- `#EXTINF:[segment-lenght]` lines in the m3u8 file determine the length of each segment.
- Read m3u8 text file line by line and download the required .ts segments as determined by `-ss` and `-to`
- Chop(using ffmepg) the boundary ts segments as needed (see Input -> Output Example section below)
- Generate the output m3u8 file line by line giving it the relative paths of the downloaded/chopped .ts segments.
- You can use the `FFprobe` command to determine the exact length of the chopped boundary segments and use it as the `#EXTINF` of the boundary .ts segments in the output m3u8 you are generating.
- Play the m3u8 in VLC and verify the clip is correct as specified by `-ss` and `-to`.


## Sample M3u8 (for testing)
- [basketball sample](https://fhp-news-bucket.s3.amazonaws.com/sv_task/index_1.m3u8). This can be downloaded and played in VLC.


## Input -> Output Example
[reference image](./images/input_output_sample.png)(based on Sample Testing M3u8 provided above)

- This is an example with `-ss 37` and `-to 72`.
- Notice that the output m3u8 does not have absolute URLs for the .ts segments. This means the .ts segment paths are relative to the path of the output m3u8 locally.
- Red lines indicate that the original .ts segments were chopped, as indicated by the `#EXTINF` for these segments since their length is less than the original 10.01000 seconds. These are the boundary .ts segments that are determined by `-ss` and `-to.`
- White lines indicate that these are just copies of the original ts segments since they do not lie on the boundary.

## Submission & Help

While it's great if you can solve the task by yourself, we highly value effective communication if you have issues, doubts, etc. 

Feel free to reach out to us via email:
- `TO` - Luis Gomes (luis@freehitnews.com)
- `CC` - Sidhhant Agarwal (sidhhant@sportvot.com)


To submit your work, you may create private a GitHub repo and add `gomesLuis479SV` as a collaborator.

### All the best. 😃



