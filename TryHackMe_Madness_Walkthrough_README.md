# 🧠 TryHackMe -- Madness Walkthrough

> Platform: TryHackMe\
> Room: Madness\
> Difficulty: Easy\
> Author: Dinesh G\
> Tools Used: Nmap, Burp Suite, Steghide, Hex Editor, Searchsploit, SSH

------------------------------------------------------------------------

# 🔌 Step 1 -- Connect to VPN

``` bash
sudo openvpn <vpn-file-name>
```

After successful connection, access the **Target IP** in your browser.

------------------------------------------------------------------------

# 🔎 Step 2 -- Initial Enumeration

## 🔍 Nmap Scan

``` bash
nmap -sV <target_ip>
```

### 🛠 Open Ports Identified

-   **80/tcp** → HTTP
-   **22/tcp** → SSH

------------------------------------------------------------------------

# 🌐 Step 3 -- Web Enumeration

While browsing the website, we discovered a **broken image**.

Download the image:

``` bash
wget http://<target_ip>/thm.img
```

------------------------------------------------------------------------

# 🧪 Step 4 -- File Signature Analysis

Opening the image in a **hex editor**, we noticed the JPG file signature
was incorrect.

Correct JPG signature:

    FF D8 FF E0 00 10 4A 46 49 46 00 01

After correcting the header and saving the file, the image revealed a
hidden directory:

    http://<target_ip>/th1s_1s_h1dd3n/

------------------------------------------------------------------------

# 🎯 Step 5 -- Parameter Brute Force

The hidden page revealed a secret parameter with values between
**0--99**.

### 🔁 Using Burp Suite Intruder

1.  Intercept request\
2.  Send to Intruder\
3.  Set payload position\
4.  Use numbers 0--99 as payload\
5.  Start attack

✔ We successfully discovered the **passphrase** for `thm.img`.

------------------------------------------------------------------------

# 🕵️ Step 6 -- Steganography Extraction

Extract hidden content from the image:

``` bash
steghide extract -sf thm.img
```

Enter the discovered passphrase.

✔ The extraction revealed a **hidden file**.

View its contents:

``` bash
cat <extracted_file_name>
```

The file contained an **encoded value**.

------------------------------------------------------------------------

# 🔐 Step 7 -- Decode the Value

The extracted content was encoded using **ROT13**.

Decode using CyberChef:

https://cyberchef.io/#recipe=ROT13(true,true,false,13)

✔ Decoded result:

    Username: joker

------------------------------------------------------------------------

# 🖼 Step 8 -- Extract Password from Second Image

Download the additional image provided in the room.

Extract hidden content:

``` bash
steghide extract -sf <image_name>
```

This revealed a password file.

``` bash
cat password.txt
```

Now we have:

-   👤 Username: joker\
-   🔑 Password: `<extracted_password>`{=html}

------------------------------------------------------------------------

# 🔑 Step 9 -- SSH Access

Login via SSH:

``` bash
ssh joker@<target_ip>
```

List files:

``` bash
ls
cat user.txt
```

🎉 **User flag captured successfully!**

------------------------------------------------------------------------

# 🚀 Step 10 -- Privilege Escalation

## 🔎 Find SUID Binaries

``` bash
find / -type f -perm -4000 2>/dev/null
```

We identified a vulnerable version of **screen (4.5)**.

------------------------------------------------------------------------

# 💣 Step 11 -- Exploit Screen 4.5

Search exploit:

``` bash
searchsploit screen 4.5
```

Create exploit script:

``` bash
nano exploit.sh
chmod +x exploit.sh
ls -la
./exploit.sh
```

------------------------------------------------------------------------

# 👑 Step 12 -- Root Access

``` bash
cd /root
ls
cat root.txt
```

🎉 **Root flag captured successfully!**

------------------------------------------------------------------------

# 🧰 Tools Used

-   Nmap\
-   Burp Suite (Intruder)\
-   Steghide\
-   Hex Editor\
-   CyberChef (ROT13)\
-   Searchsploit\
-   SSH

------------------------------------------------------------------------

# 📚 Key Learning Points

-   File signature manipulation\
-   Hidden directory discovery\
-   Parameter brute forcing\
-   Steganography extraction\
-   ROT13 decoding\
-   SUID privilege escalation\
-   Exploit usage with Searchsploit

------------------------------------------------------------------------

# ✅ Flags Captured

✔ User Flag\
✔ Root Flag

