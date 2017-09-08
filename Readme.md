
# Remote Gateway Manager & Client


![alt text](https://github.com/RakshitAdmar/gwCfgServer/blob/master/docs/RemoteGatewayManager.png "Architecture overview")


## What it does 
1. Remote device  requests a gateway manager for an open port and registers it's public ip and mac address. 
2. Upon receipt of a free port on the manager server, remote device creates an autossh pipe which the manager can use to reverse ssh to the remote.

## Steps:-

### Client Side

#### Bash Dependedncies 
1. jq
2. curl
3. autossh



1. Rename primary interface name in clientSide/establish.sh, for e.g "eth0"
2. "touch ip" in the working directory. This is where public ip addres is cached
3. Use crontab -e to point to this script. Make it execute every hour or so. If ip remains same then a request won't be made


### Server Side

#### Dependencies 
1. MongoDB
2. GOlang


1. Make sure go is set and go env points to correct GOPATH, etc.
2. Change the listening port in `main.go`
3. Change mongodb credenials, database, etc and `db/db.go`
4. Clone repo and run `go install . `
5. Run the binaries from wherever they are generated to. 

## TODO :

1. Daemon to monitor change in pub ip and 
2. Security imporvements
