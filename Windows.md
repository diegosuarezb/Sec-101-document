# 🪟 Windows CLI Basics (SEC101)

> [!NOTE]
> Cheatsheet with essential Windows command-line navigation and file interaction equivalents to Linux for TryHackMe.

---

## 1. Terminal Basics & User Info

| Command | Description | Usage Example |
| :--- | :--- | :--- |
| **`echo`** | Simply outputs any text you give it on the screen. | `echo TryHackMe` |
| **`whoami`** | Tells you the username of the account you are currently logged in as. | `whoami` |

---

## 2. Interacting with the Filesystem

* **`dir`** — List the contents of a directory (folders and files) *(equivalent to `ls`)*.
* **`type`** — Output the contents of a file directly to the terminal *(equivalent to `cat`)*.
* **`cd`** — Change the current working directory or print current path if used alone.
* **`cd /d`** — Change directory and drive at the same time (e.g., `cd /d D:\folder`).

```cmd
:: Quick navigation workflow example in Windows CMD:
dir
cd folder4
cd
type note.txt
```

---

## 3. Searching for Files

### 🔍 `dir /s` (Search files by name)
Allows you to search for files recursively across directories using wildcards (`*`).
```cmd
dir /s passwords.txt
dir /s *.txt
```

### 📄 `findstr` (Search text inside files)
Searches inside files for specific text content, patterns, or strings (Windows equivalent to `grep`).
```cmd
findstr /i "81.143.211.90" access.log
findstr /i "THM" access.log
```

---

##  Shell Operators & Redirection

> [!TIP]
> Just like in Linux, mastering these operators saves time when managing output and automation in Windows scripts.

* **`>`** *(Output Redirection)*: Overwrite the **entire** contents of a file with new output.
  ```cmd
  echo password123 > passwords.txt
  ```
* **`>>`** *(Append Operator)*: Add text to the **end** of an existing file without deleting its previous contents.
  ```cmd
  echo tryhackme >> passwords.txt
  ```
| Command / Tool | Description | Usage Example |
| :--- | :--- | :--- |
| **`systeminfo`** / **`ver`** | Check operating system version and details. | `systeminfo` |
| **`hostname`** | Display the hostname of the Windows machine. | `hostname` |
| **`ipconfig /all`** | Look up network adapters, IP addresses, and MAC/physical addresses. | `ipconfig /all` |
| **`tasklist`** | List running processes (useful with filters like `/FI`). | `tasklist /FI "imagename eq Notepad.exe"` |
| **`taskkill`** | Terminate a running process by PID or image name. | `taskkill /PID 1516` |
| **`shutdown /r`** / **`/a`** | Restart a system (`/r`) or abort a scheduled shutdown (`/a`). | `shutdown /r` |
  
##  Windows PowerShell Essentials

| Cmdlet / Command | Description | Usage Example |
| :--- | :--- | :--- |
| **`Get-Command`** | Retrieve a list of available commands matching a pattern. | `Get-Command -Name *Remove*` |
| **`Get-Help`** | Show documentation, examples, and usage help for cmdlets. | `Get-Help New-LocalUser -Examples` |
| **`Get-Content`** | Read and display the content of a file (PowerShell alternative to `type`/`cat`). | `Get-Content C:\Treasure\Hunt\flag.txt` |
| **`Get-ChildItem`** | List items in a directory (PowerShell alternative to `dir`/`ls`). | `Get-ChildItem -Path C:\Users` |
| **`Get-FileHash`** | Calculate cryptographic hashes for files. | `Get-FileHash C:\Users\user\file.txt` |
| **`Invoke-Command`** | Run commands on a remote computer. | `Invoke-Command -ComputerName Target -ScriptBlock {Get-Service}` |

```powershell
# Quick PowerShell filtering example (finding items > 100 bytes):
Get-ChildItem | Where-Object -Property Length -gt 100
```
