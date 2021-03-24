This vulnerability(CVE-2020-8558) allows attackers to connect node localhost service port inside k8s pod. 

利用该漏洞，可以访问当前K8S节点或其他节点(node)上所监听的Localhost端口（POC：https://github.com/tabbysable/POC-2020-8558）。   
  
*注：对非当前节点的攻击可能会受到云网络和VPC设计的影响。*

See more in 

1. https://github.com/kubernetes/kubernetes/pull/91666
2. https://github.com/kubernetes/kubernetes/issues/90259
3. https://unit42.paloaltonetworks.com/cve-2020-8558/

## Usage


run `./cdk evaluate`:

```
...
[Information Gathering - Sysctl Variables]
2021/01/20 16:07:02 net.ipv4.conf.all.route_localnet = 1

2021/01/20 16:07:02 You may be able to access the localhost service of the current container node or other nodes.
2021/01/20 16:07:02 CVE-2020-8558: The Kubelet and kube-proxy components in versions 1.1.0-1.16.10, 1.17.0-1.17.6, and 1.18.0-1.18.3 were found to contain a security issue which allows adjacent hosts to reach TCP and UDP services bound to 127.0.0.1 running on the node or in the node's network namespace. Node setting allows for neighboring hosts to bypass localhost boundary.
...
```

![image](https://user-images.githubusercontent.com/7868679/105202739-43b66a80-5b7d-11eb-92ad-c205a446ac7e.png)
