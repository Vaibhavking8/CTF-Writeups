# CTF-Writeups
CTF writeups documenting my learning journey through web, crypto, pwn, rev, and forensics challenges, including thought process, tools, and exploits used to solve each problem.

## CTF Tools Reference

A categorized list of common tools used across CTF challenge types.

---

### 🌐 Web Exploitation
| Tool | Description |
|------|-------------|
| [Burp Suite](https://portswigger.net/burp) | Intercept and manipulate HTTP traffic |
| [OWASP ZAP](https://www.zaproxy.org/) | Web app vulnerability scanner |
| [sqlmap](https://sqlmap.org/) | Automated SQL injection detection and exploitation |
| [gobuster](https://github.com/OJ/gobuster) | Directory and file brute-forcing |
| [ffuf](https://github.com/ffuf/ffuf) | Fast web fuzzer for directories, parameters, headers |
| [nikto](https://cirt.net/Nikto2) | Web server scanner |
| [curl](https://curl.se/) | Command-line HTTP requests |
| [Postman](https://www.postman.com/) | API testing and request crafting |
| [Wappalyzer](https://www.wappalyzer.com/) | Identify technologies used on websites |

---

### 🔐 Cryptography
| Tool | Description |
|------|-------------|
| [CyberChef](https://gchq.github.io/CyberChef/) | Encode/decode/encrypt/decrypt anything in-browser |
| [hashcat](https://hashcat.net/hashcat/) | GPU-accelerated hash cracking |
| [John the Ripper](https://www.openwall.com/john/) | Password and hash cracking |
| [RsaCtfTool](https://github.com/RsaCtfTool/RsaCtfTool) | RSA attacks (small exponent, wiener, etc.) |
| [dcode.fr](https://www.dcode.fr/en) | Classical cipher identifier and solver |
| [quipqiup](https://quipqiup.com/) | Automated substitution cipher solver |
| [FactorDB](http://factordb.com/) | Online integer factorization database |
| [SageMath](https://www.sagemath.org/) | Math system for crypto challenges |
| [openssl](https://www.openssl.org/) | Crypto operations from the command line |

---

### 🔍 Forensics
| Tool | Description |
|------|-------------|
| [Autopsy](https://www.autopsy.com/) | Digital forensics platform |
| [Volatility](https://volatilityfoundation.org/) | Memory dump analysis |
| [Wireshark](https://www.wireshark.org/) | Network packet capture and analysis |
| [binwalk](https://github.com/ReFirmLabs/binwalk) | Firmware and binary analysis / file extraction |
| [foremost](http://foremost.sourceforge.net/) | File carving from disk images |
| [exiftool](https://exiftool.org/) | Read and write metadata in files |
| [strings](https://linux.die.net/man/1/strings) | Extract printable strings from binaries |
| [file](https://linux.die.net/man/1/file) | Identify file type by magic bytes |
| [hexedit / xxd](https://linux.die.net/man/1/xxd) | Hex viewer and editor |
| [NetworkMiner](https://www.netresec.com/?page=NetworkMiner) | Passive network forensics from pcap files |
| [scalpel](https://github.com/sleuthkit/scalpel) | File carving tool |

---

### 🕵️ Steganography
| Tool | Description |
|------|-------------|
| [steghide](http://steghide.sourceforge.net/) | Hide/extract data in images and audio |
| [zsteg](https://github.com/zed-0xff/zsteg) | Detect LSB steganography in PNG and BMP |
| [stegsolve](https://github.com/eugenekolo/sec-tools/tree/master/stego/stegsolve) | Visual image analysis for hidden data |
| [Stegseek](https://github.com/RickdeJager/stegseek) | Fast steghide password cracker |
| [Audacity](https://www.audacityteam.org/) | Audio waveform and spectrogram analysis |
| [Sonic Visualiser](https://www.sonicvisualiser.org/) | Detailed audio spectrogram inspection |
| [openstego](https://www.openstego.com/) | Steganography tool for images |
| [pngcheck](http://www.libpng.org/pub/png/apps/pngcheck.html) | Verify PNG integrity and chunk data |
| [imagemagick](https://imagemagick.org/) | Image manipulation and analysis |

---

### 💣 Binary Exploitation (Pwn)
| Tool | Description |
|------|-------------|
| [pwntools](https://github.com/Gallopsled/pwntools) | CTF framework for exploit development |
| [GDB + peda/pwndbg/gef](https://github.com/pwndbg/pwndbg) | Debugger with exploit-focused extensions |
| [ROPgadget](https://github.com/JonathanSalwan/ROPgadget) | Find ROP gadgets in binaries |
| [Ropper](https://github.com/sashs/Ropper) | ROP chain builder |
| [checksec](https://github.com/slimm609/checksec.sh) | Check binary security mitigations |
| [one_gadget](https://github.com/david942j/one_gadget) | Find one-shot shell gadgets in libc |
| [libc-database](https://github.com/niklasb/libc-database) | Identify libc version from leaked addresses |
| [patchelf](https://github.com/NixOS/patchelf) | Modify ELF binary interpreter and RPATH |

---

### 🔄 Reverse Engineering
| Tool | Description |
|------|-------------|
| [Ghidra](https://ghidra-sre.org/) | NSA's open-source reverse engineering suite |
| [IDA Free](https://hex-rays.com/ida-free/) | Industry-standard disassembler/decompiler |
| [Binary Ninja](https://binary.ninja/) | Modern binary analysis platform |
| [Radare2](https://rada.re/n/) | Open-source reverse engineering framework |
| [Cutter](https://cutter.re/) | GUI frontend for Radare2 |
| [x64dbg](https://x64dbg.com/) | Windows debugger for 32/64-bit executables |
| [dnSpy](https://github.com/dnSpy/dnSpy) | .NET assembly decompiler and debugger |
| [jadx](https://github.com/skylot/jadx) | Android APK decompiler |
| [apktool](https://apktool.org/) | Decode and rebuild Android APK files |
| [upx](https://upx.github.io/) | Unpack UPX-packed executables |

---

### 🌍 OSINT
| Tool | Description |
|------|-------------|
| [Maltego](https://www.maltego.com/) | Link analysis and data mining |
| [theHarvester](https://github.com/laramies/theHarvester) | Email, domain, and IP enumeration |
| [Shodan](https://www.shodan.io/) | Search engine for internet-connected devices |
| [WHOIS](https://who.is/) | Domain registration lookup |
| [Wayback Machine](https://web.archive.org/) | Historical snapshots of websites |
| [Google Dorks](https://www.exploit-db.com/google-hacking-database) | Advanced Google search operators |
| [CUPP](https://github.com/Mebus/cupp) | Custom wordlist generator from personal info |
| [SpiderFoot](https://www.spiderfoot.net/) | Automated OSINT reconnaissance |
| [Sherlock](https://github.com/sherlock-project/sherlock) | Find usernames across social platforms |

---

### 🔢 Miscellaneous / General
| Tool | Description |
|------|-------------|
| [Python 3](https://www.python.org/) | Scripting and rapid exploit/solve development |
| [SageMath](https://www.sagemath.org/) | Advanced math for crypto and number theory |
| [CyberChef](https://gchq.github.io/CyberChef/) | Swiss army knife for encoding/decoding |
| [ngrok](https://ngrok.com/) | Expose local server to the internet |
| [tmux](https://github.com/tmux/tmux) | Terminal multiplexer for multi-window workflows |
| [Docker](https://www.docker.com/) | Containerized challenge environments |
| [VSCode](https://code.visualstudio.com/) | Code editor with CTF-friendly extensions |

---

### 📡 Network
| Tool | Description |
|------|-------------|
| [Wireshark](https://www.wireshark.org/) | Deep packet inspection and analysis |
| [nmap](https://nmap.org/) | Network scanning and service enumeration |
| [netcat (nc)](https://linux.die.net/man/1/nc) | TCP/UDP connections and listeners |
| [tcpdump](https://www.tcpdump.org/) | Command-line packet capture |
| [tshark](https://www.wireshark.org/docs/man-pages/tshark.html) | CLI version of Wireshark |
| [scapy](https://scapy.net/) | Python packet crafting and manipulation |
| [masscan](https://github.com/robertdavidgraham/masscan) | High-speed port scanner |
