*This project has been created as part of the 42 curriculum by mel-bakh.*

## Description


Born2BeRoot is a system administration project designed to introduce the fundamentals of managing and securing a Linux server. The objective of this project is to install and configure a virtual machine using Debian or Rocky Linux, following a set of strict rules.

Through this project, I learned how to manage users and groups, configure sudo with specific security constraints, secure the system using UFW and SSH, and enforce strong password policies.

## Instructions

This project does not require compilation or execution of a program.

To review the project, the virtual machine must be launched using a virtualization tool such as VirtualBox. Once the machine is running, the evaluator can log in using the configured user credentials and verify the system configuration.

The evaluation consists of checking:
- User and group management
- Sudo configuration and logging
- Password policy rules
- SSH configuration and restrictions
- Firewall (UFW) rules

## Resources

The following resources were used during the completion of this project:

- Born2BeRoot GitBook  
  https://noreply.gitbook.io/born2beroot  
  Used as a general reference to understand the project requirements and expected system configuration.

- Online articles and forums  
  Used to clarify specific configuration issues related to SSH, UFW, sudo, and password policies.

### Use of Artificial Intelligence

AI tools were used as a learning assistant to:
- Clarify Linux concepts and system administration principles
- Help understand error messages and configuration issues
- Improve the clarity and correctness of documentation text


### Description

I chose Debian for this project because it is stable, lightweight, and well suited for server environments. Debian prioritizes reliability over frequent updates, which makes it ideal for a system administration project focused on security and consistency. Its clear documentation and simplicity also make it easier to understand and control the system configuration without unnecessary complexity.


**Debian vs Rocky Linux**
- Debian focuses on stability and simplicity.
- Rocky Linux is more enterprise-oriented and closer to Red Hat systems.
- Debian is easier to configure for learning system administration.

**AppArmor vs SELinux**
- AppArmor uses simple, profile-based rules.
- SELinux relies on complex security contexts and stricter controls.
- AppArmor is easier to understand and manage.

**UFW vs firewalld**
- UFW provides a simple, rule-based firewall.
- firewalld is more dynamic and complex.
- UFW is better suited for minimal and secure setups.

**VirtualBox vs UTM**
- VirtualBox is widely used and officially supported at 42.
- UTM is optimized for macOS and Apple Silicon.
- VirtualBox offers more mature and flexible VM management features.


## Main Design Choices

- **Partitioning:**  
  LVM was used to allow flexible disk management and easier resizing of partitions if needed.

- **Security Policies:**  
  Strong password policies were enforced, sudo access was restricted and logged, SSH access was secured, and AppArmor was enabled to add an extra layer of system protection.

- **User Management:**  
  A non-root user was created and assigned to specific groups. Direct root login was disabled to reduce security risks.

- **Services Installed:**  
  Only essential services were installed, mainly SSH for remote access, in order to keep the system minimal and secure.
