# ssh-bruteforce-lab
Simulation of SSH brute-force attacks on a Linux VM with log monitoring and localhost firewall defense

## Lab Objectives

Simulate failed SSH login attempts with invalid users.

Audit SSH activity using journalctl.

Monitor and count failed login attempts.

Secure localhost SSH access using firewall rules.

## Environment

OS: Kali Linux Rolling

Kernel: 6.18.5+kali-amd64

Virtualization: VirtualBox VM

SSH Server: OpenSSH

## Steps Performed
1. Test failed SSH logins
ssh fakeuser@localhost


Repeated attempts were made to generate failed login entries.

2. Audit SSH logs
journalctl -u ssh --since "10 minutes ago"
journalctl -u ssh | grep "Failed password"
journalctl -u ssh | grep "Invalid user"

3. Count failed attempts per IP
journalctl -u ssh | grep "Failed password" | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr

4. Block localhost SSH access with firewall
iptables -A INPUT -s 127.0.0.1 -p tcp --dport 22 -j DROP
ip6tables -A INPUT -s ::1 -p tcp --dport 22 -j DROP


Tested by trying to SSH again:

ssh localhost


Expected result: Connection denied, confirming firewall rules work.

5. Verify firewall rules
iptables -L INPUT -n --line-numbers
ip6tables -L INPUT -n --line-numbers


## Key Learnings

How SSH logs failed authentication attempts.

How to analyze logs using journalctl and grep.

How to count failed login attempts per IP address.

How to block SSH connections using iptables/ip6tables.

The importance of monitoring SSH activity for system security.
