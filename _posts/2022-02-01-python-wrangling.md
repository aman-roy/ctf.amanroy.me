---
layout: post
title: "picoCTF: Python Wrangling"
tags: picoCTF writeup general-skills
---


## [Info](#info)

Problem link - [picoCTF: Python Wrangling](https://play.picoctf.org/practice/challenge/166)


## [Solution](#solution)


There are three files involved in this problem. 

1. encrypted flag [`flag.txt.en`]
```
gAAAAABgUAIV8D5MJdzgLLTkkMlbU84ARVwfX4brMt2rJQCjkpLItytfWVZe1L2dp4K8VrKgRU3axStKJEAqcM0iDaxiYE54Boh8UfAAo1RNifKnlDrFz0gLaznVSFVj2xAUa4V35180
```
2. password [`pw.txt`]
```
68f88f9368f88f9368f88f9368f88f93
```
3. python script [`ende.py`]

We need to run the python script and pass the flag to it. Also, a password is needed to decrypt the flag.

The important piece of code in `ende.py` is  

```python
elif sys.argv[1] == "-d":
    if len(sys.argv) < 4:
        sim_sala_bim = input("Please enter the password:")
    else:
        sim_sala_bim = sys.argv[3]

    ssb_b64 = base64.b64encode(sim_sala_bim.encode())
    c = Fernet(ssb_b64)

    with open(sys.argv[2], "r") as f:
        data = f.read()
        data_c = c.decrypt(data.encode())
        sys.stdout.buffer.write(data_c)

```

After looking at the above code, we need to pass few arguments to decrypt the flag.

The format will be something like - 
```shell
python ende.py sys.argv[1] sys.argv[2] sys.argv[3]
```

Here we need to assign these values - 
```python
sys.argv[1] = '-d' # to make it enter inside the decryption block
sys.argv[2] = 'flag.txt.en' # with open(sys.argv[2], "r") uses it to get the cipher text
sys.argv[3] = '68f88f9368f88f9368f88f9368f88f93' # optional but it should be entered interactively if not passed 
```

To summarize, running this will spit out the flag - 
```shell
python ende.py -d flag.txt.en 68f88f9368f88f9368f88f9368f88f93
```

## [Flag](#flag)

Here is the flag - 
```
picoCTF{4p0110_1n_7h3_h0us3_68f88f93}
```
