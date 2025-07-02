---
layout: post
title: "Unicode vs Ascii"
date: 2024-12-02 19:43:00 +0300
categories: [Programming]
tags: 
pin: false
description: "Beyond ASCII: How Unicode Changed the Game (and Broke It)."
toc: true
image: /assets/img/Ascii/ascii.png
---



Hi everyone, and welcome back! Today, we’re diving into the fascinating world of **ANSI and Unicode**—two concepts that often trip people up. We’ll start from the basics and build all the way up to the advanced details, breaking down exactly how these encodings work (and why they matter).

Before we get into character sets, let’s clear up another common source of confusion: **signed vs. unsigned types**. Ready to demystify it all? Let’s jump in!

## Signed and Unsigned Types
Integral types may be signed or unsigned.

Signed represent negative or positive numbers(including zero).

By default int,short,long and long long are all signed.

We obtain the corresponding unsigned types by adding the keyword `unsigned` to the type ie `unsigned long`

Values in computers are represented in binary and representing binary we use certain number of bits.

At unsigned we're not going to dedicate any number of the bits to the sign ie negative.Entire bits are for the value itself making us have large range for unsigned numbers.

POC

```cpp
#include <iostream>

using namespace std;

int main(int argc,char** argv){
    unsigned int num = -1;
    int x = num;

    cout << num << "," << x << '\n';

    return 0;
}
```

After compiling and running we find output:

```bash
4294967295,-1
```
> -1 was converted to its 2s complement. 
{: .prompt-tip }

## ANSI Vs UNICODE

**ASCII -Character encoding**
- Maps some bits(0's and 1's) into characters.
- Uses 7 bits for encoding, it can represent 2^7 =128 different characters.
- Some characters ie `65->A, 166->t` Binary(`100001->A , 1110100->t`)
- Use the converter [here](https://www.duplichecker.com/ascii-to-text.php).
- original 7 bits were only enough to represent characters and punctuations.
- Since a byte has 8 bits,there was a lot of competition which other character should be supported.


**Unicode - Universal Character Encoding**
- Supports many different alphabets and even emojis.
- Unlike ASCII unicode does not define how its mapping should be implemented.
- Only specifies which character refers to which code point.Code point is a hexidecimal number representing a character ie `u+0041-> A`


### DWORD,LPSTR,LPBYTE,HANDLE

You might wonder why Windows-specific types show up in a discussion about ASCII and Unicode. The reason is simple: **these types often define how text and binary data are stored, referenced, and manipulated in memory.**

For example:

* `LPSTR` is a pointer to an ANSI (ASCII) string.
* `LPWSTR` is a pointer to a Unicode string.
* `LPBYTE` can point to raw byte buffers, which could contain either ASCII or Unicode data.
* `HANDLE` and `DWORD` are commonly used in Windows APIs that process encoded text.

Understanding these types is crucial if you’re working with Windows applications, calling APIs, or writing code that has to handle different encodings safely. If you mix them up—or assume the wrong encoding—you can end up with **corrupted strings, crashes, or subtle security vulnerabilities.**


### Decompiler
Primarily, at the heart of a decompiler's functionality is its ability to do precisely that: decompile code. It's a fundamental expectation, after all. Within Binja, users can traverse binary code interactively, accessing disassembled code while making annotations as required. Binja provides multiple views, including disassembly, LILL, MLIL, HLIL, Pseudo-C, and SSA views. In forthcoming posts, I'll delve deeper into these views, and you'll come to appreciate their practical utility. At this stage, it's not essential to delve into the specifics of what each view offers. Typically, when starting, most users tend to primarily utilize disassembly and Pseudo-C views. These two perspectives provide a solid foundation for initial exploration and analysis.
![Exemple of the decompiler's view](/assets/img/hero-1/decomp.png)
_Exemple of the decompiler's view_

### Multi-architecture support
Another one of Binja's notable strengths lies in its extensive support for multiple architectures. It provides compatibility with a broad spectrum of architectures, including well-established ones like x86, ARM, MIPS, PowerPC, and many others. What's particularly noteworthy is that Binja goes beyond this list. In subsequent posts, I'll delve deeper into this aspect, but it's worth mentioning that it offers the capability to introduce support for new architectures through workflows and plugins. This feature proves exceptionally valuable for individuals engaged in tasks involving less common or emerging instruction sets, as well as those tackling Capture The Flag (CTF) challenges. A few exemples include [Motorola 68k](https://github.com/galenbwill/binaryninja-m68k), [msp430](https://github.com/joshwatson/binaryninja-msp430), [Renesas M16C](https://github.com/whitequark/binja-m16c) or [Renesas V850](https://github.com/tizmd/binja-v850).