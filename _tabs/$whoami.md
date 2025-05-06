---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---


<h1>🛡 Offensive Security Researcher & Red Team Operator</h1>
<h4>Simulating adversaries. Strengthening defenses.</h4>


<link href="https://fonts.googleapis.com/css2?family=Alfa+Slab+One&display=swap" rel="stylesheet">

<!-- Animated text block -->
<h1>i'm
  <div class="waviy">
    <span style="--i:1">M</span>
    <span style="--i:2">A</span>
    <span style="--i:3">E</span>
    <span style="--i:4">R</span>
    <span style="--i:5">I</span>  
  </div>
</h1>

<!-- Inline style for animation and reflection -->
<style>
.waviy {
  position: relative;
  -webkit-box-reflect: below -20px linear-gradient(transparent, rgba(0,0,0,.2));
  font-size: 60px;
}
.waviy span {
  font-family: 'Alfa Slab One', cursive;
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


<h1>whoami: <span id="dynamic-role">pentester</span></h1>

<script>
  const words = ["pentester", "pythonist", "red teamer", "reverser", "offsec nerd"];
  let index = 0;
  setInterval(() => {
    document.getElementById("dynamic-role").textContent = words[index];
    index = (index + 1) % words.length;
  }, 2000); // Change every 2 seconds
</script>


Cybersecurity is not a product — it’s a mindset. I specialize in offensive security, conducting red team operations, reverse engineering, and custom exploit development to emulate real-world threats and identify security gaps before adversaries do.

---

> I work at the intersection of hacking, strategy, and security architecture.  
{: .prompt-info }

> "Know your enemy and know yourself and you will not be defeated in a hundred battles." — Sun Tzu  
{: .prompt-tip }

> If you think you’re secure just because you’ve installed antivirus, think again.  
{: .prompt-warning }

> Never underestimate a misconfigured service — it might be your breach vector.  
{: .prompt-danger }

---

![Desktop View](/assets/maerih.webp){: .shadow }  
_A view from the red team lab._

![Smaller View](/assets/maerih.webp){: w="350" h="200" }  
_Sample malware analysis environment._

---

## 💻 Offensive Toolkit

I work hands-on with a diverse set of tools and techniques tailored for red teaming and adversarial simulation.

- **Languages**: C++, Python, PowerShell, Bash  
- **Frameworks**: Cobalt Strike, Metasploit, Mythic, Empire  
- **Recon**: Nmap, Amass, Shodan, Maltego  
- **Exploitation**: Custom scripts, CVE weaponization, social engineering  
- **Post-Exploitation**: BloodHound, Mimikatz, SharpHound, Rubeus  
- **Methodologies**: MITRE ATT&CK, STRIDE, PASTA, OSSTMM, PTES  

This is inline code: `while (accessLevel != "domain-admin") escalate();` — part of my automation logic.

---

## 🧬 Philosophy

> The most secure system is still vulnerable — you just haven’t found the hole yet.

Red team operations are more than technical tasks — they’re about understanding how people think, how systems fail, and how attackers persist. I bring that mindset into every engagement, making defenders stronger through offense.

---

{% include embed/youtube.html id='0Wbc5ZwkAMw' title='Sample Red Team Demo' autoplay=true muted=true %}

---

## 🔗 Connect With Me

- 🌐 [My Research Blog](https://yourblog.example.com)
- 🐙 [GitHub: yourusername](https://github.com/yourusername)
- 🧠 [TryHackMe Profile](https://tryhackme.com/p/yourname)
- 🎯 [Hack The Box](https://app.hackthebox.com/profile/yourname)
- 💼 [LinkedIn](https://linkedin.com/in/yourusername)
- 📧 [Email Me](mailto:your@email.com)

---

> Add Markdown syntax content to file `_tabs/about.md`  
{: .filepath }  
and it will show up on this page.  
{: .prompt-tip }

> Add Markdown syntax content to file `_tabs/about.md`{: .filepath } and it will show up on this page.
{: .prompt-tip }
