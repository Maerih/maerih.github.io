---
title: PGP KEY
icon: fas fa-key
order: 1
---

# 🔑 PGP Public Key

Use this page to **securely communicate** or verify the integrity of my research artifacts.  
Below, everything is organized into cards for easy navigation.

---

<div class="card" style="margin-bottom: 1.5rem; padding: 1rem; border-radius: 0.75rem; border: 1px solid #e2e8f0; background-color: #f9fafb;">
<h3>🧾 Fingerprint</h3>
<p><code>5D6C 9688 9F39 4A42 794F 2441 DF3F FB27 AC9C 93DF</code></p>
<p><strong>Tip:</strong> Always verify the fingerprint <em>out-of-band</em> before trusting this key.</p>
</div>

<div class="card" style="margin-bottom: 1.5rem; padding: 1rem; border-radius: 0.75rem; border: 1px solid #e2e8f0; background-color: #f0fdf4;">
<h3>📦 Public Key</h3>
<pre>
-----BEGIN PGP PUBLIC KEY BLOCK-----
mQINBGQaVTsBEADQA5bDbLCdqQRtxmpKQTTfFZ9u8ta0w5fqFIWLxH7/nqkB57W5
ZCkV7xsCKoad98Uub0BynMgFtEIFWNeTVhTEe60boArWM4R7S0KpAfymCyQSt0ZY
+hHe08gnhQ4vWIq9dj8WKbns3EoJdlNmHRd+T7DcI7aZp2l472lnBdUYUfXlPNTT
jR6e5o4PuDRWb3LJ3U80W5psIT2g6wZarhTl36tI5C6TdpsA50rN+t4kdG6oWVEy
KtgyBoPixsM3iHtsu4UacFW9zLgMBgcsojyH10ErLFAGO7sTlcZOsEb0nvxGHf+t
TBgJXXn3BPm8zvlDCX22pJw/qQTKPfgS7zp9z2TF/gbergIZjNqJc9ZqGiIEwFZ5
fsX7m3lnVTCMQYzvkG6ygIUvOEQ4pp6Dc1DH67ofuuDurr2S5TQzBaX30hfH4E/V
gNbcJPZQWSiTSvoozb4L+W31j3VhVOWvLs18sDQwFcCEGb
-----END PGP PUBLIC KEY BLOCK-----
</pre>
<p>Download the key: <a href="/assets/publickey.asc" target="_blank">publickey.asc</a></p>
</div>

<div class="card" style="margin-bottom: 1.5rem; padding: 1rem; border-radius: 0.75rem; border: 1px solid #e2e8f0; background-color: #f0f9ff;">
<h3>🌐 Keyservers</h3>
<ul>
<li><a href="https://pgp.mit.edu/" target="_blank">MIT PGP Keyserver</a></li>
<li><a href="https://keyserver.ubuntu.com/" target="_blank">Ubuntu Keyserver</a></li>
</ul>
<pre>
gpg --keyserver pgp.mit.edu --recv-keys 5D6C96889F394A42794F2441DF3FFB27AC9C93DF
</pre>
</div>

<div class="card" style="margin-bottom: 1.5rem; padding: 1rem; border-radius: 0.75rem; border: 1px solid #e2e8f0; background-color: #fff7ed;">
<h3>✍️ Verifying Messages or Artifacts</h3>
<pre>
gpg --verify artifact.txt.sig artifact.txt
</pre>
<p>A valid signature confirms the file is <strong>authentic</strong> and <strong>untampered</strong>.</p>
</div>

<div class="card" style="margin-bottom: 1.5rem; padding: 1rem; border-radius: 0.75rem; border: 1px solid #e2e8f0; background-color: #f5f3ff;">
<h3>🛡️ Secure Communication Tips</h3>
<ul>
<li>Always verify the fingerprint <em>out-of-band</em>.</li>
<li>Use this key to encrypt emails, files, or research artifacts.</li>
<li>Never rely solely on keyservers; double-check fingerprints.</li>
<li>Keep your private key offline or hardware-backed for security.</li>
</ul>
</div>

<div class="card" style="padding: 1rem; border-radius: 0.75rem; border: 1px solid #e2e8f0; background-color: #f0fdf4;">
<h3>💻 Copy Fingerprint</h3>
<button onclick="navigator.clipboard.writeText('5D6C96889F394A42794F2441DF3FFB27AC9C93DF')" 
style="padding: 0.5rem 1rem; border-radius: 0.5rem; border: none; background-color: #4ade80; color: #000; cursor: pointer;">
Copy Fingerprint
</button>
</div>
