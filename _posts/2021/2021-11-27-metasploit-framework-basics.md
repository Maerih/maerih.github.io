---
title: Metasploit Framework Basics
date: 2021-11-27 00:00:00 +0800
categories: [TOOLS, METASPLOIT]
tags: [Metasploit]     # TAG names should always be lowercase
description: Metasploit Framework, a tool for developing and executing exploit code against a remote target machine.
pin: false
toc: true
math: true
---


<h4>🔍 Introduction to Metasploit Framework</h4>

```bash
 msfconsole
This copy of metasploit-framework is more than two weeks old.
 Consider running 'msfupdate' to update to the latest version.
Metasploit tip: View all productivity tips with the tips command
                                                  
                                   ___          ____
                               ,-""   `.      < HONK >
                             ,'  _   e )`-._ /  ----
                            /  ,' `-._<.===-'
                           /  /
                          /  ;
              _          /   ;
 (`._    _.-"" ""--..__,'    |
 <_  `-""                     \
  <`-                          :
   (__   <__.                  ;
     `-.   '-.__.      _.'    /
        \      `-.__,-'    _,'
         `._    ,    /__,-'
            ""._\__,'< <____
                 | |  `----.`.
                 | |        \ `.
                 ; |___      \-``
                 \   --<
                  `.`.<
                    `-'



       =[ metasploit v6.4.49-dev-                         ]
+ -- --=[ 2492 exploits - 1283 auxiliary - 431 post       ]
+ -- --=[ 1466 payloads - 49 encoders - 13 nops           ]
+ -- --=[ 9 evasion                                       ]

Metasploit Documentation: https://docs.metasploit.com/

msf6 > 
```

In the world of cybersecurity, the Metasploit Framework stands out as one of the most powerful and widely used tools for penetration testing and ethical hacking. Originally developed by H.D. Moore in 2003 as a portable network tool using Perl, Metasploit has evolved into a comprehensive framework now maintained by Rapid7. It provides security professionals and researchers with a robust platform to develop, test, and execute exploits against remote targets.

![Desktop View](/assets/img/general/msfconsole.png){: .shadow } 
_Screenshot when starting Msfconsole!_



At its core, Metasploit simplifies the process of discovering vulnerabilities, gaining access, and validating security defenses — all within a structured, modular environment. From system exploitation and privilege escalation to post-exploitation and pivoting, Metasploit offers a full arsenal of features that make it indispensable for offensive security testing.


## 🛠️ Metasploit Basics: Essential Commands

Metasploit is a powerful tool for penetration testers and security researchers. Before diving into real-world exploitation or post-exploitation tasks, it's important to get comfortable with the Metasploit console (`msfconsole`) and its basic commands.

> 📂 **Metasploit Console Location (if not in your system PATH):**  
> `/usr/share/metasploit-framework`

---

<h4>Setting up Metasploit Database</h4>

🔧 Start PostgreSQL Service and check status
```bash
sudo systemctl start postgresql
```
![Desktop View](/assets/img/general/msf/1.png){: .shadow } 
_Start PostgreSQL Service and check status_

Initialize and start the Metasploit database service
```bash
sudo msfdb init
```
![Desktop View](/assets/img/general/msf/34.png){: .shadow } 
_Initialize and start the Metasploit database service_


Check if the database is running properly
```bash
sudo msfdb status
```
![Desktop View](/assets/img/general/msf/35.png){: .shadow } 
_Check if the database is running properly_

Inside msfconsole, run this to check DB connectivity
```bash
db_status
```
![Desktop View](/assets/img/general/msf/36.png){: .shadow } 
_Inside msfconsole, run this to check DB connectivity_


Use db_connect with credentials to manually connect to the DB
```bash
db_connect user:password@localhost:5432/msf_database
```

```bash
Config File Path

/usr/share/metaploit-framewrork/config/database.yml
```


<h4>📘 Basic Metasploit Commands</h4>


```cpp
public:
    Book() {}
    Book(string id, string title, string author, int publisherYear)
        : id(id), title(title), author(author), publisherYear(publisherYear) {}

    virtual void toString() const = 0;
};
```

Launch the Metasploit console
```bash
msfconsole
```

Display the global help menu with command usage
```bash
help
```

Show help options for the search command
```bash
search -h
```

Search for modules that include the keyword 'exploit'
```bash
search exploit
```

Search for modules targeting the Windows platform
```bash
search platform:windows
```

Search for modules related to port 22 (commonly SSH)
```bash
search port:22
```
Narrow down search to exploits related to port 22
```bash
search port:22 type:exploit
```

List all available payloads
```bash
show payloads
```

List all available exploits
```bash
show exploits
```

List auxiliary modules (used for scanning, fuzzing, etc.)
```bash
show auxiliary
```
List post-exploitation modules
```bash
show post
```

List encoding modules used to bypass filters and AV
```bash
show encoders
```

List evasion modules
```bash
show evasion
```

List nop generators
```bash
show nops
```


## Metasploit WMAP and Nessus Integration Guide

<h4> WMAP Web Scanning Commands</h4>

Initial Setup
```bash
msfconsole
load wmap
wmap_help
```
Add Target to Scope
```bash
wmap_sites -a <ip>
```

list all websites on scope
```bash
wmap_sites -l
```

Add web to target
```bash
wmap_targets -t <ip>
```

