# 🐧 Linux Fundamentals - Part 1 (SEC101)

> [!NOTE]
> Cheatsheet with the essential navigation, search, and shell operator commands in Linux for TryHackMe.

---

## 1. Terminal Basics

| Command | Description | Usage Example |
| :--- | :--- | :--- |
| **`echo`** | Simply outputs any text you give it on the screen. | `echo TryHackMe` |
| **`whoami`** | Tells you the username of the account you are currently logged in as. | `whoami` |

---

## 2. Interacting with the Filesystem

* **`ls`** — List the contents of a directory (folders and files).
* **`cat`** — Output the contents of a file directly to the terminal.
* **`cd`** — Change the current working directory to a specified path.
* **`pwd`** — Print the full path of the current working directory.

```bash
# Quick navigation workflow example:
ls
cd folder4
pwd
cat note.txt
```

---

## 3. Searching for Files

### 🔍 `find` (Search files by name)
Allows you to search for files or directories using patterns or wildcards (`*`).
```bash
find -name passwords.txt
find -name *.txt
```

### 📄 `grep` (Search text inside files)
Searches inside files for specific text content, patterns, or strings (ideal for logs).
```bash
grep "81.143.211.90" access.log
grep "THM" access.log
```

---

## 4. Shell Operators & Redirection

> [!TIP]
> Mastering these operators will save you a lot of time when automating tasks or saving scan results.

* **`&`** *(Background Operator)*: Run a command in the background.
* **`>`** *(Output Redirection)*: Overwrite the **entire** contents of a file with new output.
  ```bash
  echo password123 > passwords
  ```
* **`>>`** *(Append Operator)*: Add text to the **end** of an existing file without deleting its previous contents.
  ```bash
  echo tryhackme >> passwords
  ```

  ## 5. Linux Shells & Utilities

| Command / Utility | Description | Usage Example |
| :--- | :--- | :--- |
| **`ls`** | List contents of a directory (files and folders). | `ls -la` |
| **`grep`** | Search for patterns or strings inside files or logs. | `grep "thm-flag" /var/log/auth.log` |
| **`history`** | Display previously executed commands in the current session. | `history` |
| **`chmod +x`** | Grant executable permissions to a script file. | `chmod +x script.sh` |
