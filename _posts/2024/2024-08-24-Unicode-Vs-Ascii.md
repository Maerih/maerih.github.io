---
layout: post
title: "Unicode vs Ascii"
date: 2024-1-02 19:43:00 +0300
categories: [Programming]
tags: 
pin: false
description: "Beyond ASCII: How Unicode Changed the Game (and Broke It)."
toc: true
image: /assets/img/Ascii/ascii1.png
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

- Handles are objects that represent a resource.
- DWORDS are typedef for unsigned int.
- LPSTR is a long pointer.
- LPBYTE is a long pointer to a byte array.



Unicode can be stored in **different encoding formats**, such as UTF-8, UTF-16, and UTF-32.

* **UTF-16** typically uses **2 bytes (16 bits) per character**, but some characters (like emojis) require 4 bytes.
* **UTF-32** uses **4 bytes (32 bits) for every character**, which makes processing simpler but increases memory usage.
* **UTF-8** is variable-length: it uses 1 byte for standard ASCII characters and up to 4 bytes for other symbols.

In contrast, **ASCII always uses 1 byte (8 bits)** per character, with only the first 7 bits storing the character code (values 0–127).

Different operating systems and environments prefer different encodings:

* Many **Linux systems** and some APIs use **UTF-32** internally (4 bytes per character).
* Windows commonly uses **UTF-16** for Unicode text.

So depending on the **architecture and platform**, the same Unicode character can occupy anywhere from **1 to 4 bytes**, while ASCII stays at a fixed 1 byte per character.






### 🔤 ANSI vs Unicode in C/C++ (with Code Example)

In Windows programming, strings can be stored and handled in either **ANSI** or **Unicode** formats. Understanding the types used—like `char`, `wchar_t`, `LPCSTR`, and `LPCWSTR`—is key to using the right functions.

#### 🔹 ANSI (Single-byte per character)

* Uses `char`
* Functions: `strlen`, `LPCSTR` (Long Pointer to Constant String)

#### 🔹 Unicode (Typically 2 bytes per character)

* Uses `wchar_t`
* Functions: `wcslen`, `LPCWSTR` (Long Pointer to Constant Wide String)

---

###  Code Example: Comparing ANSI and Unicode Strings

```c
#include <stdio.h>
#include <string.h>
#include <wchar.h>

// Typedefs (as used in Windows headers)
typedef const char* LPCSTR;
typedef const wchar_t* LPCWSTR;

int main() {
    // ANSI string
    LPCSTR ansiStr = "Hello ANSI";
    printf("ANSI: %s\n", ansiStr);
    printf("Length: %zu bytes\n", strlen(ansiStr));

    // Unicode string
    LPCWSTR unicodeStr = L"Hello Unicode";
    wprintf(L"Unicode: %ls\n", unicodeStr);
    wprintf(L"Length: %zu wide chars (usually 2 bytes each)\n", wcslen(unicodeStr));

    return 0;
}
```



**Key Points**

* `strlen` returns the number of **char** characters (ANSI, 1 byte each).
* `wcslen` returns the number of **wchar\_t** characters (Unicode, typically 2 bytes each).
* `LPCSTR` = `const char*` (ANSI)
* `LPCWSTR` = `const wchar_t*` (Unicode)

**Other Types**
- Char -> char
- PSTR OR LPSTR => char*
- PCSTR OR LPCSTR => const char*
- LPWSTR => wchar_t*
- LPCWSTR => const wchar_t*
- etc




###  ANSI vs Unicode APIs and `TCHAR`

In Windows, many APIs have **ANSI** (`A`) and **Unicode** (`W`) versions:

* `SetWindowTextA()` → Takes an ANSI string (`char*`)
* `SetWindowTextW()` → Takes a Unicode string (`wchar_t*`)

To write code that can compile as **either ANSI or Unicode**, Windows SDK provides `TCHAR` and macros like `TEXT()` or `_T()`.

 **How it works:**

* `TEXT("MyApp")` automatically becomes:

  * `L"MyApp"` in Unicode builds
  * `"MyApp"` in ANSI builds

---

###  Example Code

```c
#include <windows.h>
#include <tchar.h>

void SetTitle(HWND hwnd) {
    // TCHAR adapts to ANSI or Unicode
    SetWindowText(hwnd, _T("My Application"));
}
```

---

###  Quick Explanation

* `TCHAR` = `wchar_t` in Unicode mode, `char` in ANSI mode
* `SetWindowText()` automatically maps to `SetWindowTextW()` or `SetWindowTextA()` depending on your project settings
* This lets you **write one codebase** that works in both modes


## Handling ANSI and Unicode Strings with Microsoft C Runtime Macros

Headers for the **Microsoft C Runtime Libraries (CRT)** define a special set of macros to make your code portable between ANSI and Unicode builds.

**How It Works:**

