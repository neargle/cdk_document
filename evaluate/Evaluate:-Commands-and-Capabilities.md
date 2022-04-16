检测容器内可用的linux命令以及linux capabilities，其中常用的linux命令如apt/yum, curl, wget, nc, python等会方便后续渗透流程，此外capabilities可以用于判断容器是否为特权容器，某些敏感的capabilities入CAP_SYSADMIN, CAP_NETADMIN, CAP_PTRACE等也可用来进行容器逃逸。

Detect available linux commands and capabilities inside container. Commands like apt/yum, curl, wget, nc, python, etc will do a great help to further penetration test, and capabilities like CAP_SYSADMIN, CAP_NETADMIN, CAP_PTRACE will offer you a chance to escape current container.

## Usage 
```
cdk evaluate
```

## Output
![](https://static.cdxy.me/20201124174954_ifNoct_Screenshot.jpeg)

## Configuration

Edit this file and rebuild CDK.
https://github.com/Xyntax/CDK/blob/main/conf/evaluate_conf.go

```
// check useful linux commands in container
var LinuxCommandChecklist = []string{
	"curl",
	"wget",
	"nc",
	"netcat",
	"kubectl",
	"docker",
	"find",
	"ps",
	"java",
	"python",
	"python3",
	"php",
	"node",
	"npm",
	"apt",
	"yum",
	"dpkg",
	"nginx",
	"httpd",
	"apache",
	"apache2",
	"ssh",
	"mysql",
	"mysql-client",
	"git",
	"svn",
	"vi",
	"capsh",
	"mount",
	"fdisk",
}
```