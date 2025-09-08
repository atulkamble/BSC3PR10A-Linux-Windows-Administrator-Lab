# BSC3PR10A – Linux/Windows Administrator Lab

## 📚 Google Classroom Assignments — Solutions, Commands & Steps

---

## Unit 1: Introduction to Linux & Windows Administration

### Assignment 1: Linux Command Basics — *15+ commands with steps*

> **Environment:** Any Linux (Ubuntu/Debian/RHEL/Alma/WSL). Open Terminal.

1. Show where you are

```bash
pwd
```

2. List files (long + hidden)

```bash
ls -la
```

3. Move around

```bash
cd /tmp && pwd
cd ~ && pwd
```

4. Make files/folders

```bash
mkdir -p ~/lab1/demo && cd ~/lab1/demo
touch a.txt b.txt
mkdir docs
```

5. Copy & move

```bash
cp a.txt docs/a_copy.txt
mv b.txt docs/b_moved.txt
```

6. View contents

```bash
echo "hello linux" > a.txt
cat a.txt
head -n 3 a.txt
tail -n 3 a.txt
```

7. Find text with grep

```bash
grep -n "hello" a.txt
```

8. File permissions & ownership

```bash
ls -l a.txt
chmod 640 a.txt && ls -l a.txt
# (Need sudo for chown if changing owner)
sudo chown $USER:$USER a.txt && ls -l a.txt
```

9. Who am I / user info

```bash
whoami
id
```

10. System & kernel info

```bash
uname -a
```

11. Disk usage & free space

```bash
du -sh ~/lab1
df -h
```

12. Find files

```bash
find ~/lab1 -type f -name "*.txt"
```

13. Create/remove, safely

```bash
rm -i a.txt
rmdir docs  # fails if not empty
rm -r docs  # remove directory recursively
```

**Screenshots to include:** outputs of each set (pwd, ls -la, chmod result, whoami/id, uname -a, df -h, find).

**Brief explanation example:**

* `chmod 640 a.txt` → owner read/write, group read, others none (rw-r-----).

---

### Assignment 2: Windows Command Prompt Exploration — *10+ commands with steps*

> **Environment:** Windows 10/11. Open **Command Prompt (cmd)** as normal user; use **Administrator** where noted.

1. IP configuration

```cmd
ipconfig /all
```

2. MAC addresses

```cmd
getmac /v
```

3. Host & OS details

```cmd
hostname
systeminfo
```

4. Routing & ARP cache

```cmd
route print
arp -a
```

5. Name lookups

```cmd
nslookup mitwpu.edu.in
```

6. Connections & listening ports

```cmd
netstat -ano
```

7. Processes & killing

```cmd
tasklist
taskkill /PID 1234 /F
```

8. Scheduled tasks (list)

```cmd
schtasks /query /fo LIST /v
```

9. Disk volumes

```cmd
wmic logicaldisk get name,volumename,freespace,size
```

10. Firewall state (Admin cmd)

```cmd
netsh advfirewall show allprofiles
```

**Screenshots:** each command’s output; add a one-line purpose.

---

## Unit 2: File System & User Management

### Assignment 3: Linux File System Navigation — *commands & structure*

> Explore **/home**, **/etc**, **/var**. Use read-only commands (no editing in /etc without reason).

1. Identify current FS usage

```bash
df -h
```

2. Explore /home (user data)

```bash
cd /home
ls -la
du -sh *
```

3. Explore /etc (configs)

```bash
cd /etc
ls -l | head
# Example: show a config file
head -n 10 /etc/hosts
```

4. Explore /var (logs, caches, spools)

```bash
cd /var
ls
sudo du -sh /var/log
sudo ls -l /var/log | head
```

5. Global search examples

```bash
sudo find /etc -maxdepth 1 -type f -name "*.conf" | head
sudo find /var/log -type f -name "*.log" | head
```

6. Summarize paths you visited

```bash
pwd
```

**Explain with examples:**

* `/etc` stores system-wide configuration (e.g., `/etc/ssh/sshd_config`).
* `/var/log` keeps logs (e.g., `/var/log/syslog`, `/var/log/messages`).

