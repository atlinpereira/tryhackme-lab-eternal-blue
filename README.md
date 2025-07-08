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

### 📂 Move the file to your system

If downloaded to `Downloads` folder:

```bash
mv ~/Downloads/tryhackme.ovpn ~/Documents/
