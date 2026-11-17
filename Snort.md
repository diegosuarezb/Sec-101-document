# 🛡️ Snort Challenge & Network Security Analysis (SEC101)

> [!NOTE]
> Comprehensive cheatsheet and command-line reference based on TryHackMe's Snort challenges, rule writing, and packet analysis workflows.

---

## 1. Snort Execution Modes & Flags

| Flag / Command | Description | Example Usage |
| :--- | :--- | :--- |
| **`-c`** | Specifies the configuration file to use. | `sudo snort -c local.rules` |
| **`-A full`** | Sets the alert mode to full logging. | `sudo snort -c local.rules -A full` |
| **`-l .`** | Specifies the logging directory (current directory). | `sudo snort -c local.rules -l .` |
| **`-r`** | Reads/processes a captured PCAP file. | `sudo snort -r traffic.pcap` |
| **`-n`** | Limits the number of packets processed from a capture. | `sudo snort -r snort.log.1752506656 -n 63` |
| **`-T`** | Tests and verifies the configuration file syntax. | `sudo snort -T -c local.rules` |

---

## 2. Writing Snort Rules (Syntax & Operators)

### 🔹 Basic Structure
```text
[action] [protocol] [ip_source] [port_source] [direction] [ip_dest] [port_dest] (options)
```

* **Actions:** `alert`, `log`, `pass`, `drop`, `reject`
* **Directions:** 
  * `->` (Unidirectional flow: source to destination)
  * `<>` (Bidirectional/Two-sided flow: catches traffic traveling in both directions, useful for "from or to" port conditions).

### 🔹 Practical Rule Examples

#### A. Detecting HTTP Traffic (Port 80)
To detect TCP packets coming from or going to port 80 using a two-sided flow operator (`<>`):
```snort
alert tcp any 80 <> any any (msg:"TCP Port 80 Scan"; sid:1000001; rev:1;)
```

#### B. Detecting FTP Traffic (Port 21)
To inspect inbound and outbound traffic on port 21:
```snort
alert tcp any 21 <> any any (msg:"FTP Traffic Scan"; sid:1000002; rev:1;)
```

---

## 3. Analyzing Snort Logs and PCAP Files

When Snort runs with the `-l .` and `-A full` flags, it generates binary log files (e.g., `snort.log.XXXXXXXXXX`). To inspect and filter them effectively from the command line, combine `strings` and `grep`:

### 🔍 Inspecting Specific Packets
To read a specific packet index (e.g., packet #63) from a generated log file:
```bash
sudo snort -r snort.log.1752506656 -n 63
```

### 📄 Extracting Readable Text from Binary Logs (`strings` + `grep`)
Because Snort logs are binary files, convert them to readable ASCII text before searching:
```bash
# 1. Convert binary log to a readable text file
sudo strings snort.log.1752521782 > readable.log

# 2. Search for specific keywords (case-insensitive)
grep -i "ftp" readable.log
```

### 🔢 Counting Log Occurrences (`wc -l`)
To automatically count how many times a specific pattern or error occurs (for example, failed FTP logins matching `"530 User"`):
```bash
grep -i "530 User" readable.log | wc -l
```

---

## 4. Common Traffic Indicators & Patterns

* **FTP Responses:**
  * `530 User`: User not logged in / Failed authentication attempt.
  * `230 User`: Successful user login.
  * `331 Password`: Username verified, waiting for password entry.
* **Network Metrics:**
  * **TTL (Time To Live):** Decrements by 1 with each router hop; prevents routing loops.
  * **SEQ / ACK Numbers:** Used in TCP session tracking during the 3-way handshake.
