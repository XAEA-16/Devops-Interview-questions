# Linux and Shell Interview Preparation

## 1. Linux File Structure

BHE TV UP DL → mnemonic for remembering main directories:

B → /bin – essential binaries

H → /home – user home directories

E → /etc – configuration files

T → /tmp – temporary files

V → /var – variable data (logs, mail, etc.)

U → /usr – user programs and utilities

P → /proc – process info (virtual filesystem)

D → /dev – device files

L → /lib – libraries


## 2. Command Usage
- **List files:** `ls -l` (for detailed listing) or `ls -a` (to include hidden files).
- **`uname` command use:** Used to print system information. `uname -a` shows all details (kernel, OS, architecture, etc.).
- **IP address:** Check using `ip addr` (modern) or `ifconfig` (older systems).
- **Insert in file (append):** `echo "text to add" >> filename.txt`

## 3. What have you written in script
I've written scripts primarily for **automating operational tasks**, such as **system health checks** (monitoring CPU/disk), **log rotation and cleanup**, and **deployment/build automation** across environments.

## 4. What is shebang
The shebang is the first line of a script (e.g., `#!/bin/bash`). It's an instruction to the operating system's kernel, telling it **which interpreter** to use to execute the script.

## 5. Command to check file system disk space usage
The primary command is **`df -h`** (disk free, with human-readable output).

## 6. What are inodes in Linux
An **inode (index node)** is a data structure storing **metadata** about a file or directory, such as size, permissions, ownership, and the disk block locations of the data. Every file is uniquely identified by its inode number.

## 7. Write a shell script on factorial of a number.

```bash
#!/bin/bash

# Calculates the factorial of the first argument

if [ -z "$1" ] || ! [[ "$1" =~ ^[0-9]+$ ]]; then
    echo "Usage: $0 <non-negative integer>"
    exit 1
fi

number=$1
factorial=1

for (( i=1; i<=number; i++ )); do
    # Use BASH arithmetic expansion
    factorial=$(( factorial * i ))
done

echo "The factorial of $number is: $factorial"
````

## 8\. What are the key differences between Linux and Unix?

Linux is an **open-source, freely available** clone of Unix, developed by a community. Unix is the original OS, typically **proprietary** and commercial (e.g., Solaris, AIX).

## 9\. Explain the Linux file system hierarchy (FHS).

FHS defines the standard directory structure starting at **root (/)**. Key directories include `/etc` (configuration), `/home` (user data), `/var` (variable data like logs), and `/usr` (secondary program hierarchy).

## 10\. What's the difference between a hard link and a soft (symbolic) link?

A **Hard Link** points to the **same inode** as the original file; deleting the original doesn't affect the link. A **Soft Link** is a separate file that stores the **path** to the original file; deleting the original breaks the link.

## 11\. What are `inodes`?

See Q6. Inodes are data structures containing all **file metadata** (permissions, size, location of data blocks) except for the filename itself.

## 12\. How do you check disk space usage in Linux?

**`df -h`** checks filesystem utilization (how much space is free). **`du -sh /path`** checks the size of a specific directory or file.

## 13\. What are the different types of file permissions in Linux? How do you change them?

Permissions are **Read (r)**, **Write (w)**, and **Execute (x)**, applied to the **Owner (u)**, **Group (g)**, and **Others (o)**. They are changed using the **`chmod`** command, either with octal numbers (e.g., `755`) or symbolic modes (e.g., `u+x`).

## 14\. How do you check network connectivity to a remote host?

Use **`ping hostname_or_ip`** for basic ICMP reachability. For specific port connectivity, use **`telnet host port`** or **`nc -zv host port`**.

## 15\. How do you check for open ports on a Linux server?

Use **`netstat -tuln`** or the more modern **`ss -tuln`** to list all TCP and UDP sockets in a listening state.

## 16\. What is the purpose of the `/etc/hosts` file?

It provides **static hostname-to-IP address mapping** for local network name resolution, bypassing DNS for specific entries.

## 17\. What is `SSH`? How would you set up passwordless SSH?

**SSH (Secure Shell)** is a protocol for secure remote access. Passwordless setup involves generating an **RSA key pair** (`ssh-keygen`) and copying the **public key** to the remote server's `~/.ssh/authorized_keys` file using **`ssh-copy-id`**.

## 18\. How do you list all running processes on a system?

I use **`ps -ef`** or **`ps aux`** for a static list of all processes. For real-time, interactive monitoring, I use **`top`** or **`htop`**.

## 19\. How would you terminate a process? What is a "zombie process"?

Terminate a process using **`kill PID`** (sends SIGTERM, the graceful signal) or **`kill -9 PID`** (sends SIGKILL, the forced signal). A **Zombie process** is one that has finished execution but still occupies an entry in the process table because its parent hasn't collected its exit status.

## 20\. Explain the Linux boot process.

1.  **BIOS/UEFI** performs hardware checks.
2.  The **Boot Loader** (GRUB) loads.
3.  **GRUB** loads the **Kernel** and **initramfs**.
4.  The **Kernel** initializes devices.
5.  The **Init system (Systemd)** starts, reading its configuration and launching system services.

## 21\. How do you start, stop, and restart a service?

Using the standard **`systemctl`** command:

  - Start: `systemctl start service_name`
  - Stop: `systemctl stop service_name`
  - Restart: `systemctl restart service_name`
  - Enable (at boot): `systemctl enable service_name`

## 22\. What are `cron` jobs?

**Cron jobs** are **time-based scheduled tasks** managed by the `cron` utility. They are configured via the **`crontab`** to run scripts or commands automatically at specified intervals (e.g., hourly backups).

## 23\. What commands would you use to monitor a server's performance?

  - **Overall/CPU/Memory:** **`top`** or **`htop`** (real-time).
  - **Memory:** **`free -h`**.
  - **Disk I/O:** **`iostat`** or **`iotop`**.
  - **Network Traffic:** **`sar -n DEV`** or **`iftop`**.

## 24\. How do you check the kernel version of a Linux system?

The command **`uname -r`** reports the kernel release version.

## 25\. What are some common ways to diagnose a Linux server that won't boot?

1.  **Check GRUB:** Verify the boot parameters and root partition.
2.  **Boot to Rescue Mode:** Enter single-user or rescue mode via GRUB to access the root shell and manually check logs or fix config files.
3.  **Check Drive Integrity:** Check for disk errors.

## 26\. What are "load averages" and what do they indicate?

Load averages represent the **average number of processes either running or waiting to run** (in the run queue) over the last **1, 5, and 15 minutes**. A load average higher than the number of CPU cores indicates **CPU congestion/overload**.

## 27\. How do you create, delete, and manage users and groups in Linux?

  - **Create User:** `useradd username` (or `adduser`).
  - **Delete User:** `userdel -r username` (removes home directory too).
  - **Create Group:** `groupadd groupname`.
  - **Manage User Groups:** `usermod -aG groupname username` (adds user to a supplementary group).

## 28\. What is the purpose of the `/etc/passwd` and `/etc/shadow` files?

  - **`/etc/passwd`:** Stores **non-sensitive user account data** (username, UID, home directory, default shell). The password field is often marked 'x'.
  - **`/etc/shadow`:** Stores **encrypted password hashes** and account expiration policies. It is highly restricted (readable only by root).

## 29\. What are some basic security best practices for a Linux server?

1.  **Regular patching** and updates.
2.  **Disable root SSH login** and use strong SSH key authentication.
3.  Implement a **firewall** (`ufw`/`firewalld`) to restrict access.
4.  Enforce **strong password policies**.

## 30\. Explain `SELinux` or `AppArmor`.

These are **Mandatory Access Control (MAC)** systems. They enhance security by assigning **security contexts** (SELinux) or **profiles** (AppArmor) to users and processes, restricting what resources (files, ports) they can access, regardless of standard file permissions.

## 31\. What is the `#!/bin/bash` line at the start of a script?

