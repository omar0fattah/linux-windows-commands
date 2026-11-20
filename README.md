# linux-windows-commands

<div align="center">

# Linux and Windows Command Cheat Sheet

**One page. Every basic command. Linux and Windows side by side.**

![Linux](https://img.shields.io/badge/Linux-Bash-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Windows](https://img.shields.io/badge/Windows-CMD-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![Level](https://img.shields.io/badge/Level-Beginner-2ea44f?style=for-the-badge)

*Built so I stop forgetting the basics. Organized by what you're trying to do, not by command name.*

</div>

---

## Contents

1. [Navigation](#navigation)
2. [Files and Folders](#files-and-folders)
3. [Reading Files](#reading-files)
4. [Searching](#searching)
5. [Redirection and Pipes](#redirection-and-pipes)
6. [Processes](#processes)
7. [System Info](#system-info)
8. [Networking](#networking)
9. [Permissions and Users](#permissions-and-users)
10. [Packages](#packages)
11. [Power](#power)
12. [Getting Help](#getting-help)
13. [Handy Combos](#handy-combos)

> [!NOTE]
> Linux commands are written for Bash on Kali/Debian. Windows commands are for **CMD**. Anything marked *(admin)* needs an elevated prompt on Windows or `sudo` on Linux.

---

## Navigation

| Task | Linux | Windows (CMD) |
|---|---|---|
| Show current folder | `pwd` | `cd` |
| List files | `ls -la` | `dir /a` |
| Change folder | `cd /var/log` | `cd C:\Users` |
| Change drive | n/a | `cd /d D:\` |
| Show folder tree | `tree -L 2` | `tree /f` |
| Clear the screen | `clear` or `Ctrl+L` | `cls` |

**`ls` flags:** `-l` long format, `-a` include hidden files, `-h` human-readable sizes, `-t` sort by time.
**`cd` shortcuts (Linux):** `cd ..` up one level, `cd ~` home, `cd -` previous folder.

> [!TIP]
> Linux paths are **case-sensitive** (`Desktop` and `desktop` are different). Windows paths are not.

> [!NOTE]
> `tree` may not be installed on Linux: `sudo apt install tree`. On Windows, `tree` shows folders only unless you add `/f`.

---

## Files and Folders

| Task | Linux | Windows (CMD) |
|---|---|---|
| Make a folder | `mkdir notes` | `mkdir notes` |
| Make nested folders | `mkdir -p a/b/c` | `mkdir a\b\c` |
| Make an empty file | `touch notes.txt` | `type nul > notes.txt` |
| Copy a file | `cp a.txt b.txt` | `copy a.txt b.txt` |
| Copy a folder | `cp -r folder/ backup/` | `xcopy folder backup /e /i` |
| Move or rename | `mv a.txt /tmp/` | `move a.txt C:\temp\` |
| Delete a file | `rm old.txt` | `del old.txt` |
| Delete an empty folder | `rmdir notes` | `rmdir notes` |
| Delete a folder and contents | `rm -r notes` | `rmdir /s /q notes` |

> [!WARNING]
> **There is no recycle bin.** `rm`, `del` and `rmdir /s` delete for good. Double-check the path, and be extra careful with `sudo rm -rf`.

> [!TIP]
> Wildcards work on both: `rm *.log` (Linux) and `del *.log` (Windows) delete every `.log` file in the current folder.

---

## Reading Files

| Task | Linux | Windows (CMD) |
|---|---|---|
| Print a file | `cat hello.txt` | `type hello.txt` |
| Scroll a long file | `less big.txt` | `more big.txt` |
| First 10 lines | `head -n 10 file.txt` | PowerShell: `Get-Content file.txt -Head 10` |
| Last 10 lines | `tail -n 10 file.txt` | PowerShell: `Get-Content file.txt -Tail 10` |
| Follow a file live | `tail -f /var/log/auth.log` | PowerShell: `Get-Content file.txt -Wait` |
| Count lines | `wc -l file.txt` | `find /c /v "" file.txt` |

> [!TIP]
> `cat` on a huge file floods your terminal. Use `less` (press `q` to quit) or `head`.

---

## Searching

| Task | Linux | Windows (CMD) |
|---|---|---|
| Search text inside files | `grep -rin "password" /etc/` | `findstr /s /i "password" *.txt` |
| Filter command output | `ps aux \| grep chrome` | `tasklist \| findstr chrome` |
| Find a file by name | `find / -name "*.conf" 2>/dev/null` | `dir C:\ /s /b \| findstr .conf` |
| Fast filename search | `locate nmap` | `where nmap` |
| Where is a command? | `which python3` | `where python` |

**`grep` flags:** `-i` ignore case, `-r` recurse into folders, `-n` show line numbers, `-v` invert match.

> [!NOTE]
> `2>/dev/null` hides the "Permission denied" spam when searching from `/`. `locate` can miss new files until you run `sudo updatedb`.

---

## Redirection and Pipes

| Symbol | What it does | Linux | Windows (CMD) |
|---|---|---|---|
| `>` | Send output to a file (**overwrites**) | `ls > files.txt` | `dir > files.txt` |
| `>>` | Send output to a file (**appends**) | `echo "more" >> files.txt` | `echo more >> files.txt` |
| `\|` | Feed one command into another | `cat f.txt \| grep error` | `type f.txt \| findstr error` |
| `2>` | Redirect errors only | `find / -name x 2>/dev/null` | `dir x 2>nul` |

### Create and read a text file

```bash
# Linux
echo "Hello" > hello.txt
cat hello.txt
```

```cmd
:: Windows
echo Hello > hello.txt
type hello.txt
```

> [!WARNING]
> `>` **overwrites** the file. Use `>>` to keep what's already there. In CMD, the space before `>` ends up inside the file (`Hello ` with a trailing space), so write `echo Hello> hello.txt` if that matters.

---

## Processes

| Task | Linux | Windows (CMD) |
|---|---|---|
| List processes | `ps aux` | `tasklist` |
| Live monitor | `top` or `htop` | `taskmgr` |
| Kill by PID | `kill 1234` | `taskkill /PID 1234` |
| Force kill by PID | `kill -9 1234` | `taskkill /PID 1234 /F` |
| Kill by name | `pkill firefox` | `taskkill /IM firefox.exe /F` |

> [!TIP]
> Try the polite kill first (`kill 1234`) before forcing it with `-9` or `/F`. A forced kill gives the program no chance to save or clean up.

---

## System Info

| Task | Linux | Windows (CMD) |
|---|---|---|
| OS version | `cat /etc/os-release` | `ver` |
| Full system details | `uname -a`, `hostnamectl`, `lscpu` | `systeminfo` |
| Current user | `whoami` | `whoami` |
| User ID and groups | `id` | `whoami /all` |
| Machine name | `hostname` | `hostname` |
| Disk space | `df -h` | `fsutil volume diskfree C:` |
| RAM usage | `free -h` | `systeminfo` |
| Uptime | `uptime` | `systeminfo` (see boot time) |
| Date and time | `date` | `date /t` and `time /t` |

> [!TIP]
> Pull one line out of `systeminfo` with `systeminfo | findstr /B /C:"OS"`.

---

## Networking

| Task | Linux | Windows (CMD) |
|---|---|---|
| IP and interface info | `ip a` | `ipconfig /all` |
| Test reachability | `ping -c 4 8.8.8.8` | `ping -n 4 8.8.8.8` |
| Trace the route | `traceroute google.com` | `tracert google.com` |
| DNS lookup | `dig google.com` or `nslookup google.com` | `nslookup google.com` |
| Listening ports + program | `sudo ss -tulnp` | `netstat -abon` *(admin)* |
| Fetch a URL | `curl -I https://example.com` | `curl -I https://example.com` |
| Download a file | `wget https://example.com/f.zip` | `curl -O https://example.com/f.zip` |

**`netstat` flags:** `-a` all connections, `-b` owning executable, `-o` PID, `-n` numeric (no DNS lookups).
**`ss` flags:** `-t` TCP, `-u` UDP, `-l` listening only, `-n` numeric, `-p` show process.

> [!WARNING]
> `ping` on Windows sends **4 packets** and stops. On Linux it runs **forever** until `Ctrl+C`, so use `-c 4`. The count flag is `-n` on Windows and `-c` on Linux.

> [!TIP]
> To find what's using a port, filter the output and match the PID against your process list:
> `netstat -abon | findstr :443` (Windows) or `sudo ss -tulnp | grep :443` (Linux).

---

## Permissions and Users

Run `ls -l` on Linux and you'll see something like `-rwxr-xr--`: file type, then **owner**, **group**, **others**, each with `r` (read = 4), `w` (write = 2), `x` (execute = 1).

| Task | Linux | Windows (CMD) |
|---|---|---|
| Change permissions | `chmod 755 script.sh` | `icacls file /grant user:F` |
| Make a script executable | `chmod +x script.sh` | n/a |
| Change owner | `sudo chown omar:omar file` | `takeown /f file` |
| Run as admin/root | `sudo command` | Open CMD as Administrator |

**Common modes:** `644` owner read/write, everyone else read. `755` owner everything, everyone else read/execute. `600` owner only (SSH private keys need this).

> [!NOTE]
> A script won't run with `./script.sh` until it has the execute bit: `chmod +x script.sh`.

---

## Packages

| Task | Linux (Debian/Kali) | Windows (CMD) |
|---|---|---|
| Refresh package list | `sudo apt update` | `winget source update` |
| Upgrade everything | `sudo apt upgrade` | `winget upgrade --all` |
| Install | `sudo apt install tree` | `winget install <name>` |
| Remove | `sudo apt remove tree` | `winget uninstall <name>` |
| Search | `apt search nmap` | `winget search nmap` |

> [!TIP]
> Always `apt update` before `apt install`, otherwise you can hit "Unable to locate package".

---

## Power

| Task | Linux | Windows (CMD) |
|---|---|---|
| Shut down now | `shutdown -h now` | `shutdown /s /t 0` |
| Restart now | `shutdown -r now` | `shutdown /r /t 0` |
| Cancel a pending shutdown | `shutdown -c` | `shutdown /a` |

> [!NOTE]
> Plain `shutdown /s` or `/r` on Windows gives you a countdown (about 30 seconds). `/a` only works during that countdown, which is the whole point of it.

---

## Getting Help

| Task | Linux | Windows (CMD) |
|---|---|---|
| Full manual | `man ls` (`q` to quit) | `help dir` |
| Quick usage | `ls --help` | `dir /?` |
| Command history | `history` | `doskey /history` |
| Search history | `Ctrl+R` | `F7` (pick from a list) |
| Autocomplete | `Tab` (twice to list options) | `Tab` |
| Stop a running command | `Ctrl+C` | `Ctrl+C` |

---

## Handy Combos

```bash
# Linux: see what's using port 443
sudo ss -tulnp | grep :443

# Linux: save your network config to a file
ip a > netinfo.txt

# Linux: count the files in a folder
ls | wc -l

# Linux: watch a log live and filter it
tail -f /var/log/auth.log | grep "Failed"
```

```cmd
:: Windows: see what's using port 443
netstat -abon | findstr :443

:: Windows: save your network config to a file
ipconfig /all > netinfo.txt

:: Windows: kill every Chrome process
taskkill /IM chrome.exe /F
```

---

<div align="center">

*Found a mistake or want to add something? Open an issue or a pull request.*

</div>
