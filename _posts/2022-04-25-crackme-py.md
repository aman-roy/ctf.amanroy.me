---
layout: post
title: "picoCTF: crackme-py"
tags: picoCTF writeup reverse-engineering
---

## [Info](#info)

Problem link - [picoCTF: crackme-py](https://play.picoctf.org/practice/challenge/175)

## [Solution](#solution)

A python code is provided in the challenge. The secret is in encrypted form present in the code itself.
```python
bezos_cc_secret = "A:4@r%uL`M-^M0c0AbcM-MFE02fh3e4a5N"
```

Other than that, there are two functions `decode_secret` and `choose_greatest`. But, only `choose_greatest` is called. 

The `decode_secret` expects one `secret` argument which can decode the `secret` into plaintext. we have the function as well as the encrypted aka `secret` text. Let's pass it to the function and see what happens.

Add this at the end of the `crackme.py` file - 
```python
decode_secret(bezos_cc_secret)
``` 

And run the file using `python` to print out the flag - 
```shell
python crackme.py
```

## [Flag](#flag)

Here is the flag - 
```
picoCTF{1|\/|_4_p34|\|ut_a79b6c2d}
```
