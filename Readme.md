
# Remote Gateway Manager & Client

<p align="center">
<img width="531" height="186" src="https://github.com/RakshitAdmar/gwCfgServer/blob/master/docs/RemoteGatewayManager.png">
</p>

## What it does 
1. Remote device  requests a gateway manager for an open port and registers it's public ip and mac address. 
2. Upon receipt of a free port on the manager server, remote device creates an autossh pipe which the manager can use to reverse ssh to the remote.

---

## Client Bash Dependedncies 
1. jq
2. wget

## Server Dependencies 
1. MongoDB
2. GOlang

---

## HOWTO

### Key exchange
1. Generate RSA keys in both manager server and remote device `ssh-keygen -t rsa`
2. Store the public keys in `~/.ssh/authorized_keys` for both manager and remote. (Remote side, managers key is not necessary if login with password is preferred)

### Client Side
1. Store the folder cleintSide and give it user permssions
2. Rename primary interface name in clientSide/master.sh, for e.g "eth0"
3. "touch ip" & "touch port" in the working directory. This is where public ip addres is cached
4. Use crontab -e to point to this script. Make it execute every hour or so. If ip remains same then a request won't be made
   An example of cronjob is as follows
	> @reboot su username -c 'sh /home/username/clientSide/master.sh' >> /home/username/clientSide/logs/cronlog 2>&1
5. Ensure that the parameters `ServerAliveInterval 60` and `ServerAliveCountMax 10` in file `/etc/ssh/ssh_config are set on the client side are present, to ensure persistent tunnel



### Server Side

1. Make sure go is set and go env points to correct GOPATH, etc.
2. Change the listening port in `main.go`
3. Change mongodb credenials, database, etc and `db/db.go`
4. Clone repo and run `go install . `
5. Run the binaries from wherever they are generated to. 
6. Ensure that the parameters `ClientAliveInterval 60` and `ClientAliveCountMax 10` in file `/etc/ssh/sshd_config are set on the client side are present, to ensure persistent tunnel

## TODO :

1. Daemon to monitor change in pub ip and 
2. Security imporvements
3. Incorporate Ansible
3. Daemon to monitor SSH tunnel
