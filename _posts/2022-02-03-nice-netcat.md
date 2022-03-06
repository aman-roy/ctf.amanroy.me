---
layout: post
title: "picoCTF: Nice netcat..."
tags: picoCTF writeup general-skills
---

## [Info](#info)

Problem link - [picoCTF: Python Wrangling](https://play.picoctf.org/practice/challenge/156)

## [Solution](#solution)

Let's go straight connecting to the `mercury.picoctf.net` at port `49039` and see what happens.
```
nc mercury.picoctf.net 49039
```

Running the above command gives you bunch of numbers and closes the connection.
```
112 
105 
99 
111 
67 
.
.
.
56 
125 
10 
```

We can easily spot that `112` is the ASCII value for `p` and `105` translates to `i`. 

Now we need to convert these bunch of numbers to its `char` value and print it. Doing that by hand can be tedious. Let's use python to make it swift.

At first, we need to fetch these numbers from the server and save it in a list.
```python
import socket

TCP_IP = 'mercury.picoctf.net'
TCP_PORT = 49039

def get_data():
	s = socket.socket(socket.AF_INET, 
		socket.SOCK_STREAM)
	s.connect((TCP_IP, TCP_PORT))
	data = s.recv(1024).decode("utf-8")
	s.close()
	
	data = data.replace("\n", "").split()
	return list(map(int, data))
```

Now, need to convert these ASCII values to its char value.

```python
def int_to_ascii(int_list):
	final = ""
	for i in int_list:
		final += chr(i)
	return final
```

The complete code will look something like this - 
```python
import socket

TCP_IP = 'mercury.picoctf.net'
TCP_PORT = 49039

def get_data():
	s = socket.socket(socket.AF_INET, 
		socket.SOCK_STREAM)
	s.connect((TCP_IP, TCP_PORT))
	data = s.recv(1024).decode("utf-8")
	s.close()
	
	data = data.replace("\n", "").split()
	return list(map(int, data))

def int_to_ascii(int_list):
	final = ""
	for i in int_list:
		final += chr(i)
	return final

# Prints the flag
print(int_to_ascii(get_data()))
```

## [Flag](#flag)

Here is the flag - 
```
picoCTF{g00d_k1tty!_n1c3_k1tty!_3d84edc8}
```
