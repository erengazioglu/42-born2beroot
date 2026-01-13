# Born2beroot

_This project has been created as part of the 42 curriculum by egaziogl._

## Description

Born2beRoot is an exercise in system administration using virtualization. The goal is to set up a secure Debian server with strict partitioning, password policies, SSH configuration, and automated monitoring—all following security best practices.

### Requirements

The project requires:
- Setting up a virtual machine with Debian (latest stable version)
- Configuring LVM with encrypted partitions
- Implementing strong password policies using PAM
- Configuring SSH to run on port 4242 with root login disabled
- Setting up UFW firewall with only port 4242 open
- Creating a monitoring script that runs every 10 minutes via cron
- Configuring sudo with strict rules and logging

Bonus requirements:
- Additional partitions beyond the mandatory ones (`/home`, `/var`, `/srv`, `/tmp`, `/var/log`)
- Setting up a functional WordPress website with lighttpd, MariaDB, and PHP

### The challenge

The main challenge lies in understanding Linux system architecture from the ground up—partitioning schemes, LVM, PAM modules, SSH security, firewall rules, and system monitoring. The project forces you to work without a graphical interface and to deeply understand each configuration file you modify.

### Implementation

**VM initial setup:**
- I used a SanDisk Usb 3.0 for my virtual machine.  It's not the best but I had it lying around and it worked out fine.
- I allocated 32gb to have plenty of room in case I do the bonus part & need more space than expected. 

**Disk partitioning:**
- `/boot` gets its own physical volume, whereas root (`/`) and other directories will live under the same LV (`sda5`).
- After reading so many forum discussions and blog posts ([1](https://www.reddit.com/r/linuxquestions/comments/w2vsgs/what_space_should_i_allocate_to_root_and_home/), [2](https://dev.to/jabulani_m/... )), I settled on: 
    - No separate partitions for `/bin`, `/usr`, `/dev` or `/media`, `/lib`.
    - Yes for `/swap`, `/home`, `/var`, `/srv`, `/tmp`.
    - a separate LV for `/boot` (EFI partition),
    - 2Gs for `/swap` (4 if extra performance is needed), 
    - a set amount of Gs for `/` (mileage may vary depending on necessity, but 10G seems enough), 
    - and the rest for `/home`.

**Final partition scheme (bonus):**
```
sda1    /boot       0.5 GB
sda5    /swap       2 GB
sda5    /           8.5 GB
sda5    /home       3 GB
sda5    /var        9 GB
sda5    /var/log    2 GB
sda5    /srv        5 GB
sda5    /tmp        2 GB
```

## Instructions

No compilation or integration required—this is a system administration project.

### Testing

**Password policies:**
```bash
sudo useradd test
sudo passwd test    # Try various passwords to test policy enforcement
```

**SSH connection:**
```bash
ssh -p 42422 egaziogl@localhost    # From host machine
```

**Firewall:**
```bash
sudo ufw status                    # Check active rules
```

**Sudo logging:**
```bash
sudo sudoreplay -d /var/log/sudo/ -l    # List sudo logs
sudo cat /var/log/sudo/log42            # View text logs
```

**System monitoring:**
```bash
sudo crontab -l                   # Verify cron job
# Wait for the script to run and check output
```

## Resources

**LVM & Partitioning:**
- [What is LVM?  (AskUbuntu)](https://askubuntu.com/questions/3596/what-is-lvm-and-what-is-it-used-for)
- [LVM Howto (The Linux Documentation Project)](https://tldp.org/HOWTO/LVM-HOWTO/)
- [The Linux Filesystem Explained (The Linux Foundation)](https://www.linuxfoundation.org/blog/blog/classic-sysadmin-the-linux-filesystem-explained)
- [FHS (Wikipedia)](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)
- [Swap space (Red Hat docs)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/storage_administration_guide/ch-swapspace#tb-recommended-system-swap-space)

**Password Policies:**
- [Configure minimum password length (Linux Audit)](https://linux-audit.com/authentication/configure-the-minimum-password-length-on-linux-systems/#login-settings)
- [Pluggable Authentication Modules (PAM) (Red Hat)](https://www.redhat.com/en/blog/pluggable-authentication-modules-pam)

**SSH & Security:**
- [OpenSSH (Ubuntu docs)](https://help.ubuntu.com/community/SSH/OpenSSH/Configuring)  
- [What is a computer port? (Cloudflare)](https://www.cloudflare.com/learning/network-layer/what-is-a-computer-port/)
- [Mandatory Access Control (NordLayer)](https://nordlayer.com/learn/access-control/mandatory-access-control/)
- [AppArmor Official Website](https://apparmor.net/)

**System Administration:**
- [Debian Handbook (Hostname configuration)](https://debian-handbook.info/browse/stable/sect.hostname-name-service.html)
- [Sudo configuration (Debian Wiki)](https://wiki.debian.org/sudo/)
- [Sudoers defaults cheatsheet (AskApache)](https://www.askapache.com/s/u.askapache.com/2012/09/sudoers-defaults-cheatsheet.txt)
- [Red Hat sudo rules](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/6/html/identity_management_guide/defining-sudorules)


## Project Description

All of the choices I made boil down to the same thing: simplicity and convenience, so that I can focus on the more "architectural" aspects of system administration rather than try to learn little details specific to services. VBox + Debian + AppArmor + UFW is a super straightforward choice and everything plays nicely out of the box.

### Debian vs Rocky Linux

I chose **Debian** for this project primarily due to its simplicity:
- APT (robust and well established package management system, which I was already familiar comfortable with)
- Extensive documentation
- Overall popularity

### AppArmor vs SELinux

It's a MAC (Mandatory Access Control) that comes pre-installed with Debian and is simple and intuitive. It's already working out of the box.

### UFW vs firewalld

Again, a question of simplicity:
- `sudo apt install UFW && sudo ufw enable` and it's already working.
- `ufw allow 4242` is as simple as it gets.

### VirtualBox vs UTM

I've already dabbled with VirtualBox and it's very simple to configure. I preferred learning system administration itself and what to do _inside_ virtualization rather than learn a new virtualization method.