**Screenshots:** outputs of ls, head, df -h, du -sh.

---

### Assignment 4: Windows User & Group Management — *PowerShell steps*

> Open **PowerShell as Administrator**.

1. Create users

```powershell
New-LocalUser -Name "student1" -NoPassword
New-LocalUser -Name "student2" -NoPassword
# (Optional secure password)
# $pw = Read-Host -AsSecureString "Enter password"
# New-LocalUser -Name "student1" -Password $pw
```

2. Create a group

```powershell
New-LocalGroup -Name "LabUsers"
```

3. Add users to group

```powershell
Add-LocalGroupMember -Group "LabUsers" -Member "student1","student2"
```

4. Create a folder and assign NTFS permissions

```powershell
New-Item -ItemType Directory -Path "C:\LabShare" | Out-Null
# Grant Modify to group
icacls "C:\LabShare" /grant "LabUsers:(M)" /T
# Verify permissions
icacls "C:\LabShare"
```

5. Verify users & group membership

```powershell
Get-LocalUser | Where-Object Name -in "student1","student2"
Get-LocalGroupMember -Group "LabUsers"
```

**Screenshots:** commands and outputs, plus **C:\LabShare** security tab (optional).

---

## Unit 3: Process & Service Management

### Assignment 5: Linux Process Monitoring — *ps/top/htop/systemctl*

> If `htop` missing: `sudo apt-get install htop` or `sudo dnf install htop`.

1. Snapshot with ps

```bash
ps aux | head
ps aux --sort=-%mem | head
ps aux --sort=-%cpu | head
```

2. Live view

```bash
top
# or
htop
```

3. Kill a process (graceful, then force)

```bash
# find PID e.g., sleep 1000
sleep 1000 &
ps aux | grep "sleep 1000"
kill -15 <PID>     # SIGTERM
kill -9 <PID>      # SIGKILL (only if needed)
```

4. Services (systemd)

```bash
systemctl list-units --type=service | head
systemctl status ssh
sudo systemctl restart ssh
sudo systemctl enable ssh
```

**Screenshots:** ps sorted outputs, htop view, systemctl status.

---

### Assignment 6: Windows Services & Task Manager — *services & startup*

> Use **PowerShell (Admin)** for CLI. Task Manager for GUI screenshots.

1. List services & state

```powershell
Get-Service | Sort-Object Status,DisplayName | Select-Object Status,Name,DisplayName | more
```

2. Start/Stop a service (example: Print Spooler)

```powershell
Stop-Service -Name Spooler
Start-Service -Name Spooler
Get-Service -Name Spooler
```

3. Check startup apps (GUI)

* Open **Task Manager → Startup** tab → take screenshot (show enabled/disabled).

  *CLI alternative (startup tasks via registry/Task Scheduler varies).*

4. Resource usage

```powershell
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10 Name,Id,CPU,PM
```

5. Network utilization (Resource Monitor screenshot)

* Win+R → `resmon` → **Network** tab screenshot.

**Screenshots:** Get-Service, Start/Stop service result, Task Manager → Startup, Get-Process top CPU.

---

## Unit 4: Networking in Linux & Windows

### Assignment 7: Linux Networking — *IP config, tests, sockets*

**A) Show current network info**

```bash
ip addr
ip route
resolvectl status 2>/dev/null || systemd-resolve --status 2>/dev/null
```

**B) Temporary manual IP (non-persistent)**

> Replace `eth0` with your interface (see `ip link`).

```bash
# Remove DHCP lease (optional)
sudo dhclient -r eth0 2>/dev/null || true

# Assign static IP (example)
sudo ip addr flush dev eth0
sudo ip addr add 192.168.1.50/24 dev eth0
sudo ip link set eth0 up
sudo ip route add default via 192.168.1.1
```

**C) Test connectivity**

```bash
ping -c 4 8.8.8.8
ping -c 4 google.com
```

**D) Socket/port checks**

```bash
ss -tulpn | head
# ifconfig alternative (legacy):
ifconfig 2>/dev/null || echo "ifconfig not installed"
```

**E) Netstat (if present)**

```bash
netstat -tulpn 2>/dev/null || echo "netstat not installed"
```

