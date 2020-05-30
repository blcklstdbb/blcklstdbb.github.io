---
layout: single
title: Cryptopals Set 1 Walkthrough - Challenge 1
date: 2020-5-23
classes: wide
excerpt: "My take on the Cryptopals Challenge"
header:
  teaser: /assets/images/crypto/cryptopals.jpg
tags:
  - Cryptography
  - CTF
---

## The Challenge

The first challenge involves converting the following hex string to base64:

`49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d`

The result should be:

`SSdtIGtpbGxpbmcgeW91ciBicmFpbiBsaWtlIGEgcG9pc29ub3VzIG11c2hyb29t`

With an important clue of "Always operate on raw bytes, never on encoded strings." Which of course I didn't comprehend until _after_ using `b''` and I even used `.encode(utf-8)`. When I used either function it did not convert the string to raw bytes it just added a b' in front of the string `b"49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d"`. I got a different output when I switched to the `bytes.fromhex()` method after a few minutes on StackOverflow and [Python Doc](https://docs.python.org/3/library/stdtypes.html#bytes.fromhex) page on the method.

```
>>> bytes.fromhex('49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d')
b"I'm killing your brain like a poisonous mushroom"
```

![](/assets/images/memes/meme-aww-yeah.jpg)

Now we're getting somewhere! After that I slapped together this piece of code:

```
import base64

hex_str = "49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d"

byte_str = bytes.fromhex(hex_str)  # b"I'm killing your brain like a poisonous mushroom"
base64_str = base64.b64encode(byte_str)  # {bytes} b'SSdtIGtpbGxpbmcgeW91ciBicmFpbiBsaWtlIGEgcG9pc29ub3VzIG11c2hyb29t'
ascii_string = base64_str.decode("ASCII")  # {str} 'SSdtIGtpbGxpbmcgeW91ciBicmFpbiBsaWtlIGEgcG9pc29ub3VzIG11c2hyb29t'
print(ascii_string)
```
