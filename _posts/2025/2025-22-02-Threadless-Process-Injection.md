---
layout: post
title: "Threadless Process Injection: Slipping In Silently"
date: 2025-04-22 19:43:00 +0300
categories: [Malware Development]
tags: 
pin: false
description: Executing malware using the target’s own threads.
toc: true
image: /assets/img/JSFuck/jsfuck.png
---

<h1>OverView: Threadless Process Injection</h1>

- Most classic process injection plays out like this:

- `→ Alloc memory (VirtualAllocEx) → Drop payload (WriteProcessMemory) → Fire it off (CreateRemoteThread)`

- EDRs are all over that combo—trip any one, and you’re flagged or killed.

- But here’s the twist: Threadless Injection skips the obvious. No new thread, no direct execution from the injector. Instead, the payload hijacks execution silently, using the target’s own threads.

- Threadless Injection is a modern process injection method, which eliminates the need for explicitly creating a dedicated execution thread, thereby reducing the number of steps from the three outlined above to just two. In contrast to the classic thread injection, the execution takes place naturally within an already-existing thread context of the target process. 


# Preparing for Threadless Process Injection

- Hook the first instruction of a target DLL export to hijack control flow.
- Extract the return address from the stack and subtract 5 to get the original export address.
- Save volatile registers to the stack (to restore later).
- Restore original export bytes (removes the hook).
- Call injected shellcode right after our hook logic.
- Once shellcode execution is complete, restore saved registers.
- Jump back to the original export to maintain normal program behavior.

![Desktop View](/assets/img/ThreadLess/thl.png){: .shadow } 



## Demo

- First choose a suitable target to inject to ie notepad, teams, remote desktop, etc
- For us we'll be using remote desktop.
- Analyze target using tools to identify suitable API to hijack.

Let's start our analyzes `remote desktop` as our target.

![Desktop View](/assets/img/ThreadLess/1th.png){: .shadow } 
_Analzing apis called by Remote Desktop AKA mstsc.exe_

We Observe that our `mstsc.exe` has been loaded with PID of `6124` and the APIs have loaded.

![Desktop View](/assets/img/ThreadLess/2th.png){: .shadow } 
_mstsc.exe APIs with PID of 6124_

Going through all these damn Windows APIs trying to find the perfect hook spot had me like:

<iframe src="https://giphy.com/embed/Sn1m4mcE6mbHwjaWIx" width="480" height="271" style="" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/therokuchannel-season-1-the-roku-channel-honest-renovations-Sn1m4mcE6mbHwjaWIx">via GIPHY</a></p>

There are so many APIs and all I wanted was one that gets called enough times to slide my shellcode.

After burning way too many brain cells and coffee, I settled on hooking CloseHandle.

Why?
- Because it's called a lot. And by "a lot," I mean spam-level frequency in most apps.

But let's be honest—this isn’t the most elegant or stealthy option. It works, sure, but it’s noisy. You’re basically piggybacking on Windows cleanup ops.

So yeah, I went full `CloseHandle` gang for my threadless injection. Not perfect, but it gets the payload through the door... for now.

> If you want to be more surgical (and stealthy), consider hooking APIs like:`CryptProtectMemory & CryptUnprotectMemory`  
{: .prompt-tip }

> Avoid frequent or noisy API calls that may overwhelm the beacon or shellcode. Excessive calls can lead to detection, instability, or unintended behavior in the target process.
{: .prompt-danger }

## Using ThreadlessInject by CCob

We'll be using the **ThreadlessInject** tool developed by [CCob](https://github.com/CCob). You can find the repository here:  
🔗 [https://github.com/CCob/ThreadlessInject](https://github.com/CCob/ThreadlessInject)

To get started:

1. **Clone the repository**:
    ```bash
    git clone https://github.com/CCob/ThreadlessInject.git
    ```
2. **Build the tool using Visual Studio to generate:** `ThreadlessInject.exe.`

3. Once compiled, you can view the available options by running:
    ```bash
    ThreadlessInject.exe -h
    ```

![Desktop View](/assets/img/ThreadLess/3th.png){: .shadow } 
_ThreadlessInject Options_