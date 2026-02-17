```{=html}
<p align="center">
```
`<img src="https://img.shields.io/badge/TryHackMe-Madness-red?style=for-the-badge&logo=tryhackme" />`{=html}
`<img src="https://img.shields.io/badge/Difficulty-Easy-green?style=for-the-badge" />`{=html}
`<img src="https://img.shields.io/badge/Category-Web%20%7C%20Steganography%20%7C%20Privilege%20Escalation-blue?style=for-the-badge" />`{=html}
```{=html}
</p>
```
```{=html}
<h1 align="center">
```
🧠 TryHackMe -- Madness Walkthrough
```{=html}
</h1>
```
```{=html}
<p align="center">
```
A complete technical walkthrough demonstrating web enumeration,
steganography analysis, brute forcing, and privilege escalation.
```{=html}
</p>
```

------------------------------------------------------------------------

## 📌 Overview

  -----------------------------------------------------------------------
  Category                            Details
  ----------------------------------- -----------------------------------
  Platform                            TryHackMe

  Room                                Madness

  Difficulty                          Easy

  Focus Areas                         Web Enumeration, File Analysis,
                                      Steganography, SSH, SUID
                                      Exploitation

  Author                              Dinesh G
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 🔌 1️⃣ Initial Access

## VPN Connection

``` bash
sudo openvpn <vpn-file-name>
```

After connecting, access the target IP in the browser.

------------------------------------------------------------------------

# 🔎 2️⃣ Enumeration

## Nmap Scan

``` bash
nmap -sV <target_ip>
```

### Open Ports Identified:

-   **80/tcp → HTTP**
-   **22/tcp → SSH**

------------------------------------------------------------------------

### 📸 Screenshot Placeholder

/screenshots/01-nmap-scan.png

------------------------------------------------------------------------

# 🌐 3️⃣ Web Analysis

Download the broken image:

``` bash
wget http://<target_ip>/thm.img
```

------------------------------------------------------------------------

# 🧪 4️⃣ File Signature Manipulation

Correct JPG Header:

FF D8 FF E0 00 10 4A 46 49 46 00 01

Hidden directory discovered:

http://`<target_ip>`{=html}/th1s_1s_h1dd3n/

------------------------------------------------------------------------

### 📸 Screenshot Placeholder

/screenshots/02-hex-editor-fix.png /screenshots/03-hidden-directory.png

------------------------------------------------------------------------

# 🎯 5️⃣ Parameter Brute Forcing

Using Burp Suite Intruder (0--99 payload range) to discover passphrase.

------------------------------------------------------------------------

### 📸 Screenshot Placeholder

/screenshots/04-burp-intruder.png

------------------------------------------------------------------------

# 🕵️ 6️⃣ Steganography Extraction

``` bash
steghide extract -sf thm.img
cat <extracted_file_name>
```

Encoded value discovered.

------------------------------------------------------------------------

# 🔐 7️⃣ Decode (ROT13)

Username identified:

joker

------------------------------------------------------------------------

# 🔑 8️⃣ SSH Access

``` bash
ssh joker@<target_ip>
cat user.txt
```

------------------------------------------------------------------------

### 📸 Screenshot Placeholder

/screenshots/05-user-flag.png

------------------------------------------------------------------------

# 🚀 Privilege Escalation

``` bash
find / -type f -perm -4000 2>/dev/null
searchsploit screen 4.5
./exploit.sh
```

``` bash
cd /root
cat root.txt
```

------------------------------------------------------------------------

### 📸 Screenshot Placeholder

/screenshots/06-root-flag.png

------------------------------------------------------------------------

# 🧰 Tools Used

-   Nmap
-   Burp Suite
-   Steghide
-   Hex Editor
-   CyberChef
-   Searchsploit
-   SSH

------------------------------------------------------------------------

# 🎯 Skills Demonstrated

-   Web Enumeration
-   File Signature Analysis
-   Steganography
-   Brute Forcing
-   Privilege Escalation

------------------------------------------------------------------------

⭐ If you found this helpful, consider starring the repository!
