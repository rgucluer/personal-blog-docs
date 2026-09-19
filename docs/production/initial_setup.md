# Initial Setup of a Virtual Private Server

Read documentation of your VPS Service Provider, and make root user able to login via ssh without a password (via public key) .

## [Create & Add ssh keys](create-ssh-keys-prod.md)

- If you want to change the SSH port, wait after coolify initial setup.  Change ssh port after you create Coolify root user and connected to Coolify Dashboard.

- ssh to VPS
- Update apt packages

## Install apt packages
Install packages
- lego
- nano
- curl
- python3-pip
- ncdu

```bash
$ apt install lego nano curl python3-pip ncdu
```

## Set hostname on VPS

```bash
$ hostnamectl set-hostname < server-name >
```

```bash
$ nano /etc/hosts
```

```bash
127.0.1.1 www.< prod-domain-name >
127.0.1.1 traefik.< prod-domain-name > coolify.< prod-domain-name >
127.0.1.1 < servername >.< prod-domain-name > < prod-domain-name > < servername >
127.0.0.1 localhost
.....
```
Save, and exit.

Then, restart the SSH service and the new hostname is set:
```bash
$ systemctl restart ssh
```

Example: serv1.my-domain.com


## Change timezone

https://linuxconfig.org/ubuntu-24-04-change-timezone

Status
```bash
$ timedatectl
```

Find your Timezone
```bash
$ timedatectl list-timezones
```

Use up/down arrow keys, PgUp/PgDown keys. Search for your timezone. Copy or note the string.


Set New Timezone
```bash
$ timedatectl set-timezone < timezone >
```

Edit /etc/ssh/sshd_config. After you succesfully connected with public ssh keys, 
set PasswordAuthentication, and KbdInteractiveAuthentication to no .
It will be even better if you limit users who can ssh with AllowUsers directive.


```bash
.....
# Set your ssh port here, Open this port in your firewall
Port 22
MaxAuthTries 6
PermitRootLogin prohibit-password
PubkeyAuthentication yes
# PasswordAuthentication no
# KbdInteractiveAuthentication no
UsePAM yes
X11Forwarding no
.....
# TCPKeepAlive yes
# ClientAliveInterval 60
# ClientAliveCountMax 3
.....
# AllowUsers < vps-user-name > root
```

Enter < vps-user-name > without angle brackets

```bash
$ systemctl restart ssh
```

TODO: It would be better if we can use Coolify with a regular user.

Do not close current terminal, open a new terminal window, try to ssh in without entering your password, if succesfull then you can close the first terminal .

---

### Continue with: Set swap [set-swap](../install-personal-blog-prod.md#set-swap-set-swap)

---
### References
- https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys

- https://superuser.com/questions/8077/how-do-i-set-up-ssh-so-i-dont-have-to-type-my-password

- https://askubuntu.com/questions/46930/how-can-i-set-up-password-less-ssh-login
