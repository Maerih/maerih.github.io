---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---

<link href="https://fonts.googleapis.com/css2?family=Alfa+Slab+One&display=swap" rel="stylesheet">

<!-- Animated text block -->
<div style="display: flex; align-items: center; justify-content: space-between;">
  <div>
    <h1>I'm
      <div class="waviy">
        <span style="--i:1">M</span>
        <span style="--i:2">A</span>
        <span style="--i:3">E</span>
        <span style="--i:4">R</span>
        <span style="--i:5">I</span> 
        <span style="--i:5">H</span> 
      </div>
    </h1>
  </div>
  <div>
    <img src="/assets/maeri.webp" alt="Maerih" style="max-width: 50px; border-radius: 12px;" />
  </div>
</div>


<div style="font-size: 30px; font-weight: bold; font-family: monospace;">
  <span id="text"></span><span id="console" class="console-underscore">|</span>
</div>


I’m Onyonka Maeri, an Offensive Security Researcher with a sharp focus on Penetration Testing, Exploit Development, and Reverse Engineering. 

My work involves tactically breaking down complex systems to uncover vulnerabilities, developing targeted exploits, and reinforcing defenses against advanced threats. 

I specialize in Windows Internals and advanced scripting to craft bespoke solutions that stay ahead of evolving attack vectors. Currently, at SCIAT AFRICA, I lead research efforts to enhance cybersecurity strategies and empower organizations to combat sophisticated cyber threats.


## 💻 Offensive Toolkit

I work hands-on with a diverse set of tools and techniques tailored for red teaming and adversarial simulation.

- **Languages**: C++, Python, PowerShell, Bash, Assembly (x64 & ARM), Java 
- **Reverse Engineering**: IDA Pro, Ghidra, x64dbg, Frida, Radare2
- **Recon**: Nmap, Amass, Shodan, Maltego  
- **Exploitation**: Custom exploit development, CVE weaponization, reverse engineering  
- **Post-Exploitation**: BloodHound, Mimikatz, SharpHound, Rubeus  
- **Methodologies**: MITRE ATT&CK, STRIDE, PASTA, OSSTMM, PTES 
- **Frameworks**: Cobalt Strike, Metasploit, Mythic, Empire 


## 🔗 Connect With Me

- 🌐 [Portfolio](https://maerih.github.io/Portfolio/)
- 🐙 [GitHub](https://github.com/maerih)
- 🧠 [TryHackMe Profile](https://tryhackme.com/p/yourname)
- 🎯 [Hack The Box](https://app.hackthebox.com/profile/yourname)
- 💼 [LinkedIn](https://www.linkedin.com/in/maeri/)
- 📧 [Email Me](mailto:onyonkamaeriofficial@gmail.com)





<!-- Inline style for animation and reflection -->
<style>
.waviy {
  position: relative;
  -webkit-box-reflect: below -20px linear-gradient(transparent, rgba(0,0,0,.2));
  font-size: 60px;
}
.waviy span {
  font-family: Consolas, 'Lucida Console', 'Courier New', Monaco, Menlo, monospace;
  position: relative;
  display: inline-block;
  color: #fff;
  text-transform: uppercase;
  animation: waviy 1s infinite;
  animation-delay: calc(0.1s * var(--i));
}
@keyframes waviy {
  0%, 40%, 100% {
    transform: translateY(0);
  }
  20% {
    transform: translateY(-20px);
  }
}
</style>

<script>
  window.onload = function () {
    consoleText(['A C++ist,', 'An Exploit Developer,', 'A Pentester,', 'A Hunter,'], 'text', ['tomato', 'rebeccapurple', 'lightblue']);
  };

  function consoleText(words, id, colors) {
    if (colors === undefined) colors = ['#fff'];
    var visible = true;
    var con = document.getElementById('console');
    var letterCount = 1;
    var x = 1;
    var waiting = false;
    var target = document.getElementById(id);
    target.setAttribute('style', 'color:' + colors[0]);
    window.setInterval(function () {
      if (letterCount === 0 && !waiting) {
        waiting = true;
        target.innerHTML = words[0].substring(0, letterCount);
        setTimeout(function () {
          var usedColor = colors.shift();
          colors.push(usedColor);
          var usedWord = words.shift();
          words.push(usedWord);
          x = 1;
          target.setAttribute('style', 'color:' + colors[0]);
          letterCount += x;
          waiting = false;
        }, 1000);
      } else if (letterCount === words[0].length + 1 && !waiting) {
        waiting = true;
        setTimeout(function () {
          x = -1;
          letterCount += x;
          waiting = false;
        }, 1000);
      } else if (!waiting) {
        target.innerHTML = words[0].substring(0, letterCount);
        letterCount += x;
      }
    }, 120);

    setInterval(function () {
      if (visible) {
        con.style.visibility = 'hidden';
        visible = false;
      } else {
        con.style.visibility = 'visible';
        visible = true;
      }
    }, 400);
  }
</script>