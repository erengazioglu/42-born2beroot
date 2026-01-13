---
language: english
location: france
locale: USA
hostname: egaziogl42
domain name: (none)
non-root user full name: Eren Gazioglu

---
## USERS

root: Born42BeR00t!
egaziogl: TooGood2BeRoot
encrypted partition: 1337LVMpassword!


---
## PARTITIONS

sda1	/boot		.5
sda5	/swap		2
sda5	/		8.5
sda5	/home		3
sda5	/var		9
sda5	/var/log	2
sda5	/srv		5
sda5	/tmp		2

---
## INSTALLATIONS

install sudo by running `su -` (and inserting password for root)  
the `-` flag sets the correct path variables too so it's important  
then `usermod -aG sudo egaziogl` for allowing sudo access  
while you're at it, `groupadd user42` and `usermod -aG user42 egaziogl`
if you don't have a user, `useradd`  
`exit` to leave the root shell  
(you can use whereis to check if it exists)  

I went on and `sudo install man-db` as well, so I could read man pages directly  
then `sudo install openssh-server`  
and check with `sudo service ssh status`.  

Installed `iptables` to make sure there are no firewall blocking issues.

---
## CONFIGURATION

### Passwords

- Main article: `man 5 login.defs`
- Main article: [Configure minimum password length (Linux Audit)](https://linux-audit.com/authentication/configure-the-minimum-password-length-on-linux-systems/#login-settings)
- Side article: [Pluggable authentication modules](https://www.redhat.com/en/blog/pluggable-authentication-modules-pam)

Change passwords for the current user with just `passwd`.  
Change it for root with `su && passwd`.  
You can also run `passwd <username>` as root to change someone else's.

- `/etc/login.defs` hold some config but just for password age.
- `/etc/pam.d/common-password` is where modules used for password settings are defined. After installing pam_pwquality, you can run `grep pwquality /etc/pam.d/common-password` to see if it's managing the passwords.
- `pwquality` is a package to configure password quality settings: `sudo apt install libpam-pwquality` for install, `sudo vim /etc/security/pwquality.conf` for configuration
- For separate rules for root, I wrote into **/etc/pam.d/common_password**:
```
password [success=1 default=ignore] pam_succeed_if.so uid != 0
password requisite pam_pwquality.so difok=0
password requisite pam_pwquality.so difok=7
```
and in **/etc/security/pwquality.conf**:
```
minlen = 10
dcredit = -1
ucredit = -1
lcredit = -1 
maxrepeat = 3
usercheck = 1
retry = 3
enforce_for_root
```

To test, switch to root, then `useradd test`, and try out some passwords.  


### SSH

SSH Server (located at `/etc/ssh/sshd_config`):
- Port: 4242
- PermitRootLogin: no

SSH Client (located at `/etc/ssh/ssh_config`):
- Port: 4242

> restart the ssh service: `sudo service ssh restart && sudo service ssh status | grep 4242`

It didn't work from inside WSL but from powershell I was able to run `ssh -p 4242 egaziogl@localhost` and get access.

### UFW (Firewall)

`sudo apt install ufw` and `sudo ufw enable`.  
Now `ssh -p 4242 egaziogl@localhost` will timeout.  

`sudo ufw allow 4242` will create an ALLOW rule.  
After which, it will be possible to connect with ssh again.  
`sudo ufw status` to check.

### Hostname

- Main article: [Debian Handbook](https://debian-handbook.info/browse/stable/sect.hostname-name-service.html)

### Sudo rules & logging

- Main article: [Debian Wiki](https://wiki.debian.org/sudo/)
- Main article: [Cheatsheet (AskApache)](https://www.askapache.com/s/u.askapache.com/2012/09/sudoers-defaults-cheatsheet.txt)
- `man sudoers`
- Side article: [Red Hat Docs](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/6/html/identity_management_guide/defining-sudorules)

Any file in `/etc/sudoers.d` without a '.' or not ending with '~' will be parsed for config. I set the following:

```
Defaults log_input
Defaults log_output
Defaults logfile=/var/log/sudo/log42
Defaults iolog_dir=/var/log/sudo/
Defaults badpass_message="Duuuuude, that's not your password. Why don't you, like, type properly or something?"
Defaults passwd_tries=3
Defaults requiretty
```

You can check the logs by running: 
```bash
sudo sudoreplay -d /var/log/sudo/ -l    # lists sudo logs
sudo sudoreplay 00/00/0A                # example
```

---
## Study notes

### TTY (Teletypewriter)

The interface after installing debian is a TTY, which means there's no "scrollback" buffer.  
Pipe commands into `less` to be able to read their input.

### Apt

Advanced Package Tool, used to install, update, remove, manage packages, through the use of repositories.

- `sudo apt update` will update the APT package index.
- `sudo apt upgrade <?package-name>` will upgrade the installed packages to the latest versions. Optional `<?package-name>` will update only that specific package instead.
- Therefore commonly you see `sudo apt update -y && sudo apt upgrade -y`, where `-y` means "yes to all".
- `sudo apt install <package-name|package-location>` will install either over the internet (package-name) or a local .deb package (package-location).
- `sudo apt remove <package-name>` will remove a package.
- `sudo apt list --installed` will list all installed packages -> use `sudo apt list --installed | grep ssh` to check if openssh was installed, for example.

### SSH

_(study this later)_


### MAC & AppArmor

[Mandatory Access Control](https://nordlayer.com/learn/access-control/mandatory-access-control/)
[AppArmor Official Website](https://apparmor.net/)



---
## Things to learn about

GRUB Boot loader
SELinux?
AppArmor
aptitude vs apt
UFW
ssh, openssh
cron
awk
sed
