---
title: TITLE
date: 2021-11-27 00:00:00 +0800
categories: [TOOLS]
tags: [Metasploit]     # TAG names should always be lowercase
description: Short summary of the post.
pin: false
math: true
---


<h4>🔍 Introduction to Metasploit Framework</h4>

In the world of cybersecurity, the Metasploit Framework stands out as one of the most powerful and widely used tools for penetration testing and ethical hacking. Originally developed by H.D. Moore in 2003 as a portable network tool using Perl, Metasploit has evolved into a comprehensive framework now maintained by Rapid7. It provides security professionals and researchers with a robust platform to develop, test, and execute exploits against remote targets.

At its core, Metasploit simplifies the process of discovering vulnerabilities, gaining access, and validating security defenses — all within a structured, modular environment. From system exploitation and privilege escalation to post-exploitation and pivoting, Metasploit offers a full arsenal of features that make it indispensable for offensive security testing.

Whether you’re a beginner learning about cybersecurity or an experienced red team operator, understanding Metasploit is crucial to mastering modern penetration testing techniques.

<h4>🛠️ Metasploit Basics: Essential Commands for Beginners</h4>


Metasploit is a powerful tool for penetration testers and security researchers. Before diving into real-world exploitation or post-exploitation tasks, it's important to get comfortable with the Metasploit console (`msfconsole`) and its basic commands.

> 📂 **Metasploit Console Location (if not in your system PATH):**  
> `/usr/share/metasploit-framework`

---

## 📘 Basic Metasploit Commands

```bash
# Launch the Metasploit console
msfconsole

# Display the global help menu with command usage
help

# Search for modules that include the keyword 'exploit'
search exploit

# Search for modules targeting the Windows platform
search platform:windows

# Search for modules related to port 22 (commonly SSH)
search port:22

# Narrow down search to exploits related to port 22
search port:22 type:exploit

# Show help options for the search command
search -h

# List all available payloads
show payloads

# List all available exploits
show exploits

# List auxiliary modules (used for scanning, fuzzing, etc.)
show auxiliary

# List post-exploitation modules
show post

# List encoding modules used to bypass filters and AV
show encoders

# List evasion modules
show evasions

# List nop generators
show nops
