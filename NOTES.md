---
language: english
location: france
locale: USA
hostname: egaziogl42
domain name: (none)
non-root user full name: Eren Gazioglu

---
## USERS

root: born42beroot (actually should be Born42BeRoot)
egaziogl: toogood2beroot (actually should be TooGood2BeRoot)
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
and check with `sudo service ssh status`


---
## CONFIGURATION

SSH (located at `/etc/ssh/sshd_config`):  
- Port: 4242
- PermitRootLogin: no

---
## Study notes

### TTY (Teletypewriter)

The interface after installing debian is a TTY, which means there's no "scrollback" buffer. 

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
