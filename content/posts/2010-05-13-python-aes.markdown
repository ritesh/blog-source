---
date: 2010-05-13 18:59:00+00:00
slug: python-aes
title: Python AES
wordpress_id: 24
categories:
- aes
- encryption
- example
- python
---

Cryptography isn't available by default on python. One needs to install the [PyCrypto](http://www.dlitz.net/software/pycrypto/) toolkit to make it work. Looks something like this: 

```   
from Crypto.Cipher import AES   
import base64   
  
key = "x0Dx0Fx07x0Fx0Cx0Fx08x0Fx06x0Fx09x0Fx06x0Fx03x0Fx0Fx06x0Fx0Fx0Fx0Fx0Fx0Fx06x0Fx09x0Fx06x0Fx03x0F"   
objAes=AES.new(key, AES.MODE_ECB)   
plaintext='This is sparta!!'   
#looks nicer with base64   
enc = objAes.encrypt(plaintext)   
print "Encrypted: " + base64.b64encode(enc)   
print "Decrypted: " + objAes.decrypt(enc)   
```