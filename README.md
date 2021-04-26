# linux-useful-commands
Just another reference to linux useful commands

### General

#### cat with grep and surrounding lines
```
cat tenant-backend.logs | grep "Starting the XML logging module" -C 50
# - -A (after)
# - -B (before)
# - -C (context [before + after])
```

#### Convert all files recursively
```
find . -type f -print0 | xargs -0 dos2unix
```

#### Clear and Edit File
```
cat /dev/null > interactive.sh && nano interactive.sh
```

#### Retrieve jobs running in background
```
#  If you have more than one process running in the background, do this:
jobs

# use the fg to get the job by the index 
fg %3
```

#### Locate File
```
sudo find . -type f -name "*.crt"
sudo find . -type f -name "FileName"
```

### SO UPDATES

#### Change the overcommit strategy
```
sysctl -a | grep overco
sysctl -w vm.overcommit_memory=1
sysctl -w vm.swappiness=1
```

#### Increase Open File Limit 
```
* - nofile 16384
```

```
/etc/security/limit.conf
# Edit the /etc/security/limit.conf file.
# Change the statement that specifies the value of nofiles to 8000.
# If you want the change to take effect in the current session, type ulimit -n 8000.
```

#### Get value on a base array
```
yum install jq
cat example.json | jq '.[] | .HostsPath'
```

#### Convert Files to Unix Recurssively
```
find . -type f -print0 | xargs -0 dos2unix
```

***

## Server Resources Check

#### PS order by Memory
```
ps aux  | awk '{print $6/1024 " MB\t\t" $11}'  | sort -n
ps aux  | sort -n
ps aux --sort -rss
ps aux | sort -rn -k 5,6
ps --sort -rss -eo rss,pid,command | head
ps aux | awk '{print $4"\t"$11}' | sort | uniq -c | awk '{print $2" "$1" "$3}' | sort -nr
```

#### Process Tree
```
pstree -hgcp
```

#### Hard Disk usage
```
df -h
```

#### Available RAM Memory
```
free -m
```

#### cpu, ram stats
```
vmstat -SM 1 100
```

#### shell alias  
```
alias ducks='du -cks * | sort -rn | head'
ducks
```

#### Test websocket with Curl
```
curl --include \
     --no-buffer \
     --header "Connection: Upgrade" \
     --header "Upgrade: websocket" \
     --header "Host: localhost:80" \
     --header "Origin: http://localhost:80" \
     --header "Sec-WebSocket-Key: SGVsbG8sIHdvcmxkIQ==" \
     --header "Sec-WebSocket-Version: 13" \
     http://localhost:80/
```

#### Copy ssh keys
```
ssh-copy-id -i ~/.ssh/mykey user@host
```

#### restart network interfaces
```
sudo ip addr flush interface-name
sudo systemctl restart networking
```

#### Check open ports without netstat or lsof
```
declare -a array=($(tail -n +2 /proc/net/tcp | cut -d":" -f"3"|cut -d" " -f"1")) && for port in ${array[@]}; do echo $((0x$port)); done
```

#### Sharing Folder 
```
mount -t cifs -o username=<change it> //whebs05/temp/Bifrost /root/tie/volumes/bifrost-server/shared/
```

#### Check Firewalld Status 
```
systemctl status firewalld.service
sudo firewall-cmd --permanent --zone=trusted --add-interface=docker0
sudo firewall-cmd --reload
```

#### Check Iptables Status
```
sudo iptables -L
sudo iptables -L --line-numbers
sudo iptables -A FORWARD -i docker0 -o eth0 -j ACCEPT
sudo iptables -A FORWARD -i eth0 -o docker0 -j ACCEPT
sudo iptables -I INPUT -j ACCEPT
```

```
sudo journalctl -fu NetworkManager
```

#### TCP Null Scan to fool a firewall to generate a response ##
```
## Does not set any bits (TCP flag header is 0) ##
nmap -sN 192.168.253.44
```

#### TCP Fin scan to check firewall ##
```
## Sets just the TCP FIN bit ##
nmap -sF 192.168.253.44
```

#### TCP Xmas scan to check firewall ##
```
## Sets the FIN, PSH, and URG flags, lighting the packet up like a Christmas tree ##
nmap -sX 192.168.253.44
```

#### Complete check
```
nmap -v -sS -A -T4 192.168.253.44
```

#### Realtime Packages Received
```
watch -n1 -d "iptables -vnxL | grep -v -e pkts -e Chain | sort -nk1 | tac | column -t"
```
