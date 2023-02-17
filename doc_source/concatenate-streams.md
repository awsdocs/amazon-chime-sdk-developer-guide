# Concatenating data streams<a name="concatenate-streams"></a>

**Note**  
To automate the process of concatenating media capture artifacts, refer to [Creating media concatenation pipelines](create-concat-pipe.md) in this guide\.

This example uses ffmpeg to concatenate video or audio files into a single mp4 file\. First, create a filelist\.txt file that contains all the input files\. Use this format: 

```
file 'input1.mp4'
file 'input2.mp4'
file 'input3.mp4'
```

Next, use this command to concatenate the input file:

```
ffmpeg -f concat -i filelist.txt -c copy output.mp4
```

For more information about media concatenation pipelines, refer to [Creating media concatenation pipelines](create-concat-pipe.md) in this guide\.