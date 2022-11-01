# babyrsa
## Crypto (50 pts)

Writeup by danlliu (WolvSec)

## Challenge

We are given an encrypted file, as well as a public key file.

## Solution

Let's first check out the public key:

```
❯ openssl rsa -pubin -in key.pem -text
Public-Key: (4096 bit)
Modulus:
    00:9c:86:30:a8:b1:c8:5a:db:58:45:3b:8a:2f:fd:
    df:a6:b2:6d:c6:ca:34:e1:f5:b5:62:78:7a:e8:3e:
    3c:d4:27:b3:01:02:b4:b5:d7:a1:dd:f3:77:38:5d:
    5c:f2:92:40:52:95:ba:30:42:85:b3:b9:1b:5a:56:
    50:52:c2:be:3e:1a:1c:3f:68:29:28:04:a0:75:99:
    6e:19:90:a8:bc:60:ee:39:60:54:77:ed:3d:5f:61:
    76:1b:8b:73:ef:d3:e7:fe:48:11:3a:10:86:bd:c9:
    fc:55:e7:c6:f7:21:a9:02:e5:8e:98:9c:a2:05:77:
    ef:bb:c1:88:82:3f:b7:bb:26:b6:a1:88:a2:7b:66:
    00:b1:72:a8:6b:d0:02:a1:d5:ca:6e:37:f9:77:18:
    c0:c0:27:35:46:40:8a:31:31:18:7b:e6:27:0c:2b:
    98:7f:e1:88:30:9a:e5:b5:9d:ea:66:24:9b:7a:b0:
    76:3a:ec:8e:c0:b1:c0:45:ec:77:92:cd:34:ab:a2:
    d3:d7:72:18:7b:4e:62:7e:59:90:30:df:48:c5:e8:
    e4:e5:c3:b8:5f:13:07:ab:0b:6a:39:88:f8:8e:0a:
    3f:a7:2f:6a:7e:09:11:3e:9d:7b:74:25:cf:f8:e6:
    bb:55:d1:9e:ee:c8:7b:b0:a5:71:d8:ff:da:9c:d1:
    4f:c3:ef:37:be:9a:1e:c4:68:ba:ea:cf:06:78:20:
    75:db:0e:4a:58:a2:4b:5e:2e:92:93:ab:10:f8:26:
    59:8a:d2:ab:b0:d0:0b:c0:2b:1a:9a:1a:53:eb:a1:
    13:9d:f6:a9:b4:39:ba:55:55:f7:dc:a0:8e:2b:13:
    64:69:df:aa:76:3e:7c:af:f1:b3:90:11:c8:24:3e:
    05:b1:38:c7:70:5e:06:96:12:97:b3:ac:4f:79:a2:
    62:9a:80:5b:53:5e:cc:27:c7:77:3f:04:d7:7b:a8:
    9d:d8:a0:d0:fa:ec:14:03:7a:99:28:e4:e9:c3:8b:
    f9:d3:c4:b3:16:0e:2f:93:1f:db:6c:4c:26:60:b4:
    19:cd:ea:b5:5d:55:4f:38:18:b6:f7:0b:a2:0e:7e:
    1b:b5:ed:1f:9c:f0:56:c0:c6:b6:60:b9:1c:3c:e2:
    03:f6:e5:c4:03:01:46:68:7c:02:a6:d1:1d:b2:68:
    97:6d:59:a3:de:78:8c:a4:9b:a0:e2:ea:fb:4a:ff:
    80:5e:3b:d3:fc:da:2e:7c:47:5e:ec:fd:cf:63:f4:
    c1:f7:64:e3:ba:06:aa:9e:f0:af:2a:57:f0:e0:be:
    e7:f3:d8:70:4c:3e:c5:e2:9c:98:d0:5b:ad:5a:51:
    8b:5c:aa:16:e6:2e:40:1c:7e:47:55:14:cb:9e:75:
    19:e0:53
Exponent: 3 (0x3)
writing RSA key
-----BEGIN PUBLIC KEY-----
MIICIDANBgkqhkiG9w0BAQEFAAOCAg0AMIICCAKCAgEAnIYwqLHIWttYRTuKL/3f
prJtxso04fW1Ynh66D481CezAQK0tdeh3fN3OF1c8pJAUpW6MEKFs7kbWlZQUsK+
PhocP2gpKASgdZluGZCovGDuOWBUd+09X2F2G4tz79Pn/kgROhCGvcn8VefG9yGp
AuWOmJyiBXfvu8GIgj+3uya2oYiie2YAsXKoa9ACodXKbjf5dxjAwCc1RkCKMTEY
e+YnDCuYf+GIMJrltZ3qZiSberB2OuyOwLHARex3ks00q6LT13IYe05iflmQMN9I
xejk5cO4XxMHqwtqOYj4jgo/py9qfgkRPp17dCXP+Oa7VdGe7sh7sKVx2P/anNFP
w+83vpoexGi66s8GeCB12w5KWKJLXi6Sk6sQ+CZZitKrsNALwCsamhpT66ETnfap
tDm6VVX33KCOKxNkad+qdj58r/GzkBHIJD4FsTjHcF4GlhKXs6xPeaJimoBbU17M
J8d3PwTXe6id2KDQ+uwUA3qZKOTpw4v508SzFg4vkx/bbEwmYLQZzeq1XVVPOBi2
9wuiDn4bte0fnPBWwMa2YLkcPOID9uXEAwFGaHwCptEdsmiXbVmj3niMpJug4ur7
Sv+AXjvT/NoufEde7P3PY/TB92TjugaqnvCvKlfw4L7n89hwTD7F4pyY0FutWlGL
XKoW5i5AHH5HVRTLnnUZ4FMCAQM=
-----END PUBLIC KEY-----
```

