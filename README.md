*This project has been created as part of the 42 curriculum by mel-bakh*

## Description

Born2beroot is a hands-on system administration project that introduces the essential skills needed to set up, configure, and secure a Linux server. The primary objective is to create a fully functional virtual machine running either Debian or Rocky Linux, configured according to strict security and operational standards.

Throughout this project, you'll gain practical experience in:
- Setting up encrypted disk partitions with LVM
- Managing users, groups, and permissions
- Configuring sudo with strict security policies and comprehensive logging
- Implementing strong password policies using PAM
- Securing remote access through SSH configuration
- Setting up and managing a firewall (UFW)
- Understanding system administration best practices

This project serves as a foundation for understanding how production servers are configured and maintained in real-world environments.

## Instructions

### Prerequisites
- VirtualBox (or UTM for macOS/Apple Silicon)
- Debian ISO or Rocky Linux ISO
- At least 8GB of disk space allocated for the VM

### Running the Virtual Machine

1. **Launch the VM:**
   ```
   Open VirtualBox and start the Born2beroot machine
   ```

2. **Login:**
   - Use the non-root user credentials configured during installation
   - The root account is disabled for direct login

3. **Verify Configuration:**
   
   Check the partition structure:
   ```bash
   lsblk
   ```
   
   Verify sudo is properly configured:
   ```bash
   sudo -v
   ```
   
   Check UFW firewall status:
   ```bash
   sudo ufw status
   ```
   
   View SSH configuration:
   ```bash
   sudo cat /etc/ssh/sshd_config | grep -E "Port|PermitRootLogin"
   ```
   
   Test password policy:
   ```bash
   sudo chage -l <username>
   ```
   
   View monitoring script output:
   ```bash
   sudo /usr/local/bin/monitoring.sh
   ```

### Key System Details

- **SSH Port:** 4242 (default 22 changed for security)
- **Firewall:** UFW enabled with only necessary ports open
- **Sudo:** Configured with TTY requirement, restricted PATH, and logging enabled
- **Password Policy:** 30-day expiration, 2-day minimum age, complexity requirements enforced

## Resources

### Documentation & References

