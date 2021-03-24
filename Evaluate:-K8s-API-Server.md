检查ENV信息判断当前容器是否属于K8s Pod，获取K8s api-server连接地址并尝试匿名登录，如果成功意味着可以直接通过api-server接管K8s集群。

Retrieve K8s cluster information from container ENV then try to auth K8s api-server with user system:anonymous. A successful authorization means you can takeover K8s cluster directly by sending commands to api-server.

## Usage 
```
cdk evaluate
```

## Output
![](https://static.cdxy.me/20201124180206_qWYvo6_Screenshot.jpeg)