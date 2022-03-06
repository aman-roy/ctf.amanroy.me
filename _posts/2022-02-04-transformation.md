---
layout: post
title: "picoCTF: Transformation"
tags: picoCTF writeup reverse-engineering
---

## [Info](#info)

Problem link - [picoCTF: Transformation](https://play.picoctf.org/practice/challenge/104)

## [Solution](#solution)

An encrypted algorithm is used to encrypt a text. We need to reverse engineer it.

Here is the algorithm - 

```python
''.join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, len(flag), 2)])
```

Here is the encrypted text - 
```
flag = "灩捯䍔䙻ㄶ形楴獟楮獴㌴摟潦弸彥㜰㍢㐸㙽"
```


To understand the encryption algorithm, first break it down into multiple lines.

```python
result = ""

for i in range(0, len(flag), 2):
	first_letter = ord(flag[i]) << 8
	second_letter = ord(flag[i + 1])
	final = chr(first+second)
	result+=final

print(result)
```

Looking at the above code, we understand - 

* Each letter in the cipher text, holds two letters of the plain text.
* The length of the flag is even number. (useless info!)

Few other things to keep in mind - 
* Flag consist of ASCII values which ranges from `(0 - 255)`. 
* To store the above value, we need 8 bits (at max!). 


Now, to explain the algorithm in short - 
> Each time two letters from the plain text is taken for encryption.
> They are getting converted into there integer value.
> Then one cipher text is getting generated, in which the first letter is saved at the first 8 bits.
> And the second letter is saved at the later 8 bit location.

We need to reverse engineer the above code. Basically, how to write the <i>f<sup>-1</sup>()</i> a?

Here is the final code - 

```python
flag = "灩捯䍔䙻ㄶ形楴獟楮獴㌴摟潦弸彥㜰㍢㐸㙽"

output = ""

for i in flag:
	first_letter = chr(ord(i)>>8)
	second_letter = chr(ord(i)&255)
	output += first_letter+second_letter

print(output)
``` 


## [Flag](#flag)

Here is the flag - 
```
picoCTF{16_bits_inst34d_of_8_e703b486}
```
