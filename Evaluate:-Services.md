从ENV和进程信息中提取容器内的敏感服务，如python,ssh等，便于部署后续逃逸/持久化攻击。

Detect sensitive service running in container such as Python, SSH, etc. It's convenient to do further escape or persistence with these service.

# Usage 
```
cdk evaluate
```

# Output
![](https://static.cdxy.me/20201124174158_FCmAEO_Screenshot.jpeg)

# Configuration

Edit this file and rebuild CDK. https://github.com/Xyntax/CDK/blob/main/conf/evaluate_conf.go

```
// match ENV to find useful service
var SensitiveEnvRegex = "(?i)\\bssh_|k8s|kubernetes|docker|gopath"

// match process name to find useful service
var SensitiveProcessRegex = "(?i)ssh|ftp|http|tomcat|nginx|engine|php|java|python|perl|ruby|kube|docker|\\bgo\\b"

```