- **Official Debian Documentation** – [https://www.debian.org/doc/](https://www.debian.org/doc/)
- **Born2BeRoot Guide (GitBook)** – Comprehensive project walkthrough


### AI Usage

AI tools were used strategically throughout this project to enhance learning and efficiency:

**Learning & Concept Clarification:**
- Understanding the differences between AppArmor and SELinux
- Clarifying PAM configuration options and their security implications

**Documentation:**
- Structuring this README according to 42 standards
- Improving clarity and organization of technical explanations
- Reviewing configuration files for best practices

**Note:** All actual configuration was done manually to ensure full understanding of each component.

## Project Description

### Operating System Choice: Debian

For this project, I chose **Debian 12** as the operating system for several compelling reasons:

**Why Debian?**
- **Stability:** Debian is renowned for its rock-solid stability, making it perfect for learning system administration without unexpected issues
- **Simplicity:** The APT package manager is straightforward and well-documented
- **Community:** Extensive documentation and a large, helpful community
- **Learning-Friendly:** Clear separation between system components makes it easier to understand what each configuration does

**Debian vs Rocky Linux:**

| Aspect | Debian | Rocky Linux |
|--------|--------|-------------|
| **Target Audience** | General purpose, desktop & server | Enterprise servers |
| **Package Manager** | APT (apt/dpkg) | DNF (dnf/rpm) |
| **Learning Curve** | Beginner-friendly | Steeper (enterprise-oriented) |
| **Default Security** | AppArmor | SELinux |
| **Support** | Community-driven | Community + enterprise focus |
| **Best For** | Learning, versatility | Production, enterprise migration |

**Pros of Debian:**
- Easier to configure and troubleshoot for beginners
- Lighter resource footprint
- Faster package updates for stable releases
- More suited for this educational project

**Cons of Debian:**
- Less emphasis on enterprise certifications
- Smaller commercial support ecosystem compared to RHEL-based distros

---

### Main Design Choices

#### 1. Partitioning Strategy

I implemented **LVM (Logical Volume Manager) with full disk encryption** for maximum security and flexibility:

**Partition Structure:**
```
/dev/sda
├── /dev/sda1 (976M) – /boot (unencrypted, required for GRUB)
└── /dev/sda5 (29.8G) – Encrypted partition (LUKS)
    └── LVM Volume Group: mel-bakh-vg
        ├── root (8.1G) → / (root filesystem)
        ├── swap_1 (1.6G) → swap
        └── home (20.2G) → /home (user data)
```

**Why This Design?**
- **Encryption:** Protects data at rest; even if someone gains physical access to the VM disk, they cannot read the data without the passphrase
- **LVM Benefits:**
  - Easy resizing of partitions without downtime
  - Snapshot capabilities for backups
  - Flexibility to add more storage later
- **Separate /home:** Isolates user data from system files, making reinstallation or upgrades safer
- **Swap Space:** Provides virtual memory for better system performance

#### 2. Security Policies

**Password Policy (via PAM):**
- Passwords expire every 30 days
- Minimum 2 days before password can be changed again
- 7-day warning before expiration
- Minimum 10 characters with complexity requirements:
  - At least one uppercase letter
  - At least one lowercase letter
  - At least one digit
  - Maximum 3 consecutive identical characters
  - Password must not contain the username
  - At least 7 characters different from the old password (for root: 7, for other users: 7)

**Sudo Configuration:**
- Authentication limited to 3 attempts
- Custom error message for failed attempts
- TTY required (prevents background scripts from using sudo)
- Restricted PATH for security
- Comprehensive logging to `/var/log/sudo/`
- Input/output logging enabled
- requiretty enabled to prevent automated exploitation

**SSH Hardening:**
- Changed default port from 22 to 4242 (reduces automated attacks)
- Root login disabled (`PermitRootLogin no`)

#### 3. User Management

- Created a non-root user (`mel-bakh`) for daily operations
- User added to groups:  `sudo`
- Root account exists but cannot be used for direct login

#### 4. Services Installed

The system follows a minimalist approach, installing only essential services:

**Core Services:**
- **SSH (OpenSSH Server)** – Secure remote access on port 4242
- **UFW (Uncomplicated Firewall)** – Simple firewall management
- **sudo** – Controlled privilege escalation with logging
- **PAM (Pluggable Authentication Modules)** – Password policy enforcement

#### 5. Monitoring Script

A custom **monitoring.sh** script was created to provide real-time system information:

**Script Functionality:**
The script displays comprehensive system metrics broadcasted to all terminals every 10 minutes via `wall` command:
- Operating system architecture and kernel version
- Physical and virtual CPU count
- RAM usage (current/total and percentage)
- Disk usage (current/total and percentage)
- CPU load percentage
- Last boot date and time
- LVM status (active/inactive)
- Active TCP connections
- Number of logged-in users
- Server IPv4 address and MAC address
- Count of commands executed with sudo

**Automation:**
- Configured in root's crontab to run every 10 minutes
- Executes automatically at system startup
- Uses system commands (`uname`, `df`, `free`, `awk`) to gather real-time data
- Broadcasts information to all terminals without causing errors

This monitoring solution provides administrators with continuous visibility into system health and resource utilization without requiring external monitoring tools.

---

### System Comparisons

#### AppArmor vs SELinux

Both are Mandatory Access Control (MAC) systems that restrict program capabilities beyond traditional Unix permissions.

| Feature | AppArmor (Debian Default) | SELinux (Rocky/RHEL Default) |
|---------|---------------------------|------------------------------|
| **Security Model** | Path-based profiles | Context-based labels |
| **Configuration** | Human-readable profiles | Complex policy language |
| **Learning Curve** | Easier for beginners | Steeper, requires deep understanding |
| **Flexibility** | Good for common scenarios | Extremely granular control |
| **Debugging** | Simpler logs | More detailed but complex logs |

---

#### UFW vs firewalld

Both manage the underlying `iptables`/`nftables` firewall, but with different interfaces.

| Feature | UFW (Used in this project) | firewalld |
|---------|----------------------------|-----------|
| **Interface** | Command-line, rule-based | Zone-based, XML configs |
| **Complexity** | Simple and straightforward | More complex, more features |
| **Default on** | Debian/Ubuntu | RHEL/Rocky/Fedora |
| **Use Case** | Single-server, simple rules | Enterprise, dynamic zones |
| **Syntax** | `ufw allow 4242/tcp` | `firewall-cmd --add-port=4242/tcp` |
| **Persistence** | Automatic | Requires `--permanent` flag |

---

#### VirtualBox vs UTM

| Feature | VirtualBox (Used in this project) | UTM |
|---------|-----------------------------------|-----|
| **Platform** | Windows, macOS, Linux | macOS only (optimized for ARM) |
| **Architecture** | x86_64, some ARM support | ARM (Apple Silicon) and x86_64 |
| **Maturity** | Very mature, since 2007 | Newer, launched 2019 |
| **Performance (Intel)** | Excellent | Good |
| **Performance (ARM)** | Emulation (slower) | Native (excellent) |

---
