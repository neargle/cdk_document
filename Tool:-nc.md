# Tool: nc

还原部分linux nc命令的功能，用于传输文件以及正反向shell。

Create TCP tunnel like linux command `nc` for sending file or spawn an interactive shell.

Copied from https://github.com/jiguangin/netcat

## Usage
```
usage: netcat [-l] [-v] [-p port] [-n tcp]

options:
  -e	shell mode
  -h string
    	host addr to connect or listen (default "0.0.0.0")
  -help
    	print this help
  -l	listen mode
  -n string
    	network protocol (default "tcp")
  -p int
    	host port to connect or listen (default 4000)
  -v	verbose mode
```

# Example

Listen & Connect
```
listen: 
cdk nc -l -v -p <port>

connect:
cdk nc -h <host> -p <port>
```
![png](https://static.cdxy.me/20201130110151_dfGaQr_Screenshot.jpeg)


Send File
```
cdk nc -l -v -p <port> < file
cdk nc -h <host> -p <port> > file
```
![png](https://static.cdxy.me/20201130110313_lFlvEp_Screenshot.jpeg)


Shell (listen)
```
cdk nc -l -v -p <port> -e
cdk nc -h <host> -p <port>
```
![png](https://static.cdxy.me/20201130110634_ZQyHMu_Screenshot.jpeg)

Shell (reverse)
```
cdk nc -l -v -p <port>
cdk nc -h <host> -p <port> -e
```
![png](https://static.cdxy.me/20201130110759_MTVBms_Screenshot.jpeg)
