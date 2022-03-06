---
layout: post
title: "picoCTF: Information"
tags: picoCTF writeup forensics
---


## [Info](#info)

Problem link - [picoCTF: Information](https://play.picoctf.org/practice/challenge/186)


## [Solution](#solution)

A `cat.jpg` file is given. By the looks of it, nothing seems interesting. But there are things beyond data, i.e. metadata.

we can check the metadata of the given image file using `exiftool`.
```shell
❯ exiftool cat.jpg
ExifTool Version Number         : 12.16
File Name                       : cat.jpg
Directory                       : .
File Size                       : 858 KiB
File Modification Date/Time     : 2021:12:25 20:38:22+01:00
File Access Date/Time           : 2021:12:25 20:38:21+01:00
File Inode Change Date/Time     : 2021:12:25 20:45:24+01:00
File Permissions                : rwxrwxr-x
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.02
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Current IPTC Digest             : 7a78f3d9cfb1ce42ab5a3aa30573d617
Copyright Notice                : PicoCTF
Application Record Version      : 4
XMP Toolkit                     : Image::ExifTool 10.80
License                         : cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9
Rights                          : PicoCTF
Image Width                     : 2560
Image Height                    : 1598
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 2560x1598
Megapixels                      : 4.1
```

Here the interesting bit is `License` field. 
```shell
License: cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9
```

This value is `base64` encoded. Let's convert it in `ASCII` that will give us the flag.
```shell
echo cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9 | base64 --decode
```

## [Flag](#flag)

Here is the flag -
```
picoCTF{the_m3tadata_1s_modified}
```
