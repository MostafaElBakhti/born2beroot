*This project has been created as part of the 42 curriculum by mel-bakh*

## Description

Born2BeRoot is a system administration project that introduces the fundamentals of managing and securing a Linux server. The goal is to install and configure a virtual machine using Debian or Rocky Linux while following strict security rules.

This project teaches how to manage users and groups, configure sudo with security constraints, secure the system using UFW and SSH, and enforce strong password policies.

## Instructions

This project does not require compilation.

**To review the project:**
1. Launch the virtual machine using VirtualBox
2. Log in with the configured user credentials
3. Verify the system configuration:
   - User and group management
   - Sudo configuration and logging
   - Password policy rules
   - SSH configuration
   - Firewall (UFW) rules

## Resources

**References:**
- [Born2BeRoot GitBook](https://noreply.gitbook.io/born2beroot) – Project requirements and system configuration guide
- Linux documentation and online forums – For SSH, UFW, sudo, and password policy configuration

**AI Usage:**
- Clarifying Linux concepts and system administration principles
- Understanding error messages and troubleshooting configuration issues
- Improving documentation clarity

## Project Description

### Operating System Choice

I chose **Debian** because it is stable, lightweight, and well-suited for servers. Debian focuses on reliability and has clear documentation, making it ideal for learning system administration.

### Main Design Choices

**Partitioning:**
- Used **LVM (Logical Volume Manager)** with **encrypted partitions** for security and flexibility
- Disk structure:
  - `/boot` (976M) – Unencrypted boot partition
  - Encrypted partition (29.8G) containing LVM volume group `mel-bakh-vg`:
    - `root` (8.1G) – System files mounted on `/`
    - `swap_1` (1.6G) – Swap space for memory management
    - `home` (20.2G) – User data mounted on `/home`
- Encryption ensures data protection even if the physical disk is compromised
- LVM allows easy resizing and management of logical volumes without repartitioning

**Security Policies:**
- Strong password policies (expiration, complexity, reuse prevention)
- Restricted and logged sudo access
- Secured SSH (no root login, custom port)

**User Management:**
- Created non-root user with specific group assignments
- Disabled direct root login

**Services Installed:**
- SSH for remote access ufw sudo pam.d cron 
- Minimal services to keep the system secure and lightweight

### System Comparisons

**Debian vs Rocky Linux**
- **Debian:** Stable, simple, uses `apt` package manager. Great for learning.
- **Rocky Linux:** Enterprise-focused, RHEL-compatible, uses `dnf`. Better for production.
- **Choice:** Debian is easier to learn and configure.

**AppArmor vs SELinux**
- **AppArmor:** Simple profile-based security. Easy to understand.
- **SELinux:** Complex context-based security. More powerful but harder to manage.
- **Choice:** AppArmor is simpler for this project.

**UFW vs firewalld**
- **UFW:** Simple rule-based firewall. Straightforward configuration.
- **firewalld:** Dynamic zone-based firewall. More complex.
- **Choice:** UFW is better for minimal setups.

**VirtualBox vs UTM**
- **VirtualBox:** Cross-platform, widely used, officially supported at 42.
- **UTM:** Optimized for macOS and Apple Silicon.
- **Choice:** VirtualBox is more mature and flexible.

# proc/cons 