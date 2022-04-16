# Tool: probe

自定义TCP扫描IP/端口。

Custom TCP IP/Port scan.

See also : https://github.com/Xyntax/CDK/wiki/Exploit:-service-probe


## Usage

```
cdk probe <ip> <port> <parallel> <timeout-ms>

<ip> : 扫描单个IP类似'1.1.1.1'，或者扫描IP段类似'1.1.1.1-255'
<port> : 要扫描的端口，类似'22,80,100-200'
<parallel> : 最大并发数，过高会触发linux `too many open files` error, 如果不确定可以使用50
<timeout-ms> : 每个TCP连接的超时时间（毫秒），如果不确定可以使用1000
```

```
cdk probe <ip> <port> <parallel> <timeout-ms>

<ip> : single ip like '1.1.1.1' or ip range like '1.1.1.1-255'
<port> : port list like '22,80,100-200'
<parallel> : the max parallels, too high will trigger linux `too many open files` error, use 50 if you are not sure.
<timeout-ms> : timeout(millisecond) for each TCP connection, use 1000 if you are not sure.
```

## Example

![png](https://static.cdxy.me/20201127175056_KPpVvG_Screenshot.jpeg)

