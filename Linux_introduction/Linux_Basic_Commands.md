# Linux Command Reference Guide

This guide helps you quickly find essential Linux commands, categorized for convenience. Whether you're a beginner or an advanced user, this quick reference assists in managing files, navigating the system, and monitoring processes with ease.  
_All commands are shown as indented plain text._

---

## Table of Contents

- [📁 File Operations](#-file-operations)
- [📜 Viewing and Manipulating File Content](#-viewing-and-manipulating-file-content)
- [🔗 File Links](#-file-links)
- [📝 Text Processing](#-text-processing)
- [🔧 System Operations](#-system-operations)
- [💻 Virtual Memory and System Information](#-virtual-memory-and-system-information)

---

## 📁 File Operations

**1) `rmdir` — Remove an empty directory**
    
    rmdir directory_name

**2) `rm -r` — Remove a directory and its contents recursively**
    
    rm -r directory_name

**3) `cp` — Copy files or directories**
    
    cp folder/file_name source_folder
    cp -r /path/to/source/ /path/to/destination/    # Copy a directory recursively

**4) `mv` — Move or rename files**
    
    mv file.txt /path/to/destination/               # Move file
    mv oldfile.txt newfile.txt                      # Rename file

**5) `rm` — Remove files**
    
    rm file.txt

---

## 📜 Viewing and Manipulating File Content

**1) `cat` — View the content of a file**
    
    cat file.txt

**2) `echo` — Write content to a file (or create a new one)**
    
    echo "hello" > file.txt                         # Creates or overwrites the file
    echo "hello again" >> file.txt                  # Appends to the file

**3) `zcat` — View the content of a compressed (gzip) file**
    
    zcat file.gz

**4) `head` — View the first 10 lines of a file**
    
    head file.txt

**5) `head -n 5` — View the first 5 lines of a file**
    
    head -n 5 file.txt

**6) `head -c 100` — View the first 100 bytes of a file**
    
    head -c 100 file.txt

**7) `tail` — View the last 10 lines of a file**
    
    tail file.txt

**8) `tail -f` — Monitor logs in real-time**
    
    tail -f /var/log/syslog                         # Continuously monitor a log file

**9) `less` / `more` — Page through content**
- `less` allows forward/backward navigation, search, and line numbers.
- `more` is simpler, forward-only navigation.
    
    less file.txt
    less -N file.txt                                # Show line numbers
    less -S file.txt                                # Disable line wrapping
    more file.txt

---

## 🔗 File Links

**1) Hard Link**  
A hard link creates another directory entry to the same inode (data). Deleting any one hard link does not remove the actual data until **all** hard links are deleted.
    
    ln file.txt file_hardlink.txt

**2) Soft Link (Symbolic Link)**  
A symlink points to a file **path**. Deleting the original file breaks the symlink.
    
    ln -s file.txt file_symlink.txt

---

## 📝 Text Processing

**1) `cut` — Extract specific characters/bytes from each line**
    
    cut -b 1-4 file.txt                              # Extract bytes 1–4
    cut -c 1 file.txt                                # Extract first character

**2) `tee` — Display and save command output**
    
    echo "hello Linux" | tee Linux.txt
    echo "hello Python" | tee -a Linux.txt          # Append to file

**3) `sort` — Sort file lines**
    
    sort file.txt                                    # Sort alphabetically
    sort -n numbers.txt                              # Sort numerically
    sort -k 2 file.txt                               # Sort by column 2 (space/tab-delimited)

**4) `diff` — Compare two files line by line**
    
    diff file1.txt file2.txt

**5) `wc` (Word Count) — Count lines, words, chars/bytes**
    
    wc -l file.txt                                   # Lines
    wc -w file.txt                                   # Words
    wc -c file.txt                                   # Bytes (or characters on some systems)
    wc file.txt                                      # Lines, words, bytes summary

---

## 🔧 System Operations

**1) `pwd` — Print the current working directory**
    
    pwd

**2) `df -h` — Display disk space usage (human-readable)**
    
    df -h

**3) `du -h` — Estimate disk usage**
    
    du -h .
    du -h --max-depth=1                              # Per-directory summary

**4) `ps` — Display process status**
    
    ps -e                                           # All processes
    ps -f                                           # Full-format listing

**5) `top` — Real-time view of system processes**
    
    top                                             # (use htop if installed)

**6) `fuser` — Identify processes using a file/socket**
    
    fuser /path/to/file

**7) `kill` — Send a signal to a process**
    
    kill PID                                        # Terminate process by PID (SIGTERM)
    kill -9 PID                                     # Forcefully terminate (SIGKILL)

**8) `free` — Display system memory usage**
    
    free -h

**9) `nohup` — Run commands immune to hangups**  
Keeps a command running after logout; output goes to `nohup.out` by default.
    
    nohup long_running_command &                    # Run in background, immune to SIGHUP
    disown -h %1                                    # Optional: detach from shell job control

---

## 💻 Virtual Memory and System Information

**1) `vmstat` — Memory, processes, paging, CPU activity**
    
    vmstat                                          # Basic system stats
    vmstat 1                                        # Update every second
    vmstat -s                                       # System summary

---
