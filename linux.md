1. File & Directory Operations
List files and directories
ls
ls -l
ls -lr
ls -la
ls -lh
ls -lrth

2. Change directories
cd /path/to/dir
cd ..
cd ~
pwd
sudo su -

3. Create files and directories:
touch filename
mkdir dir_name

4. Remove files and directories
rm filename
rm -rf dir_name
rmdir dir_name --> empty directory


5. Copy and move files
cp source destination
mv source destination
Rename files
mv oldname newname

6. dowload urls and delimiter
wget url:normal download
cut url : file format
cut -d :-f1
awk -f "/{print $1F}"

7. vim editors
vim filename
:/ top search
:? search from bottom
: set nu , no.of lines
:set nonu remove lines no.
:noh 
:q
:q! force quit without saving
:wq! save and quit
:s/sourceword/replsaceword
2s,%s --2words ,all words

esc mode:
u -undo
yy -copy
p -paste
dd -delete
10p -paste 10 times
g 2 times --top
shift+g --> bottom

8. Find files
find /path -name filename
find /path -type f


2. File Permissions
Check file permissions
ls -l filename —particular file.
Change file permissions
chmod 755 filename
chmod +x filename
chmod -x filename
Change file owner/group
chown user:group filename


3. Process Management
List processes
ps aux →list of all running processers .(ps :list process ,x :detach to terminal,u : human readable formate, a: process from all users)
ps -ef {all active process}
ps - running process.
Kill processes
kill PID
kill -9 PID


Monitor system resources:
cpu: 
top
Htop –u need to install this

Background processes
command & →running process in background
foreground process: sleep 12 
background sleep 10 &
Jobs →list of jobs running in background.
fg %1→bring back to the foreground and job id


4. System Monitoring
Disk usage
df -h
du -sh /path/to/dir →particular directory/file of  size.
Memory usage
free -h
Check system uptime
Uptime
systemctl,systemd,start,stop,restart,end


5. Network information/management{service listening on which port}
Ifconfig → ipv4,broadcast address,netmask address
ip a – ip address and network interface
Network connections
netstat -tuln
ss -tuln


5. Package Management
Debian/Ubuntu
sudo apt update
sudo apt upgrade
sudo apt install pkg
sudo apt remove pkg
sudo apt search pkg
RedHat/CentOS
sudo yum update
sudo yum install pkg
sudo yum remove pkg
sudo yum search pkg
Check installed packages
dpkg -l  →list of debeain and ubuntu
rpm -qa →redhat


6. Networking and SSH
SSH into a remote server
ssh username@hostname_or_ip
ssh -i /path/to/key username@hostname_or_ip
Transfer files via SCP
scp local_file username@hostname:/path/to/destination
scp username@hostname:/path/to/source local_file
Generate SSH key pair
ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa # Generate an SSH key pair


7. Logs and System Diagnostics
View logs
tail -f /var/log/syslog
tail -n 100 /var/log/syslog
head
Search logs
grep "keyword" /var/log/syslog

wget <url>{file formate},curl<url>{print formate}
cut -d : -f1
cut -d / -f2
awk -F "/" '{print $1F}'

System diagnostics:

dmesg
Journalctl


8. User Management
Add a user
sudo useradd username
sudo passwd username
Delete a user
sudo userdel username
sudo userdel -r username
Add user to a group
groupadd groupname
sudo usermod -aG groupname username
List all users
cat /etc/passwd

9. Archiving and Compression
Create a compressed archive
tar -cvf archive.tar directory
tar -czvf archive.tar.gz directory

10.inode, symlink,hardlink.
11. crontab:

User/Application         : communication.
      ↓
Operating System
        ↓
Kernel
        ↓
Hardware
(CPU, RAM, Disk, Network)

12. Term	Real-Life Example
Image	Photocopy of complete setup
Snapshot	Photo taken at one moment
Template	Ready-made design format

 “An image is a complete machine setup used to create new instances, a snapshot is a point-in-time backup of storage data, and a template is a reusable predefined VM configuration used for fast provisioning.”
 
process is occupying maximum memory/find and kill process taking max memory : ps aux --sort=%mem | grep head -n 5 , kill -9 pid
2. find recent changes in directory files: find /path/to/dir -type f -mmin -30
find /path/to/dir -type f -mtime -1 (f find files)
3. setup a temporary user within a expiration time: sudo useradd -e YYYY-MM-DD -m tempuser
-e YYYY-MM-DD: Expiration date (after which the account is disabled)
-m: Create home directory for the user

