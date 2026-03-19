# 🚩 Capture The Flag — TryHackMe: Pickle Rick

> Completed the **Pickle Rick** CTF room on TryHackMe — a web exploitation challenge themed around Rick and Morty. Three hidden flags were identified and captured by enumerating a web app, exploiting a command injection panel, and escalating to root to read restricted files.

---

## 📌 Room Overview

| | |
|---|---|
| **Platform** | TryHackMe |
| **Room** | Pickle Rick |
| **Category** | Web Exploitation · Linux Privilege Escalation |
| **Difficulty** | Easy |
| **Target IP** | `10.82.142.138` |
| **AttackBox IP** | `10.82.178.71` |
| **Completed** | Sun, 22 Feb 2026 |
| **Points Earned** | 90 |

---

## 🏆 Room Completion

All tasks completed. Room progress reached 100%.

![Room Completed](screenshots/Challenge_completed.png)

**Final score:** 90 points · 1 completed task · Streak: 1

---

## 🎯 Flags Summary

| Flag | Answer | Location |
|---|---|---|
| **Flag 1** | `mr. meeseek hair` | Web app command panel — found via directory enumeration |
| **Flag 2** | `1 jerry tear` | Web app command panel — found in application files |
| **Flag 3** | `3rd ingredients: fleeb juice` | Escalated to root — read `/root/3rd.txt` via `sudo strings` |

---

## 🔍 Methodology

### Reconnaissance

- Target URL discovered: `http://10.82.142.138`
- Web app enumeration performed on the target
- Login page identified — credentials recovered from source/enumeration
- Logged in and accessed the **Command Panel** at `http://10.82.142.138/portal.php`

### Command Panel — RCE via Web Interface

The web app exposed a **Command Panel** allowing arbitrary command execution — a classic unauthenticated/authenticated RCE vector.

```
http://10.82.142.138/portal.php → Command Panel → Execute
```

Commands were run directly through the browser-based interface against the server.

---

## 🚩 Flag 1 — mr. meeseek hair

**Method:** Web enumeration + command execution  
**Room progress after capture:** 33%

The first ingredient (flag) was retrieved by executing commands through the web command panel. The output revealed the first hidden ingredient stored in the web directory.

**Command used:**
```bash
cat Sup3rS3cretPickl3Ingred.txt
```

**Result:** `mr. meeseek hair` ✅

![First Flag Captured](screenshots/First_Flag_Captured.png)

---

## 🚩 Flag 2 — 1 jerry tear

**Method:** File enumeration via command panel  
**Room progress after capture:** 66%

The second ingredient was found by exploring the file system through the command panel and reading application-level notes.

**Command used:**
```bash
cat /home/rick/second\ ingredients
```

**Result:** `1 jerry tear` ✅

![Second Flag Captured](screenshots/Second_flag_captured.png)

---

## 🚩 Flag 3 — fleeb juice

**Method:** Root privilege escalation via sudo  
**Room progress after capture:** 100%

The third and final ingredient required root access. The `sudo -l` check revealed the user could run `sudo` without a password. The root-owned file `/root/3rd.txt` was read using:

```bash
sudo strings /root/3rd.txt
```

**Result:** `3rd ingredients: fleeb juice` ✅

> Note: The exact answer format required stripping the label prefix — confirmed after initial incorrect submission.

![Third Flag Captured](screenshots/Third_flag_identified_and_captured.png)

---

## 🔐 Vulnerability Summary

| Vulnerability | Description | Severity |
|---|---|---|
| **Unauthenticated RCE** | Command Panel exposed — arbitrary OS commands executable | Critical |
| **Sensitive file in web root** | `Sup3rS3cretPickl3Ingred.txt` accessible via web enumeration | High |
| **Sudo misconfiguration** | User allowed to run all commands as root without password | Critical |
| **Plaintext credentials** | Credentials found in page source / robots.txt | High |
| **No input validation** | Command panel accepts unrestricted shell commands | Critical |

---

## 🛡️ Remediation & Security Recommendations

| Finding | Recommendation |
|---|---|
| RCE via command panel | Remove or sandbox web command execution entirely |
| Sensitive files in web root | Never store secrets in publicly accessible directories |
| Sudo without password | Restrict `sudo` to specific commands with strict whitelisting |
| Credentials in source | Never expose credentials in HTML source or robots.txt |
| Missing input sanitisation | Implement strict input validation and output encoding |

---

## 🧰 Tools Used

| Tool | Purpose |
|---|---|
| **TryHackMe AttackBox** | Browser-based attack environment |
| **Firefox** | Web browser for target enumeration |
| **CyberChef** | Data encoding/decoding |
| **Reverse Shell Generator** | Bookmark — available for use |
| **Web Command Panel** | RCE vector (target app) |
| **sudo strings** | Root file reading via privilege escalation |

---

## 📁 Repository Structure

```
📦 thm-pickle-rick-ctf/
├── 📄 README.md
├── 📄 index.html
├── 📄 Capture_The_Flag.docx
├── 📄 Remediation_Security_Recommendations.docx
└── 📁 screenshots/
    ├── Challenge_completed.png
    ├── First_Flag_Captured.png
    ├── Second_flag_captured.png
    └── Third_flag_identified_and_captured.png
```

---

## ⚠️ Disclaimer

> This CTF was completed on the **TryHackMe** platform within an authorized, sandboxed environment. All target machines were provided by TryHackMe for the purpose of this challenge. No unauthorized systems were accessed.

---

## 👤 Skills Demonstrated

- Web application enumeration and directory discovery
- Command injection / RCE exploitation
- Linux file system navigation
- Privilege escalation via sudo misconfiguration
- CTF methodology (enumerate → exploit → escalate → capture)
- Security remediation report writing

---

*⭐ Room completed: TryHackMe Pickle Rick · 90 points*
