---
layout: post
title: "picoCTF: keygenme-py"
tags: picoCTF writeup reverse-engineering
---

## [Info](#info)

Problem link - [picoCTF: Tab, Tab, Attack](https://play.picoctf.org/practice/challenge/121)

## [Solution](#solution)

A python program is provided for which we need to find the license key. Going through the whole code is quite frustrating and can look daunting. 

Here are some of the important bits - 
```python
...
...
...

username_trial = "ANDERSON"
bUsername_trial = b"ANDERSON"

key_part_static1_trial = "picoCTF{1n_7h3_|<3y_of_"
key_part_dynamic1_trial = "xxxxxxxx"
key_part_static2_trial = "}"
key_full_template_trial = key_part_static1_trial + key_part_dynamic1_trial + key_part_static2_trial

...
...
...


def enter_license():
    user_key = input("\nEnter your license key: ")
    user_key = user_key.strip()

    global bUsername_trial
    
    if check_key(user_key, bUsername_trial):
        decrypt_full_version(user_key)
    else:
        print("\nKey is NOT VALID. Check your data entry.\n\n")


def check_key(key, username_trial):

    global key_full_template_trial

    if len(key) != len(key_full_template_trial):
        return False
    else:
        # Check static base key part --v
        i = 0
        for c in key_part_static1_trial:
            if key[i] != c:
                return False

            i += 1

        # TODO : test performance on toolbox container
        # Check dynamic part --v
        if key[i] != hashlib.sha256(username_trial).hexdigest()[4]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[5]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[3]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[6]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[2]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[7]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[1]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[8]:
            return False



        return True

```

To summarize the above part, we need to input a key so that we can decrypt the full version. 

First, look in the `enter_license` function -

```python
if check_key(user_key, bUsername_trial):
	decrypt_full_version(user_key)
```
Here, `check_key` function is being called with two arguments. `user_key` is the string input by the user and `bUsername_trial = b"ANDERSON"`

### First check

Now, let's dive into `check_key` function.
```python
# key = user_key
# key_full_template_trial = "picoCTF{1n_7h3_|<3y_of_xxxxxxxx}"

if len(key) != len(key_full_template_trial):
	return False
```

Here the input string's length should be equal to `key_full_template_trial` variable. If we make `key = key_full_template_trial`, the length will be equal by default.

So, for now - 
```python
key = "picoCTF{1n_7h3_|<3y_of_xxxxxxxx}"
```

### Second check

Here is the second check - 

```py
# key_part_static1_trial = "picoCTF{1n_7h3_|<3y_of_"

i = 0
for c in key_part_static1_trial:
    if key[i] != c:
        return False

    i += 1
```

This part checks that the input string(key) starts with `picoCTF{1n_7h3_|<3y_of_` or not. If it doesn't, return `False` otherwise move to the next bit.

Our key starts with `key_part_static1_trial` string, so no change is needed. By the end of the second check, our key is - 
```python
key = "picoCTF{1n_7h3_|<3y_of_xxxxxxxx}"
```

### Third check

Now comes the final check - 

```python
if key[i] != hashlib.sha256(username_trial).hexdigest()[4]:
    return False
else:
    i += 1

if key[i] != hashlib.sha256(username_trial).hexdigest()[5]:
    return False
else:
    i += 1

if key[i] != hashlib.sha256(username_trial).hexdigest()[3]:
    return False
else:
    i += 1

if key[i] != hashlib.sha256(username_trial).hexdigest()[6]:
    return False
else:
    i += 1

if key[i] != hashlib.sha256(username_trial).hexdigest()[2]:
    return False
else:
    i += 1

if key[i] != hashlib.sha256(username_trial).hexdigest()[7]:
    return False
else:
    i += 1

if key[i] != hashlib.sha256(username_trial).hexdigest()[1]:
    return False
else:
    i += 1

if key[i] != hashlib.sha256(username_trial).hexdigest()[8]:
    return False



return True
```

There are bunch of `if-conditions` to check string past `picoCTF{1n_7h3_|<3y_of_` (bunch of xxxx...). Let's look at one `if-condition` because all the rest of `if-conditions` are replica of the same.

```python
# username_trial = b"ANDERSON"

if key[i] != hashlib.sha256(username_trial).hexdigest()[4]:
    return False
```

The `4th` index of hash of string `username_trial` should replace the first `x` in the key. Likewise `5`, `3`, `6`, `2`, `7`, `1`, `8` should replace the rest of `x`. To calculate that, we can use few lines of python code - 

```python
import hashlib

keys = [4,5,3,6,2,7,1,8]
username_trial =  b"ANDERSON"
username_hash = hashlib.sha256(username_trial).hexdigest()

final_keys = ""

for key in keys:
	final_keys+=username_hash[key]

print(final_keys)

# output - 01582419
```

We need to replace this number with series of `xxx...` in key and we are done. 
```
key = "picoCTF{1n_7h3_|<3y_of_01582419}"
```


## [Flag](#flag)

Here is the flag - 
```
picoCTF{1n_7h3_|<3y_of_01582419}
```