This is the **shebang**. It instructs the kernel to use the interpreter located at `/bin/bash` to execute the script.

## 32\. What's the difference between a shell script and a Python script?

A **Shell script** (Bash) is an **interpreter for the operating system**; it excels at automation, command execution, and file manipulation. A **Python script** is a **general-purpose programming language** better suited for complex data processing, algorithms, and application logic.

## 33\. How would you automate a repetitive task using a shell script?

1.  Write the necessary commands into a **shell script file**.
2.  Make the file executable (`chmod +x script.sh`).
3.  Use the **`crontab`** utility to schedule the script to run automatically at the required time or frequency.

## 34\. What is DevOps and how does Linux fit into a DevOps environment?

**DevOps** is a cultural and professional movement focused on integrating Development and Operations to deliver software faster and more reliably. **Linux is the foundational OS** for modern DevOps: it powers **cloud infrastructure**, **containers** (Docker/K8s), and provides the **scripting/automation environment** necessary for CI/CD pipelines.

## 35\. What are the differences between different Linux distributions?

Distributions (e.g., Ubuntu, RHEL, Fedora) differ mainly in their **package management systems** (`apt` for Debian-based vs. `yum/dnf` for Red Hat-based), default **system configuration tools**, and **release cycles** (stable vs. cutting-edge).

## 36\. Explain the role of a package manager.

A package manager's role is to **automate the installation, updating, removal, and dependency management** of software packages from centralized repositories, ensuring system stability and security.

## 37\. Describe a time you had to troubleshoot a complex issue on a Linux server.

*(Example Structure)* I once had a service crashing intermittently. Initial checks showed no obvious errors, but reviewing **Systemd logs** (`journalctl -xe`) revealed "Out of Memory" (OOM) killer events. I then used **`top`** and **`free -h`** to confirm high memory utilization and **`ps aux --sort=-%mem`** to identify the process responsible. The fix involved adjusting the application's memory limits and increasing the server's swap space to manage spikes.

```

***
```