We see here that the exponent is `3`! Thus, we can perform a **small-exponent** attack, where instead of trying to factor `N` (which is hard), we can just take the cube root of the ciphertext to get the plaintext!

```python
from Crypto.Util.number import long_to_bytes, bytes_to_long
import gmpy2

n = """009c8630a8b1c85adb58453b8a2ffd
dfa6b26dc6ca34e1f5b562787ae83e
3cd427b30102b4b5d7a1ddf377385d
5cf292405295ba304285b3b91b5a56
5052c2be3e1a1c3f68292804a07599
6e1990a8bc60ee39605477ed3d5f61
761b8b73efd3e7fe48113a1086bdc9
fc55e7c6f721a902e58e989ca20577
efbbc188823fb7bb26b6a188a27b66
00b172a86bd002a1d5ca6e37f97718
c0c0273546408a3131187be6270c2b
987fe188309ae5b59dea66249b7ab0
763aec8ec0b1c045ec7792cd34aba2
d3d772187b4e627e599030df48c5e8
e4e5c3b85f1307ab0b6a3988f88e0a
3fa72f6a7e09113e9d7b7425cff8e6
bb55d19eeec87bb0a571d8ffda9cd1
4fc3ef37be9a1ec468baeacf067820
75db0e4a58a24b5e2e9293ab10f826
598ad2abb0d00bc02b1a9a1a53eba1
139df6a9b439ba5555f7dca08e2b13
6469dfaa763e7caff1b39011c8243e
05b138c7705e06961297b3ac4f79a2
629a805b535ecc27c7773f04d77ba8
9dd8a0d0faec14037a9928e4e9c38b
f9d3c4b3160e2f931fdb6c4c2660b4
19cdeab55d554f3818b6f70ba20e7e
1bb5ed1f9cf056c0c6b660b91c3ce2
03f6e5c4030146687c02a6d11db268
976d59a3de788ca49ba0e2eafb4aff
805e3bd3fcda2e7c475eecfdcf63f4
c1f764e3ba06aa9ef0af2a57f0e0be
e7f3d8704c3ec5e29c98d05bad5a51
8b5caa16e62e401c7e475514cb9e75
19e053"""

n = n.replace('\n', '')
print(n)
n = bytes_to_long(bytes.fromhex(n))

with open('out.txt', 'r') as f:
  ct = f.read()

ct = bytes_to_long(bytes.fromhex(ct))
c = gmpy2.mpz(ct)

r = gmpy2.iroot(c, 3)
while not r[1]:
  c += n
  r = gmpy2.iroot(c, 3)
  print(r)

print(r)

pt = 48117239019789625284185298265087555527408631488673632169280752836852590016893
print(long_to_bytes(pt))
```

From here, we can just run the script, and convert the resulting number to a string.

```
009c8630a8b1c85adb58453b8a2ffddfa6b26dc6ca34e1f5b562787ae83e3cd427b30102b4b5d7a1ddf377385d5cf292405295ba304285b3b91b5a565052c2be3e1a1c3f68292804a075996e1990a8bc60ee39605477ed3d5f61761b8b73efd3e7fe48113a1086bdc9fc55e7c6f721a902e58e989ca20577efbbc188823fb7bb26b6a188a27b6600b172a86bd002a1d5ca6e37f97718c0c0273546408a3131187be6270c2b987fe188309ae5b59dea66249b7ab0763aec8ec0b1c045ec7792cd34aba2d3d772187b4e627e599030df48c5e8e4e5c3b85f1307ab0b6a3988f88e0a3fa72f6a7e09113e9d7b7425cff8e6bb55d19eeec87bb0a571d8ffda9cd14fc3ef37be9a1ec468baeacf06782075db0e4a58a24b5e2e9293ab10f826598ad2abb0d00bc02b1a9a1a53eba1139df6a9b439ba5555f7dca08e2b136469dfaa763e7caff1b39011c8243e05b138c7705e06961297b3ac4f79a2629a805b535ecc27c7773f04d77ba89dd8a0d0faec14037a9928e4e9c38bf9d3c4b3160e2f931fdb6c4c2660b419cdeab55d554f3818b6f70ba20e7e1bb5ed1f9cf056c0c6b660b91c3ce203f6e5c4030146687c02a6d11db268976d59a3de788ca49ba0e2eafb4aff805e3bd3fcda2e7c475eecfdcf63f4c1f764e3ba06aa9ef0af2a57f0e0bee7f3d8704c3ec5e29c98d05bad5a518b5caa16e62e401c7e475514cb9e7519e053
(mpz(48117239019789625284185298265087555527408631488673632169280752836852590016893), True)
b'jadeCTF{rs4_1s_s0_fr3ak1ng_3asy}'
```
