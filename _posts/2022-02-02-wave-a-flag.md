---
layout: post
title: "picoCTF: Wave a flag"
tags: picoCTF writeup general-skills
---


## [Info](#info)

Problem link - [picoCTF: Wave a flag](https://play.picoctf.org/practice/challenge/170)


## [Solution](#solution)

We need to run the executable[`warm`] to see what happens. But before running, make it executable - 

```shell
chmod +x warm
```

```shell
./warm
Hello user! Pass me a -h to learn what I can do!
```

Now let's pass `-h` flag.
```text
./warm -h
Oh, help? I actually don't do much, but I do have this flag here: picoCTF{b1scu1ts_4nd_gr4vy_6635aa47}

```


## [Flag](#flag)

Here is the flag - 
```
picoCTF{b1scu1ts_4nd_gr4vy_6635aa47}
```
