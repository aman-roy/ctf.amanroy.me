---
layout: post
title: "picoCTF: Matryoshka doll"
tags: picoCTF writeup forensics
---

## [Info](#info)

Problem link - [picoCTF: Matryoshka doll](https://play.picoctf.org/practice/challenge/129)

## [Solution](#solution)

There is one image file. Opening the image file, nothing looks suspicious, but we need to find the flag.

Let's analyze it with `binwalk` tool to see if there is something hidden behind the image. 

```shell
binwalk dolls.jpg
```

Here is the output - 
```
0             0x0             PNG image, 594 x 1104, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
272492        0x4286C         Zip archive data, at least v2.0 to extract, compressed size: 378954, uncompressed size: 383938, name: base_images/2_c.jpg
651612        0x9F15C         End of Zip archive, footer length: 22
```

So, there is something fishy behind this image file. Let's exctract the files.
```shell
binwalk -e dolls.jpg
cd _dolls.jpg.extracted/
ls
```

Here is the output - 
```shell
4286C.zip  base_images
```

Let's exctract the zip file - 
```shell
unzip 4286C.zip
```

It will asking permission for overwriting image inside `base_image`, allow it. Let's `cd` inside the `base_image` and look for the flag. 

Here again, there is one image file. The above process needs to be repeated till we find the image file - 
```shell
# second iteration
cd base_images
binwalk -e 2_c.jpg
cd _2_c.jpg.extracted
unzip 2DD3B.zip

# third iteration
cd base_images
binwalk -e 3_c.jpg
cd _3_c.jpg.extracted
unzip 1E2D6.zip

# forth iteration
cd base_images
binwalk -e 4_c.jpg
cd _4_c.jpg.extracted
```

After the forth iteration, we can spot a `flag.txt` file, if we `ls` in this directory.
```shell
ls      # output - 136DA.zip  flag.txt
cat flag.txt
```


## [Flag](#flag)

Here is the flag - 
```
picoCTF{bf6acf878dcbd752f4721e41b1b1b66b}
```
