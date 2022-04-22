---
layout: post
title: "picoCTF: Static ain't always noise"
tags: picoCTF writeup general-skills
---

## [Info](#info)

Problem link - [picoCTF: Static ain't always noise](https://play.picoctf.org/practice/challenge/163)

## [Solution](#solution)


Two files are given - 
```
ltdis.sh    # Shell script
static      # Executable
```

Let's try to run the executable(`static`) first. Here is the output - 
```
Oh hai! Wait what? A flag? Yes, it's around here somewhere!
```

So, where is the flag???

Let's run the shell script. This shell script expects one executable as an argument. (look inside the file to know that)
```shell
sh ltdis.sh static
```

Running the above command will generate two other files - 
```
static.ltdis.strings.txt  
static.ltdis.x86_64.txt
```

If you go through the files and look for the flags, you can spot it. Another way to find the flag is to use `grep` tool.
```shell
cat static.ltdis.strings.txt static.ltdis.x86_64.txt | grep pico
```

## [Flag](#flag)

Here is the flag - 
```
picoCTF{sma11_N_n0_g0od_55304594}
```
