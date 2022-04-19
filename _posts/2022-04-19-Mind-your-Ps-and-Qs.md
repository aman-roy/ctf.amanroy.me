---
layout: post
title: "picoCTF: Mind your Ps and Qs"
tags: picoCTF writeup cryptography
---

## [Info](#info)

Problem link - [picoCTF: Mind your Ps and Qs](https://play.picoctf.org/practice/challenge/162)

## [Solution](#solution)

Three values are provided on the problem page -
```text
Decrypt my super sick RSA:
c: 421345306292040663864066688931456845278496274597031632020995583473619804626233684
n: 631371953793368771804570727896887140714495090919073481680274581226742748040342637
e: 65537
```

Here is a step-by-step guide for RSA - [https://www.cs.utexas.edu/~mitra/honors/soln.html](https://www.cs.utexas.edu/~mitra/honors/soln.html)

Our first task is to find factors for n. Using an online [tool](https://www.dcode.fr/prime-factors-decomposition), we have derived two factors `p` and 	`q`.
```
p: 1461849912200000206276283741896701133693
q: 431899300006243611356963607089521499045809
``` 

Using these values we can now compute `phi(n)`.
```
phi(n): (p-1)*(q-1) = 631371953793368771804570727896887140714061729769155038068711341335911329840163136
```

Next step is to compute `d` using above computed values. There is an easy way to do that using Python. We need to come up with multiplicative inverse of `d * e mod phi(n) = 1`
```python
d = pow(e, -1, phi_n)
```

Now using `d` which is in reality is a private key, we can decrypt the ciphertext `c`.
```python
plaintext = pow(c,d,n)
```

Convert it to ASCII and the flag will be revealed. 
```python
print(bytearray.fromhex(hex(plaintext)[2:]).decode())
```

To summarize, Here is the complete code - 
```python
# already given
c = 421345306292040663864066688931456845278496274597031632020995583473619804626233684
n = 631371953793368771804570727896887140714495090919073481680274581226742748040342637
e = 65537


# Find factors using - https://www.dcode.fr/prime-factors-decomposition
p = 1461849912200000206276283741896701133693
q = 431899300006243611356963607089521499045809

# compute 
phi_n = (p-1)*(q-1)
d = pow(e, -1, phi_n)
plaintext = pow(c,d,n)

# print
print(bytearray.fromhex(hex(plaintext)[2:]).decode())
```

## [Flag](#flag)

Here is the flag - 
```
picoCTF{sma11_N_n0_g0od_55304594}
```