* If `_UNICODE` is **defined**, these macros resolve to the **Unicode (wide-character) versions**.
* If `_UNICODE` is **not defined**, they resolve to the **ANSI versions**.

---

### 🔹 Example: String Length Macros

| Function/Macro           | Resolved Version (ANSI)                   | Resolved Version (Unicode)   |
| ------------------------ | ----------------------------------------- | ---------------------------- |
| `strlen(const char*)`    | Counts ANSI string length                 | N/A                          |
| `wcslen(const wchar_t*)` | N/A                                       | Counts Unicode string length |
| `_tcslen(const TCHAR*)`  | `strlen()` if ANSI, `wcslen()` if Unicode |                              |

---

### 🔹 What About String Copy and Concatenation?

Windows prefixes like `_tcs` or `_tc` adapt automatically:

| Operation           | ANSI Function      | Unicode Function   | Generic Macro       |
| ------------------- | ------------------ | ------------------ | ------------------- |
| Copy string         | `strcpy()`         | `wcscpy()`         | `_tcscpy()`         |
| String length       | `strlen()`         | `wcslen()`         | `_tcslen()`         |
| Concatenate strings | `strcat()`         | `wcscat()`         | `_tcscat()`         |
| Secure variants     | `strcpy_s()`, etc. | `wcscpy_s()`, etc. | `_tcscpy_s()`, etc. |

---

### 🔹 Why Use `TCHAR` and `_tcs*`?

These macros and typedefs let you **compile the same source code** for:

* **ANSI builds** (`char`-based functions)
* **Unicode builds** (`wchar_t`-based functions)

This way, you don’t have to rewrite your logic—just set your project to ANSI or Unicode.

---

### 🧠 **Quick Example**

```c
#include <tchar.h>
#include <string.h>
#include <wchar.h>
#include <stdio.h>

int main() {
    const TCHAR* myString = _T("Hello World");
    size_t len = _tcslen(myString);
    
    _tprintf(_T("Length: %zu\n"), len);
    return 0;
}
```

In **ANSI build**, this resolves to:

```c
strlen(const char*)
```

In **Unicode build**, it resolves to:

```c
wcslen(const wchar_t*)
```

---

### What Missed?

* `_tprintf()` is the generic macro for `printf` (ANSI) or `wprintf` (Unicode).
* `_T()` or `TEXT()` wraps your string literals to adapt automatically (`"text"` vs. `L"text"`).
* Secure string functions have `_s` suffixes (like `_tcscpy_s()`).

> Some headers use UNICODE instead of _UNICODE 
{: .prompt-tip }


Sure—here’s a **clean, clear rewrite** with a title, explanation, code example, and some extra details:

---

## 📝 Representing ANSI and Unicode Strings in C/C++

Strings in C and C++ are defined using **double quotes**, but the way they’re stored in memory depends on whether you use ANSI or Unicode.


### 🔹 ANSI Strings

* Written with plain quotes:


"This is an ANSI string"

````
- Each character takes **1 byte (8 bits)**.
- Suitable for standard ASCII text.

---

### 🔹 Unicode Strings
- To create a Unicode (wide) string, add an `L` prefix:  
```c
L"This is a Unicode string"
````

* Each character typically takes **2 bytes (16 bits)** or more.
* The total size is always a **multiple of 2 bytes**.

---

### 🔹 Generic (Portable) Strings

* Use `_T()` or `TEXT()` macros to automatically select ANSI or Unicode at compile time:

  ```c
  ```

\_T("This is either ANSI or Unicode")

````
- This lets you write one codebase that works in both modes.

---

###  Code Example

```c
#include <tchar.h>
#include <stdio.h>

int main() {
  // ANSI string
  const char* ansiStr = "ANSI Example";
  printf("ANSI: %s (Length: %zu)\n", ansiStr, strlen(ansiStr));

  // Unicode string
  const wchar_t* unicodeStr = L"Unicode Example";
  wprintf(L"Unicode: %ls (Length: %zu)\n", unicodeStr, wcslen(unicodeStr));

  // Generic string
  const TCHAR* genericStr = _T("Generic Example");
  _tprintf(_T("Generic: %s (Length: %zu)\n"), genericStr, _tcslen(genericStr));

  return 0;
}
````

---

### Lastie

* `L` prefix = wide character literal (`wchar_t*`).
* `_T()` macro = picks ANSI or Unicode automatically.
* `wprintf()` and `_tprintf()` are needed to print Unicode or generic strings.
* For Windows API functions, you often also see `TEXT()` as an alternative to `_T()`.

<iframe src="https://giphy.com/embed/dB0zhyjJ6Ejf4aETYZ" width="480" height="271" style="" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/foilarmsandhog-foil-arms-and-hog-fah-conor-mckenna-dB0zhyjJ6Ejf4aETYZ"></a></p>