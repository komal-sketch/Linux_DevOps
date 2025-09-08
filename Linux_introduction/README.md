# Linux Basics – A Beginner's Guide


## 📑 Table of Contents
1. [What is Linux? Introduction to Linux Operating System (OS)](#-1-what-is-linux)  
2. [How to Install Linux?](#-2-how-to-install-linux)  
3. [How to Download & Install Linux (Ubuntu) on a Windows PC?](#-3-how-to-download--install-linux-ubuntu-on-a-windows-pc)  
4. [Difference Between Linux and Windows](#-4-difference-between-linux-and-windows)  
5. [Software Remote Location Server Tools](#-5-software-remote-location-server-tools)  
6. [What are Kernel, Bootloader, and Shell?](#-6-what-are-kernel-bootloader-and-shell)  
7. [Desktop Environment](#-7-desktop-environment)  
8. [Linux System Architecture](#-8-linux-system-architecture)  
9. [Information About Hardware](#-9-information-about-hardware)  
10. [Linux File System](#-10-linux-file-system)  
11. [Difference Between Linux and Unix](#-11-difference-between-linux-and-unix)  
12. [States of Processes in Linux](#-12-states-of-processes-in-linux)  
13. [Conclusion](#-conclusion)  
14. [Resources](#-resources)  

---

## 📌 1. What is Linux?
Linux is an **open-source operating system (OS)** based on **Unix**.  
It is widely used in servers, desktops, mobile devices, and embedded systems.  
Key features:  
- Free and open-source.  
- Secure and stable.  
- Highly customizable.  
- Used in most modern servers and cloud systems.  

---

## 📌 2. How to Install Linux?
Linux can be installed in different ways:  
- Directly on a computer as the main operating system.  
- Alongside Windows (dual boot).  
- Inside a virtual machine (using VirtualBox/VMware).  
- On cloud platforms (AWS, Azure, GCP).  

---

## 📌 3. How to Download & Install Linux (Ubuntu) on a Windows PC?
1. Download **Ubuntu ISO** from [Ubuntu official site](https://ubuntu.com/download).  
2. Create a **bootable USB drive** using tools like **Rufus**.  
3. Restart your PC and boot from the USB.  
4. Choose **Install Ubuntu** and follow on-screen instructions.  
5. Select installation type: dual boot (with Windows) or erase disk (Linux only).  

---

## 📌 4. Difference Between Linux and Windows  

| Feature           | Linux                           | Windows                     |
|-------------------|--------------------------------|-----------------------------|
| Cost              | Free & Open-source             | Paid (licensed)             |
| Security          | More secure (less malware)     | More vulnerable             |
| Customization     | Highly customizable            | Limited                     |
| Performance       | Lightweight and fast           | Requires more resources     |
| Usage             | Servers, Developers, Sysadmins | Home & business users       |

---

## 📌 5. Software Remote Location Server Tools
Linux provides several tools to **remotely connect and manage servers**:  
- **SSH (Secure Shell)** → Connect to servers securely.  
- **Telnet** → Legacy tool (not secure).  
- **VNC / RDP** → Remote desktop access.  
- **SCP / SFTP** → File transfer between systems.  

---

## 📌 6. What are Kernel, Bootloader, and Shell?
- **Kernel** → The core of the OS that manages hardware and system resources.  
- **Bootloader** → The program that loads the OS when the system starts.  
- **Shell** → Interface between the user and the OS (e.g., Bash).  

---

## 📌 7. Desktop Environment
A **desktop environment (DE)** provides the graphical interface (GUI) of Linux.  
Examples:  
- **GNOME**  
- **KDE Plasma**  
- **XFCE**  
- **Cinnamon**  

---

## 📌 8. Linux System Architecture
Linux is structured in layers:  
1. **Hardware** → CPU, RAM, storage, etc.  
2. **Kernel** → Manages hardware & system resources.  
3. **System Libraries** → Provide functions for applications.  
4. **System Utilities** → Core commands (cp, mv, ls, etc.).  
5. **User Applications** → Software installed by users.  

---

## 📌 9. Information About Hardware
Linux provides commands to check system hardware:  
- `lscpu` → CPU details.  
- `lsblk` → Storage details.  
- `free -h` → RAM usage.  
- `lspci` → PCI devices.  
- `lsusb` → USB devices.  

---

## 📌 10. Linux File System
- Linux uses a **hierarchical file system** starting with the root (`/`).  
- Important directories:  
  - `/home` → User files.  
  - `/etc` → Configuration files.  
  - `/bin` → Essential binaries.  
  - `/var` → Logs and temporary files.  
  - `/root` → Root user home directory.  

---

## 📌 11. Difference Between Linux and Unix  

| Aspect        | Linux                         | Unix                          |
|---------------|-------------------------------|-------------------------------|
| Source Code   | Open-source                   | Mostly proprietary            |
| Cost          | Free                          | Commercial (paid)             |
| Hardware      | Runs on almost any hardware   | Limited hardware support      |
| Development   | Community-driven              | Developed by specific vendors |

---

## 📌 12. States of Processes in Linux
A process in Linux can be in different states:  
- **Running (R)** → Actively using CPU.  
- **Sleeping (S)** → Waiting for resources.  
- **Stopped (T)** → Suspended by user.  
- **Zombie (Z)** → Process finished but entry remains in process table.  

---

## ✅ Conclusion
Linux is powerful, stable, and secure, making it a top choice for developers, sysadmins, and businesses.  
This guide gives a **basic overview** to help you understand Linux fundamentals.

---

## 📖 Resources
- [Linux Command Handbook] https://www.freecodecamp.org/news/the-linux-commands-handbook/

