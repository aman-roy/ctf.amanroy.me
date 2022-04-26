---
layout: post
title: "picoCTF: Magikarp Ground Mission"
tags: picoCTF writeup general-skills
---

## [Info](#info)

Problem link - [picoCTF: Magikarp Ground Mission](https://play.picoctf.org/practice/challenge/189)

## [Solution](#solution)

It is more of a `ssh` hands-on guide than a CTF challnge. 

First click on Run instance and `ssh` into the remote machine - 
```
ssh ctf-player@venus.picoctf.net -p 57385
```
Enter the provided password (`6dee9772`).

Now `ls` into the remote machine as instructed - 
```sh
ls 

# output
# 1of3.flag.txt instructions-to-2of3.txt 
```

To get the first part of the flag - 
```shell
cat 1of3.flag.txt

# output
# picoCTF{xxsh_
```

Get the second clue and follow it - 
```shell
cat instructions-to-2of3.txt 

# output
# Next, go to the root of all things, more succinctly `/`

cd /
ls

# output
# 2of3.flag.txt  dev   instructions-to-3of3.txt ...
```

Let's print the second part of the flag - 
```shell
cat 2of3.flag.txt

# output
# 0ut_0f_\/\/4t3r_
```

Get the third and final clue and follow it - 
```shell
cat instructions-to-3of3.txt 

# output
# Lastly, ctf-player, go home... more succinctly `~`

cd ~
ls

# output
# 3of3.flag.txt  drop-in
```

Print the last part of the flag - 
```shell
cat 3of3.flag.txt 

# output
# 540e4e79}
```

Now, combine all the three parts to get the final flag.

## [Flag](#flag)

Here is the flag - 
```
picoCTF{xxsh_0ut_0f_\/\/4t3r_540e4e79}
```
