# SSH Brute-Force Protection with Fail2ban

## Goal
Use Fail2ban to protect SSH by banning an IP address after repeated failed login attempts.

## Lab Setup
- Ubuntu Server
- Kali Linux
- VMware Workstation
- OpenSSH
- Fail2ban

## Configuration
Edit the local Fail2ban configuration:

```bash
sudo nano /etc/fail2ban/jail.local
```

Example SSH jail:

```ini
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
findtime = 10m
bantime = 3600
```

Check the configuration and restart Fail2ban:

```bash
sudo fail2ban-client -t
sudo systemctl restart fail2ban
sudo fail2ban-client status sshd
```

## Testing
From the Kali VM, make a few failed SSH login attempts against the Ubuntu VM in your lab.

Check the SSH jail and logs:

```bash
sudo fail2ban-client status sshd
sudo grep "Failed password" /var/log/auth.log | tail -10
sudo journalctl -u fail2ban --no-pager -n 30
```

## Result
When the SSH jail is enabled and the configured number of failed attempts is reached, Fail2ban should ban the source IP for the configured ban time. Verify the ban in the jail status and Fail2ban logs.

## Screenshots
Lab screenshots go in the `screenshots` folder.
