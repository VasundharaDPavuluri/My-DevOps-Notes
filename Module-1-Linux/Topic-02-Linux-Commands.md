\# My DevOps Notes – Module 1: Linux, Topic 2: Linux Commands 🐧



\## Why Linux Commands Matter

Linux is the foundation of almost every DevOps tool (Docker, Kubernetes, Jenkins, Ansible, etc.).  

Being comfortable with Linux commands is a must for managing servers, containers, and automation.



---



\## 🔹 1. File \& Directory Operations

\- `pwd` → print current working directory  

\- `ls` → list files and directories  

\- `cd` → change directory  

\- `touch file.txt` → create empty file  

\- `mkdir folder` → create directory  

\- `rm file.txt` → remove file  

\- `cp source dest` → copy file  

\- `mv old new` → move/rename file or directory  



---



\## 🔹 2. File Viewing \& Editing

\- `cat file.txt` → view file contents  

\- `less file.txt` → scroll through file  

\- `head -n 10 file.txt` → first 10 lines  

\- `tail -n 10 file.txt` → last 10 lines  

\- `nano file.txt` → edit with nano  

\- `vim file.txt` → edit with vim  



---



\## 🔹 3. File Permissions \& Ownership

\- `ls -l` → show permissions  

\- `chmod 755 file.txt` → change permissions  

\- `chown user:group file.txt` → change owner  



---



\## 🔹 4. User Management

\- `whoami` → current user  

\- `id` → user ID and groups  

\- `adduser username` → create user  

\- `passwd username` → set password  

\- `su - user` → switch user  

\- `sudo command` → run as superuser  



---



\## 🔹 5. Process Management

\- `ps` → list processes  

\- `top` → live process view  

\- `kill PID` → kill process  

\- `jobs` → list background jobs  

\- `fg %1` → bring job to foreground  

\- `bg %1` → run job in background  



---



\## 🔹 6. Disk \& File System Management

\- `df -h` → disk usage  

\- `du -sh folder/` → folder size  

\- `lsblk` → list block devices  

\- `fdisk -l` → list partitions  



---



\## 🔹 7. Networking

\- `ip addr` → show IP addresses  

\- `ping google.com` → check connectivity  

\- `curl URL` → fetch data  

\- `wget URL` → download file  

\- `ssh user@host` → connect to server  

\- `scp file user@host:/path` → copy over SSH  



---



\## 🔹 8. Package Management

(Debian/Ubuntu)  

\- `apt update` → update list  

\- `apt upgrade` → upgrade  

\- `apt install pkg` → install package  



(RHEL/CentOS/Fedora)  

\- `yum install pkg`  

\- `dnf install pkg`  



---



\## 🔹 9. System Information

\- `uname -a` → system info  

\- `hostname` → show hostname  

\- `uptime` → system uptime  

\- `free -h` → memory usage  

\- `lscpu` → CPU details  



---



\## 🔹 10. Archiving \& Compression

\- `tar -cvf archive.tar files/` → create tar  

\- `tar -xvf archive.tar` → extract tar  

\- `gzip file` → compress  

\- `gunzip file.gz` → decompress  

\- `zip file.zip files/` → zip  

\- `unzip file.zip` → unzip  



---



\## Takeaway

This list covers the \*\*most important Linux commands grouped by operations\*\*.  

It’s a quick reference for beginners and a reminder for experienced DevOps engineers.