4. check who occupied the port: sudo lsof -i :<port-number>
5. find all cron job for a specific user: crontab -u <username> -l

6. copy specific files to take backup of: ✅ Use rsync for More Robust Backups
rsync -av /etc/nginx/nginx.conf /etc/ssh/sshd_config /backup/configs/
-a: Archive (preserve permissions, timestamps, etc.)
-v: Verbose

7. find who is using the max cpu: top -o %cpu
8. logs of before and during reboot of vm: sudo reboot 
Journalctl -b -1 -->see logs
9. finding zombie process: ps aux | awk '$8 ~ /Z/ { print $0 }'
awk '$8 ~ /Z/ { print $0 }'

awk: Pattern scanning and processing tool.
$8: Refers to the 8th column, which is the STAT (process state) field.
~ /Z/: Matches if the state contains the letter Z, indicating a zombie.
{ print $0 }: Print the entire line if the condition is true.

 zombie process is:

A terminated process that has not been cleaned up by its parent.
It still has an entry in the process table.

10. how long system has been up: uptime

11. find all excutables files in directory: find foldername -type f -excutable
12. how many active and establishes tcp connections: ss -tn state established | wc -l
13. check inode usage: 📦 What Is an Inode in Linux?
An inode (index node) is a data structure used by Linux/Unix filesystems (like ext4, xfs) to store metadata about a file, not the file name or data itself.

ls -i filename
14. watch logs of live application in realtime: tail -f /path/to/logfile.log
15. dns resolution in linux command: nslookup domainname
16. limit download speed for downloading something: wget --limit-rate=1
17. broadcast shutdowns in vm: sudo shutdown -h +5 "System will shut down in 5 minutes for maintenance. Please save your work."
18. check which users are currently logged in: w
19. kill all process of a user: pkill -u username
20. live logs of any service in linux: journalctl -u servicename -f
21. monitor the process tree: pstree -p


1. System Crash / Reboot
Scenario: A production server suddenly reboots without warning.
 Question:
What steps would you take to investigate the root cause of the unexpected reboot?
Expected Approach:
Check last / last -x logs
Look in /var/log/messages, dmesg, or journal logs
Check ipmi-sel or hardware logs (if bare metal)
Investigate OOM killer or kernel panic
Check for power supply or thermal issues

2. High CPU Usage
Scenario: Your Linux server CPU usage spikes to 100%, affecting response time.
 Question:
How do you identify the root cause and mitigate it in real-time?
Expected Tools:
top, htop, ps aux --sort=-%cpu
perf, pidstat, strace, lsof
Kill/restart hung processes
Analyze logs and cron jobs

3. Slow Boot Issue
Scenario: A Linux VM takes more than 5 minutes to boot.
 Question:
How would you troubleshoot and reduce the boot time?
Steps May Include:
Use systemd-analyze blame and systemd-analyze critical-chain
Disable unnecessary services with systemctl disable
Check dmesg and hardware/driver issues
Examine fstab delays, network timeouts

4. Disk Space Full
Scenario: / is full and critical services are failing.
 Question:
What would you do immediately, and how would you prevent this in the future?
Real-Time Fixes:
du -shx /* | sort -h or ncdu
Remove logs (/var/log), cache (/var/cache), rotate logs
Find large files: find / -type f -size +500M

5. Network Latency or Drops
Scenario: Application latency increases and packets are intermittently dropping.
 Question:
How do you diagnose and isolate the network issue?
Expected Tools/Steps:
ping, mtr, traceroute, ss -tulnp
Check interface errors with ethtool, ifconfig, ip -s link
Use tcpdump to capture traffic

6. SSH Access Denied
Scenario: You’re suddenly locked out of a production server via SSH.
 Question:
What could be the causes, and how would you restore access?
Potential Causes & Fixes:
Misconfigured sshd_config
Expired user credentials
Firewall/DNS/network issues
Use console access / out-of-band management (iLO, iDRAC)

7. Service Not Starting via systemd
Scenario: A custom service you wrote fails to start with systemd.
 Question:
How do you debug and fix the service?
Key Steps:
systemctl status myservice
journalctl -xe
Verify ExecStart, Environment, Permissions

8. Memory Leak in Application
Scenario: An in-house app slowly consumes all RAM.
 Question:
How would you detect, debug, and mitigate the memory leak?
Tools:
top, free -m, vmstat, ps aux --sort -rss
Use valgrind, smem, pmap, gcore for debugging
Use ulimit, cgroups, or restart policies as a workaround

