# 🐧 Linux Notes for System Administration & DevOps

---

## 1. Understanding the Shell & Prompts

- `[ubuntu@ubuntu ~]$` → `username@hostname [current working directory]`
- `~` (tilde) → shorthand for home directory (`/home/username`)
- `#` → prompt of root user (superuser privileges)
- `$` → normal user prompt

👉 Switch to root:
```bash
sudo -i
````

👉 Check hostname:

```bash
hostnamectl
```

👉 Check system date/time:

```bash
date
```

👉 Exit session:

```bash
exit
# or Ctrl+D
```

---

## 2. File & Directory Management

👉 Print working directory:

```bash
pwd
```

👉 Change directories:

```bash
cd /etc       # go to /etc
cd -          # switch to previous directory
cd ..         # go up one level
cd ~username  # go to another user's home
```

👉 List files:

```bash
ls
ls -la        # list all with details
ls -lt        # sort by modification time
```

👉 Create & remove:

```bash
mkdir mydir
rmdir mydir
```

👉 Copy & move:

```bash
cp file1 file2
mv oldname newname
```

👉 View file contents:

```bash
cat file.txt
less file.txt
head -n 10 file.txt
tail -f logfile.log
```

👉 Locate files:

```bash
locate nginx.conf
updatedb   # update file database
```

---

## 3. User & Group Management

👉 Add new user:

```bash
sudo adduser devuser
```

👉 Change password:

```bash
passwd devuser
```

👉 Add user to group:

```bash
sudo usermod -aG sudo devuser
```

👉 List groups:

```bash
groups devuser
```

👉 Switch user:

```bash
su - devuser
```

---

## 4. Permissions & Ownership

Linux permissions: **read (r=4), write (w=2), execute (x=1)**

👉 Check permissions:

```bash
ls -l
# drwxr-xr-x  2 user group size date filename
```

👉 Change permissions:

```bash
chmod 644 file.txt
chmod +x script.sh
```

👉 Change ownership:

```bash
sudo chown user:group file.txt
```

👉 Common chmod values:

| Command     | Meaning              | Best Practice          |
| ----------- | -------------------- | ---------------------- |
| `chmod 777` | Full access to all   | ❌ Avoid (insecure)     |
| `chmod 755` | Owner rwx, others rx | ✅ Good for executables |
| `chmod 644` | Owner rw, others r   | ✅ Good for configs     |
| `chmod 600` | Owner rw only        | ✅ Use for private keys |

---

## 5. Process Management

👉 List processes:

```bash
ps aux
top
htop   # (if installed)
```

👉 Kill process:

```bash
kill -9 PID
```

👉 Background jobs:

```bash
command &        # run in background
jobs             # list
fg %1            # bring job 1 to foreground
```

👉 Systemd services:

```bash
systemctl status nginx
systemctl start nginx
systemctl stop nginx
systemctl enable nginx
```

---

## 6. System Monitoring & Performance

👉 Disk space:

```bash
df -h
du -sh /var/*
```

👉 Memory:

```bash
free -h
```

👉 Uptime/load:

```bash
uptime
```

👉 Hardware:

```bash
lscpu
lsblk
```

👉 Kernel logs:

```bash
dmesg | less
```

---

## 7. Networking

👉 Show IP info:

```bash
ip a
```

👉 Test connectivity:

```bash
ping google.com
traceroute google.com
```

👉 Open ports:

```bash
ss -tulnp
```

👉 Download files:

```bash
curl -O https://example.com/file
wget https://example.com/file
```

👉 Firewall:

```bash
sudo ufw enable
sudo ufw allow 22/tcp
sudo ufw status
```

---

## 8. Package Management

### Debian/Ubuntu

```bash
sudo apt update && sudo apt upgrade
sudo apt install nginx
sudo dpkg -i package.deb
```

### RHEL/CentOS/Fedora

```bash
sudo yum install nginx
sudo dnf update
sudo rpm -ivh package.rpm
```

### Snap

```bash
sudo snap install hello-world
```

---

## 9. Logs & Systemd

👉 Logs:

```bash
journalctl -xe
journalctl -u nginx
```

👉 Log locations:

* `/var/log/syslog`
* `/var/log/auth.log`
* `/var/log/nginx/error.log`

---

## 10. Storage & Filesystems

👉 View disks:

```bash
lsblk
fdisk -l
```

👉 Mount:

```bash
sudo mount /dev/sdb1 /mnt/data
```

👉 Persistent mount (edit `/etc/fstab`):

```fstab
/dev/sdb1   /mnt/data   ext4   defaults   0 2
```

👉 LVM basics:

```bash
pvcreate /dev/sdb
vgcreate myvg /dev/sdb
lvcreate -L 10G -n mylv myvg
mkfs.ext4 /dev/myvg/mylv
```

---

## 11. Archiving & Compression

👉 Create archive:

```bash
tar -cvf archive.tar dir/
```

👉 Extract:

```bash
tar -xvf archive.tar
```

👉 Compress:

```bash
gzip file
bzip2 file
```

👉 Rsync & SCP:

```bash
rsync -avz file user@host:/path/
scp file user@host:/path/
```

---

## 12. Cron & Scheduling

👉 Edit crontab:

```bash
crontab -e
```

👉 Example job (every day at 2 AM):

```cron
0 2 * * * /home/user/backup.sh
```

👉 List jobs:

```bash
crontab -l
```

👉 Run one-time job:

```bash
at now + 1 hour
```

---

## 13. Security & SSH

👉 Generate key:

```bash
ssh-keygen -t rsa -b 4096
```

👉 Copy key:

```bash
ssh-copy-id user@host
```

👉 Secure server:

* Disable root login (`/etc/ssh/sshd_config`)
* Use `ufw` or `firewalld`
* Use `fail2ban` to prevent brute force

---

## 14. DevOps Tools Integration

👉 Docker on Linux:

```bash
docker ps
docker run hello-world
```

👉 Podman:

```bash
podman run -it ubuntu bash
```

👉 Kubernetes:

```bash
kubectl get pods
```

👉 Ansible:

```bash
ansible all -m ping -i inventory
```

---

## 15. Best Practices

✅ Always use `sudo` instead of root login
✅ Regularly update & patch (`apt update && apt upgrade`)
✅ Monitor logs with `journalctl`
✅ Use `htop` and `iotop` for performance debugging
✅ Secure SSH (disable password auth, use keys only)
✅ Automate backups with cron + rsync
✅ Use least privilege for file permissions
✅ Document changes in `/etc/` for team visibility

---


Do you want me to also add **real-world troubleshooting scenarios** (like “disk full”, “service not starting”, “high CPU”) with solutions at the end of `linux-notes.md`?
```
