## Blue - ms17-010 Exploitation

## 🎯 Objectives
- Identify vulnerabilities on a Windows machine
- Exploit MS17-010 (EternalBlue)
- Gain initial access
- Capture all flags

---

## 🖥️ Environment

- Platform: TryHackMe
- Target: Windows machine
- Attacker: Kali Linux
- Tools: nmap, metasploit, John The Ripper
- [link to challenge](https://tryhackme.com/room/blue)
- [link to badge](https://tryhackme.com/Andurin/badges/blue?utm_campaign=social_share&utm_medium=social&utm_content=badge&utm_source=copy&sharerId=661a71c1f4b4f33e37b3307b)

---

## 🔎 Reconnaissance

We first started off with a NMAP scan to the victim's IP address using **"nmap -sS 10.48.185.197"** which provided us with the open ports on the victim system. From this we were able to determine that the port 3389 was open and vulnerable but more importantly port 139 was open which we could exploit with EternalBlue.

---

## Staging and initial entry

At this point we started up Metasploit in the terminal. We used **"search ms17-010"** to find the exploit. We then reviewed what parameters needed to be set via the "show info" command. From this we only had to set the RHOSTS value to the victims IP address which we did with **"set RHOSTS 10.48.185.197"**.

After this we entered the **"run"** command to start the exploit which succesfully intiated a meterpreter session on the victim machine. The challenge did further describe how to turn a shell into a Meterpreter session but as this was already a Meterpreter session there was no further need for this.

---

## 💥 Exploitation

One of the objectives of the challenge was to obtain the password for the user "Jon". We first migrated to the lsass service by using **"migrate 716"**. Once done we used the **"hashdump"** command to get the password hash. We then placed the hash in a .txt file

To crack the password we used John The Ripper with the rockyou.txt wordlist to crack the password. We used the following command **"john --wordlist=/usr/share/wordlists/rockyou.txt format=nt password.txt"** to obtain the password.

## Remaining System Flags

Without spoiling the flags... There was 3 flags remaining in the system that we would need to find with hints. They were found by navigating around the file system with Meterpreter in the following locations:

- c:\windows\System32\Config\
- c:\users\jon\documents
- c:\

---

## 📚 Learning Moments

- Using Nmap to identify vulnerable services  
- Configuring and executing Metasploit modules  
- Navigating a compromised system with Meterpreter  
- Re-visiting John the Ripper  

---

## ✅ Conclusion

Overall, this was a great experience and my first real **“true hacking”** feeling. It combined all the concepts learned in the previous TryHackMe modules and applied them in a single, hands-on challenge.

A highly valuable and rewarding exercise.