**F) Optional with NetworkManager**

```bash
nmcli device status
# set static
sudo nmcli con mod "Wired connection 1" ipv4.addresses 192.168.1.50/24 ipv4.gateway 192.168.1.1 ipv4.method manual
sudo nmcli con up "Wired connection 1"
```

**Screenshots:** ip addr, ip route, ss -tulpn, pings.

> *Note:* For persistent config on Ubuntu, use **netplan** (/etc/netplan/\*.yaml). On RHEL-like, use **nmcli** or ifcfg files.

---

### Assignment 8: Windows Networking — *IP config, firewall, troubleshooting*

> Use **PowerShell (Admin)**.

**A) Show current settings**

```powershell
Get-NetIPAddress
Get-DnsClientServerAddress
Test-Connection -Count 4 8.8.8.8
```

**B) Set static IP** (replace interface alias, IP, gateway, prefix)

```powershell
Get-NetAdapter
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.1.60 -PrefixLength 24 -DefaultGateway 192.168.1.1
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 8.8.8.8,1.1.1.1
```

**C) Troubleshooting tools**

```powershell
Test-Connection -Count 4 google.com
tracert google.com
nslookup mitwpu.edu.in
```

**D) Firewall rules**

```powershell
# Show profiles
Get-NetFirewallProfile
# Allow inbound TCP 8080
New-NetFirewallRule -DisplayName "Allow TCP 8080" -Direction Inbound -Protocol TCP -LocalPort 8080 -Action Allow
Get-NetFirewallRule -DisplayName "Allow TCP 8080"
```

**E) Connections**

```powershell
Get-NetTCPConnection | Sort-Object State -Descending | Select-Object -First 10 LocalAddress,LocalPort,RemoteAddress,RemotePort,State
```

**Screenshots:** before/after IP settings, firewall rule created, tracert/nslookup outputs.

---

## Unit 5: Shell Scripting & Automation

### Assignment 9: Linux Shell Script — *Directory backup with logging & retention*

**Script: `backup_dir.sh`**

```bash
#!/usr/bin/env bash
# Backup a directory into /backup as tar.gz with timestamp, keep last N backups.

set -euo pipefail

SRC_DIR="${1:-$HOME/labdata}"
DEST_DIR="${2:-/backup}"
RETENTION="${3:-5}"             # keep last N files
TS="$(date +%Y%m%d-%H%M%S)"
HOST="$(hostname -s)"
BASE="$(basename "$SRC_DIR")"
ARCHIVE="${DEST_DIR}/${HOST}_${BASE}_${TS}.tar.gz"
LOGFILE="${DEST_DIR}/backup.log"

# Pre-checks
if [[ ! -d "$SRC_DIR" ]]; then
  echo "Source dir not found: $SRC_DIR" >&2; exit 1
fi
sudo mkdir -p "$DEST_DIR"

# Create archive
echo "[$(date)] START backup of $SRC_DIR -> $ARCHIVE" | sudo tee -a "$LOGFILE" >/dev/null
sudo tar -czf "$ARCHIVE" -C "$(dirname "$SRC_DIR")" "$BASE"
echo "[$(date)] DONE  backup size: $(sudo du -sh "$ARCHIVE" | awk '{print $1}')" | sudo tee -a "$LOGFILE" >/dev/null

# Retention: keep last N
echo "[$(date)] Retention keep last $RETENTION files" | sudo tee -a "$LOGFILE" >/dev/null
sudo ls -1t "${DEST_DIR}/"*.tar.gz 2>/dev/null | tail -n +$((RETENTION+1)) | xargs -r sudo rm -f

# Verify
echo "[$(date)] Current backups:" | sudo tee -a "$LOGFILE" >/dev/null
sudo ls -lh "${DEST_DIR}/"*.tar.gz 2>/dev/null | sudo tee -a "$LOGFILE" >/dev/null
```

**Steps to run & capture:**

```bash
# 1) Prepare sample data
mkdir -p ~/labdata && echo "sample" > ~/labdata/file1.txt

# 2) Save script & make executable
nano backup_dir.sh     # paste script
chmod +x backup_dir.sh

# 3) Run
sudo ./backup_dir.sh ~/labdata /backup 5

# 4) Verify
ls -lh /backup/*.tar.gz
sudo tail -n 20 /backup/backup.log
```

