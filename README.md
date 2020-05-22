# Linux Server Hardning Tips

When you have services open to the internet, you must configure your Linux very well to make attacks difficult.

## Passwords

You must have difficult and big passwords, LIKE: 7MnhL12FgjbDas
```bash
# You can use passwd command to change
EXEMP: passwd USER
passord root
```

## Create non-ROOT user
```bash
sudo adduser <username>
#EXEMP
sudo adduser devve2kcc

# Add the user to SUDO group, to have permission to execute sudo tasks
sudo usermod -aG sudo devve2kcc
```

## SSH
If you use SSH, you can modify something to get a litle more security against brute-force and another attacks!
I recommend you change your ssh port, and do not allow root login via ssh.
You need to login with your non-ROOT user
```bash
# Open ssh config file with any editor
nano /etc/ssh/sshd_config

# Uncomment Port, and change ssh port, Like:
Port 2222

# Search for Authentication tab and change to this:
# This is the time in secounds you have since, start connecting to the server until you login.
# It will help with brute force attacks
# I leave between 35 and 45 seconds on my servers, but if you don't have a good internet, or your server is on the other side of the plant, increase the time.
LoginGraceTime 45

# Disable root login
# You must not allow root login, because, it is the first user, that a hacker will try to do bruteforce or login
PermitRootLogin no

# Disable X11Forwarding
# If you use SSH, you dont need X11forwad
X11Forwarding no

# MaxStartups
# then starting with 5 new connections pending authentication the op 60% of the new connections.
# By the time the backlog increases to 90 pending unauthenticated connections, 10% will be dropped.
# For that to happen it will have to change:
MaxStartups 5:60:10

# Now i like to Allow only a single user to login on SSH. non-ROOT
# All other users in the system, will not be able to enter ssh, if you want to give permission to another user, just add ahead
AllowUsers devve2kcc
# more than 1 users
#AllowUsers devve devve2
```

# Firewall
You can install UFW, is super simple to manage!
```bash
# Ubuntu/debian
sudo apt-get update
sudo apt-get install ufw

# Centos/ RHEL
sudo yum install epel-release
yum -y update
yum -y install ufw
```

#### Configuration
```bash
# Default rules
# Deny incomming connections
sudo ufw default deny incoming
# Allow outgoing connections
sudo ufw default allow outgoing

# ssh
# If you don't change a port, default port is 22.
# You can LIMIT and allow ssh connections using:
sudo ufw limit 22/tcp # Default Port
sudo ufw limit 2222/tcp # If you changed

# If you want to open HTTP and HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Enable UFW
sudo ufw enable
```

## Fail2ban
Fail2Ban helps you to keep your SSH server primarily protected from brute force and ddos ​​attacks. But it can also be used on http servers, webmail, and more.

You can follow my guide to setup Fail2ban
https://github.com/devve2kkcc/Fail2Ban-configuration

## Thanks

Donate: [Paypal](paypal.me/devve2k)
