---
layout: post
title: "Unicode vs Ascii"
date: 2023-12-02 19:43:00 +0300
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
Binary Ninja comes in three versions: Non-commercial (Personal), Commercial, and Enterprise. The Non-commercial and Commercial versions use individual license files, while the Enterprise version uses a floating license system. This system allows licenses to be checked out from the Enterprise Server by any client for a specified duration.
The disparities between Non-commercial and Commercial licenses are quite limited, encompassing just two distinctions: the ability to utilize them for commercial purposes and access to the headless API.
In contrast, the Enterprise version not only offers these features but also includes Single Sign-On (SSO), Project Management capabilities, Floating licenses, and Access Control features.

One particularly appealing aspect of the licensing arrangement is that your payment covers a year of updates for Binja, rather than a year of usage. This implies that when you make a payment, you'll receive updates for a year, but you can continue using Binja beyond that period (although I strongly advise everyone to renew annually, given the numerous valuable updates available).

You can find more information [here](https://binary.ninja/purchase/).

## Overview
Binja is an extensive and intricate project, resulting in the development of numerous utilities and components. In this section, I'll provide a broad overview of these components, without delving into detailed explanations for each one. In forthcoming posts, I'll delve into specific use cases and provide a more comprehensive understanding of how these components operate and their practical applications.

### Decompiler
Primarily, at the heart of a decompiler's functionality is its ability to do precisely that: decompile code. It's a fundamental expectation, after all. Within Binja, users can traverse binary code interactively, accessing disassembled code while making annotations as required. Binja provides multiple views, including disassembly, LILL, MLIL, HLIL, Pseudo-C, and SSA views. In forthcoming posts, I'll delve deeper into these views, and you'll come to appreciate their practical utility. At this stage, it's not essential to delve into the specifics of what each view offers. Typically, when starting, most users tend to primarily utilize disassembly and Pseudo-C views. These two perspectives provide a solid foundation for initial exploration and analysis.
![Exemple of the decompiler's view](/assets/img/hero-1/decomp.png)
_Exemple of the decompiler's view_

### Multi-architecture support
Another one of Binja's notable strengths lies in its extensive support for multiple architectures. It provides compatibility with a broad spectrum of architectures, including well-established ones like x86, ARM, MIPS, PowerPC, and many others. What's particularly noteworthy is that Binja goes beyond this list. In subsequent posts, I'll delve deeper into this aspect, but it's worth mentioning that it offers the capability to introduce support for new architectures through workflows and plugins. This feature proves exceptionally valuable for individuals engaged in tasks involving less common or emerging instruction sets, as well as those tackling Capture The Flag (CTF) challenges. A few exemples include [Motorola 68k](https://github.com/galenbwill/binaryninja-m68k), [msp430](https://github.com/joshwatson/binaryninja-msp430), [Renesas M16C](https://github.com/whitequark/binja-m16c) or [Renesas V850](https://github.com/tizmd/binja-v850).