**Optional cron (screenshot cron entry):**

```bash
sudo crontab -e
# Add (daily at 2:30)
30 2 * * * /usr/bin/bash /path/to/backup_dir.sh /home/<user>/labdata /backup 7
```

**Submission:** `backup_dir.sh` + PDF with screenshots (run, log, archives).

---

### Assignment 10: Windows Batch/PowerShell Script — *automation & logging*

#### Option A) PowerShell (`automate.ps1`)

```powershell
<# 
Creates a working folder, generates files, renames them with timestamps, 
and writes a log (CSV).
Usage: .\automate.ps1 -BaseDir "C:\LabAuto" -Count 5
#>

param(
  [string]$BaseDir = "C:\LabAuto",
  [int]$Count = 5
)

$ErrorActionPreference = "Stop"
$ts = Get-Date -Format "yyyyMMdd-HHmmss"
$work = Join-Path $BaseDir "work"
$log  = Join-Path $BaseDir "actions_$ts.csv"

# Prepare folders
New-Item -ItemType Directory -Path $work -Force | Out-Null

# Create files
1..$Count | ForEach-Object {
  $f = Join-Path $work ("file{0}.txt" -f $_)
  "line for file $_ at $(Get-Date)" | Out-File -FilePath $f -Encoding utf8
}

# Rename with timestamp and write log
$events = @()
Get-ChildItem -Path $work -Filter "*.txt" | ForEach-Object {
  $newName = ($_ .BaseName + "_" + $ts + $_.Extension)
  $newPath = Join-Path $work $newName
  Rename-Item -Path $_.FullName -NewName $newName
  $events += [pscustomobject]@{
    Time      = (Get-Date)
    Action    = "Rename"
    OldName   = $_.Name
    NewName   = $newName
    FullPath  = $newPath
  }
}

# Log
$events | Export-Csv -NoTypeInformation -Path $log -Encoding UTF8

# Show results
"Created/Renamed files:"
Get-ChildItem -Path $work
"`nLog file:"; $log
```

**Steps to run & capture (PowerShell):**

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
# Save automate.ps1 and run:
.\automate.ps1 -BaseDir "C:\LabAuto" -Count 5

# Verify:
Get-ChildItem C:\LabAuto\work
Get-Content (Get-ChildItem C:\LabAuto\actions_*.csv | Select-Object -Last 1).FullName
```

#### Option B) Batch (`automate.bat`) — simple variant

```bat
@echo off
set BASE=C:\LabAuto
set WORK=%BASE%\work
for /f "tokens=1-3 delims=/- " %%a in ("%date%") do (set dt=%%c%%a%%b)
for /f "tokens=1-3 delims=:." %%a in ("%time%") do (set tm=%%a%%b%%c)
set TS=%dt%-%tm%

mkdir "%WORK%" 2>nul

rem Create files
for %%i in (1 2 3 4 5) do (
  echo line for file %%i at %date% %time% > "%WORK%\file%%i.txt"
)

rem Rename with timestamp
for %%f in ("%WORK%\*.txt") do (
  ren "%%~ff" "%%~nf_%TS%%%~xf"
)

echo Done. Files in %WORK%.
```

**Steps (CMD):**

```cmd
automate.bat
dir C:\LabAuto\work
```

**Screenshots:** before/after directory listing, log CSV (PowerShell), script execution window.

---

## ✅ Submission Tips (for all assignments)

* Use **clear headings** per task in your PDF.
* Include **terminal/PowerShell screenshots** showing both **commands** and **outputs** (do not crop too tightly).
* Add **1–2 line explanations** under each screenshot: purpose + what the output confirms.
* For scripts, include **code block** + **how you executed** + **verification step**.
* Name files like:
  `TYBSC-CS_LinuxWinLab_UnitX_AssignY_<YourName>_<RollNo>.pdf`

If you want, I can also package the two scripts (**backup\_dir.sh** and **automate.ps1**) into a zip with a short README for you to share with students.
