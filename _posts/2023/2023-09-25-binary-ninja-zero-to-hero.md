---
title: "Binary Ninja Overview"
date: 2023-09-25 10:00:00 +0200
categories: [Binary Ninja, Tools]
tags: [reverse engineering, exploit, automation, binja]
comments: true
author: maerih

---

Hi everyone, and welcome to the first post of the serie `Binary Ninja: Zero to Hero`! This post will be an introduction to Binary Ninja and an overview of its different components, more than an tutorial of some kind. It will also be used as a teaser of what I will write about and what competences you'll gain from this serie. So let's get started!

## About Binary Ninja
Binary Ninja - from now on referred to as Binja - is developped by [Vector35](https://vector35.com/). Binja came to life around 6 years from now (writing this late 2023). It has numerous [Open-Source](https://github.com/Vector35/) components which makes it very adaptable. Binja's adaptability is a key strength, as it allow a diverse range of plugins. This feature empowers users to tailor their analysis workflows according to their requirements. Furthermore, Binja benefits from a thriving open-source community that has actively [contributed numerous plugins](https://github.com/Vector35/community-plugins/).

Regular updates and ongoing development efforts further bolster Binja's reliability. 

## Versions - Commercial / Personal / Enterprise
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

### Control Flow Graph
In the process of analyzing a function, understanding its flow is often crucial. Flow pertains to how the binary executes, depending on whether specific conditions are met or not. Binja offers a graphical representation of this control flow, which greatly facilitates tracking the execution path. This view serves multiple purposes, and a noteworthy example is the [Lighthouse](https://github.com/gaasedelen/lighthouse) plugin. Lighthouse is particularly valuable as it allows users to visualize the code coverage of a fuzzer on a binary.
![Exemple of the CFG's view](/assets/img/hero-1/graph.png)
_Exemple of the Control Flow Graph's view_

### Debugging
As one might expect, static analysis isn't always straightforward, and there are instances where you may require a debugger. Binary Ninja offers an integrated debugger, which is based on lldb and possesses significant capabilities. In fact, it recently received a substantial update with the implementation of Time-Travel Debugging (TTD), as detailed in the [3.5: Expanded Universe](https://binary.ninja/2023/09/15/3.5-expanded-universe.html#ttd-debugging) release notes. This debugger empowers users to perform a wide range of debugging tasks, essentially covering all the fundamental functions of a debugger and more, all from within the Binary Ninja environment.

### Plugins / Scripting
One of the notable strengths of Binja lies in its API, which is [well-documented](https://api.binary.ninja/) and accessible to users regardless of their license type. However, it's crucial to highlight a key distinction: while the personal license does not permit headless API usage, the commercial license does.

The API offers a wide range of capabilities, enabling users to perform tasks such as creating loaders, implementing support for various new architectures, aiding in pattern recognition, facilitating renaming and highlighting, and essentially accommodating any creative application one can conceive. There have been instances where the API has been employed to effectively to [remove control flow flattening](https://www.lodsb.com/removing-control-flow-flattening-with-binary-ninja) from binaries. 

In the future, I will be sharing numerous posts that delve into the API's utility, particularly regarding its use in uncovering bugs, and assisting in reverse engineering and pattern detection.

### Bonus
- Binja supports custom UI themes, allowing anyone to customize their working environment. 
- Binja supports the use of the [BinSync](https://github.com/binsync/binsync) plugin, a collaborative reversing tool built on the Git versioning system. This plugin allows for fine-grained reverse engineering collaboration across different decompilers.
- [Binary Ninja Cloud](https://cloud.binary.ninja/) is available for free, although there are some functional limitations (API, plugins, etc. are not available).

## Basic usage
In this section, I will showcase all of the major and basic utilities at your disposition, as well as give you their shortcut and tips if I have any. This can be from swiftly moving from one function to another to simply renaming a variable.

First and foremost, when it comes to reverse engineering, one of your initial tasks is renaming a variable, function, or structure. This process is relatively straightforward: simply right-click and choose "Rename Symbol," or you can use the shortcut Ctrl+N, which will prompt you to enter the new name. You can do the same for the types, by selecting "Change Type" or using `Ctrl+Y`. 

<iframe frameborder="0" class="juxtapose" width="100%" height="736" src="https://cdn.knightlab.com/libs/juxtapose/latest/embed/index.html?uid=6906d860-5b93-11ee-b5be-6595d9b17862"></iframe>
_Function view before and after being renamed_

Much improved, wouldn't you agree? You might also observe that I've included a comment describing a variable as a "canary." To add such a comment, simply select one or multiple lines, then right-click and choose "Enter Comment," or utilize the ; notation.

By the way, while I'm here, allow me to introduce you to the [0CD](https://github.com/0xb0bb/0CD) plugin. As stated in the readme, it's "a compilation of small quality-of-life enhancements frequently encountered in CTFs or similar toy problems." As far as my knowledge goes, it currently focuses on renaming canaries and retyping, and it's compatible only with Linux. In an upcoming blog post, I plan to enhance its compatibility to encompass Windows and other architectures. For now, I encourage you to install it as part of our next step: the installation of plugins.

Quickly before though, let me show you how much nicer 0CD makes the canaries:
<iframe frameborder="0" class="juxtapose" width="100%" height="437.00000000000006" src="https://cdn.knightlab.com/libs/juxtapose/latest/embed/index.html?uid=6765b9da-5ba8-11ee-b5be-6595d9b17862"></iframe>
_Canaries before and after using 0CD_

Now, let's discuss how to install plugins. In this step, we'll install 0CD so that you can utilize it throughout this blog series. You have two options: you can either select "Plugins" -> "Manage plugins" or use the shortcut Ctrl+Maj+M. Regardless of your choice, after installation, you'll need to restart Binary Ninja. You can do this by either closing the application and reopening it or using the Ctrl+P shortcut, typing "Restart," and selecting it. The benefit of the second option is that it will restore all your previous work. Voilà! You installed your first Binja plugin.

Now that you've learned how to rename, retype, install plugins, and use shortcuts effectively, let's talk about another important feature: navigating between functions. Since you won't be concentrating solely on one function, you need the ability to move between functions seamlessly.

Binary Ninja offers a convenient way to do this. You can simply scroll up or down, and the view will transition to new functions as you scroll. However, if the function you're looking for is quite far away, you have two options:
- Using the "Symbols" Section: Navigate to the "Symbols" section, where you can either scroll through the list or use the search function to find the specific function you want to jump to.
- Using the `g` Shortcut: Press the `g` key, and a prompt will appear. Here, you can enter the name or address of the function you wish to navigate to. This method allows for quick and precise navigation to a specific function, regardless of its location within the binary.

At this stage, you should be comfortable with basic reversing and navigating through functions. However, you may find it beneficial to explore other views, such as the Graph view. Switching between views is straightforward. You can either select the dropdown list (usually located under the "Linear" label) and choose "Graph," or simply press Enter to toggle between views.
<iframe frameborder="0" class="juxtapose" width="100%" height="643" src="https://cdn.knightlab.com/libs/juxtapose/latest/embed/index.html?uid=d649351a-5ba9-11ee-b5be-6595d9b17862"></iframe>
_Exemple of graph view_



Let's delve into different views such as the *LIL and SSA, and unlock the full potential of Binary Ninja's UI with tools like the stack view, tags, and the memory map.

## Binja UI tour
So, let's do a tour of Binja's UI and see how each components can be useful (and used).

### *LIL and SSA
*LIL are LLIL, MLIL and HLIL. They all have different level of abstraction, and you can learn much more about it by reading [this blog post](https://blog.trailofbits.com/2017/01/31/breaking-down-binary-ninjas-low-level-il/) from Trails of Bits. I will nonetheless talk about it, just not as in depth as it's not currently necessary.

#### LILL
LLIL is the lowest level of abstraction. The semantics of each assembly instruction are represented in the LLIL, with a one-to-one correspondence between the original instructions and the LLIL. This level of IL provides a simplified, architecture-independent representation of the assembly language instructions [docs.binary.ninja](https://docs.binary.ninja/dev/bnil-llil.html).

Here is an example of how a x86 assembly instruction might be represented in LLIL:
```
mov eax, 2
```
This could be represented in LLIL as:
```
LLIL_SET_REG
├─── eax
└─── LLIL_CONST
     └─── 2
```

#### MLIL
MLIL takes the LLIL and further simplifies it by combining multiple low-level instructions into single operations. This level of IL starts to look more like high-level source code, with complex assembly idioms and patterns translated into simpler constructs. It also introduces the concept of variables and types, which are not present in the LLIL. It can be used to perform more high-level analysis tasks, such as identifying variables and their types, tracking their usage, and modeling data flows [docs.binary.ninja](https://docs.binary.ninja/dev/bnil-mlil.html).

Here is an example of how the following x86 assembly instructions might be represented in MLIL:
```
mov eax, [ebp+8]
add eax, 1
mov [ebp+8], eax
```
This could be represented in MLIL as:
```
var_8 = var_8 + 1
```

#### HLIL
HLIL is the highest level of abstraction provided by Binary Ninja. It simplifies the MLIL even further to produce a representation that is very close to the original source code, with constructs like loops and conditionals represented in a way that is very similar to high-level programming languages. This makes it easier to understand the overall logic and control flow of the program.

Here is an example of how the following C code might be represented in HLIL:
```
for (int i = 0; i < 10; ++i) {
    printf("%d\n", i);
}
```
This could be represented in HLIL as:
```
int32_t i;
for (i = 0; i < 10; i = i + 1) {
    printf("%d\n", i);
}
```

#### SSA (Single Static Assignement)
The term "single static assignment" refers to a property of the intermediate representation of a program. The key idea is to ensure that each variable is assigned a value exactly once within a specific scope. This property simplifies many aspects of program analysis and optimization, as it eliminates issues related to variable reassignment, making it easier to reason about the program's behavior. Indeed, SSA form resolves questions of scoping and which definition of a variable should be used. A special φ function is used at join points. 

If I presented you this CFG and asked "what is the value of X?", your first question should be "which X?".

![CFG without SSA](/assets/img/hero-2/SSA_example1.1.png)
_CFG without SSA (taken from Wikipedia)_


However, if I were to present this CFG in the SSA form and asked "What is the value of X2", you would be able to answer "2".

![CFG with SSA](/assets/img/hero-2/SSA_example1.2.png)
_CFG with SSA (taken from Wikipedia)_


Now, you may observe the presence of "Y?" in the code. This is because, at times, a variable's value is contingent upon various branches, such as conditions and loops. In such instances, one might wonder about the variable's potential values. In these cases, you specify the two potential values within a Φ (Phi) function. In this context, the statement will create a fresh definition of Y, referred to as Y3, by "selecting" either Y1 or Y2 based on the previous control flow. 

![CFG with SSA and phi](/assets/img/hero-2/SSA_example1.3.png)
_CFG with SSA and phi (taken from Wikipedia)_

This feature is built-in Binja, and it's great. I'll showcase in future posts how you could use it. Here is what it looks like:
<iframe frameborder="0" class="juxtapose" width="100%" height="643.1087645195354" src="https://cdn.knightlab.com/libs/juxtapose/latest/embed/index.html?uid=ea902caa-6d00-11ee-b5be-6595d9b17862"></iframe>

### Stack View
The stack view lives up to its name by providing a visual representation of the stack, although it differs from the typical debugger's stack view. Indeed, you won't see addresses or actual data, but rather the disposition of the stack.

Here is a simple screenshot to illustrate what I am saying
![Stack View](/assets/img/hero-2/stack_view.png)
_Stack View of a function_

As evident, this display reveals the variable offsets within the stack, accompanied by their respective types and names. At 0x0, you can consistently locate the return address (eip/rip). The negative offsets pertain to local variables declared within the function, while the positive offsets correspond to non-local variables, frequently established through functions like malloc, calloc, and the like.

### Tags
Vector35 has done an excellent job [documenting tags](https://docs.binary.ninja/dev/annotation.html#tags). However, I think their documentation is more oriented on *how to use them* and *how to implement them* with the API, more than *why use them*, which is fair as its whats expected of a documentation. So here I am: tags are great. Its very helpful to quickly (and visually) know what you're looking at. You can create new tags (we'll do that in future posts!) and reference addresses/functions of interests in a few click (or line of code).

There are 19 default tags:
- Bookmarks
- Bugs
- Crashes
- Important
- Library
- Needs Analysis
- Unresolved Stack Adjustment
- Unresolved Indirect Control Flow
- Unresolved Stack Pointer Value
- Invalid Instruction
- Could not Generate Flag IL
- Unimplemented Instruction (LLIL)
- Unimplemented Instruction (MLIL)
- Unimplemented Instruction (HLIL)
- Non-code Branch
- Function too Large
- Function Exceeded Max Analysis Time
- Jump to Unhandled Relocation
- Jump to Malformed Target

Among the available tags, you might find that "Bookmarks," "Bugs," "Crashes," or custom tags are the ones you use most frequently. However, if you require additional tag types, you can create them easily. To do this, navigate to the "Tags" section and access the "Tags type" view. From there, you can create a new tag type by right-clicking or using the shortcut Ctrl+i.
![Tags](/assets/img/hero-2/tags.png)
_How and where to create tags_

With these fundamentals under your belt, you're well-equipped to dive into reverse engineering and pwn challenges using Binary Ninja! In your next post, we will explore additional plugins, delve into different views such as the *LIL and SSA, and unlock the full potential of Binary Ninja's UI with tools like the stack view, tags, and the memory map. Happy reversing and pwning!

See you soon!
