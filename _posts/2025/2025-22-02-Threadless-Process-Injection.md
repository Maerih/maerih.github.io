---
layout: post
title: "Threadless Process Injection: Slipping In Silently"
date: 2025-04-22 19:43:00 +0300
categories: [Malware Development]
tags: 
pin: false
description: Executing malware using the target’s own threads.
toc: true
image: /assets/img/ThreadLess/threadless.png
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

<iframe src="https://giphy.com/embed/Sn1m4mcE6mbHwjaWIx" width="480" height="271" style="" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

- There are so many APIs and all I wanted was one that gets called enough times to slide my shellcode.

- After burning way too many brain cells and coffee, I settled on hooking `CloseHandle`.

Why?
- Because it's called a lot. And by "a lot," I mean spam-level frequency in most apps.

- But let's be honest—this isn’t the most elegant or stealthy option. It works, sure, but it’s noisy. You’re basically piggybacking on Windows cleanup ops.

- So yeah, I went full `CloseHandle` gang for my threadless injection. Not perfect, but it gets the payload through the door... for now.

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

3. **Once compiled, you can view the available options by running:**
    ```bash
    ThreadlessInject.exe -h
    ```

![Desktop View](/assets/img/ThreadLess/3th.png){: .shadow } 
_ThreadlessInject Options_

## ThreadlessInject -> Remote Desktop Connection(mstsc.exe)

- We'll be injecting shellcode into **notepad.exe**, targeting the `CloseHandle` export function.  

- This function resides in **kernelbase.dll** and will serve as our hijack point for threadless execution.

![Desktop View](/assets/img/ThreadLess/4th.png){: .shadow } 
_Lazy Mans choice -> CloseHandle_


- Next is to prepare our shellcode.For demo purposes will be using `notepad.bin` shellcode replace this with your beacon shellcode.

| Option            | Description                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| `-p, --pid=VALUE`  | Target process ID to inject the shellcode into. Typically, this is the PID of the process you want to hijack (e.g., notepad.exe). |
| `-d, --dll=VALUE`  | The name of the DLL that contains the export to be patched. This must be a `KnownDll`, such as `kernelbase.dll`. |
| `-e, --export=VALUE` | The specific exported function within the DLL that will be hijacked (e.g., `CryptProtectMemory`). |

![Desktop View](/assets/img/ThreadLess/4th.png){: .shadow } 
_CommandLine Setting-> Running ThreadLessInject_

> Executing...
{: .prompt-danger }

- Voilà! Notepad popped up—successfully triggered after our shellcode executed. It appeared moments after interacting with the Remote Desktop window, such as minimizing or switching focus.


{% include embed/video.html src='/assets/img/ThreadLess/7.mov' types='mov'%}



{%
  include embed/video.html
  src='/assets/img/ThreadLess/7.mp4'
  types='mov'
  title='Demo video'
  autoplay=true
  loop=true
  muted=true
%}




> Threadless injection provides a low-noise method for code execution by avoiding the creation of new threads and instead hijacking existing exported functions within known DLLs. This technique can help bypass conventional detection mechanisms that monitor thread creation or common injection patterns. By leveraging tools like ThreadlessInject and selecting frequently invoked APIs such as CryptProtectMemory, attackers can achieve code execution with reduced visibility. Proper testing in realistic environments remains essential to ensure effectiveness and evade modern security solutions.
{: .prompt-info }
