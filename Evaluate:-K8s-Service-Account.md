K8s集群创建的Pod中，容器内部默认携带K8s Service Account的认证凭据(`/run/secrets/kubernetes.io/serviceaccount/token`)，CDK将利用该凭据尝试认证K8s api-server服务器并访问高权限接口，如果执行成功意味着该账号拥有高权限，您可以直接利用Service Account管理K8s集群。

By default, the containers of K8s Pods will carry a service account token in `/run/secrets/kubernetes.io/serviceaccount/token`, CDK will try to authorize K8s api-server with this service account token, and check if high-authority APIs are available. A successful connection means current Pod has a high-authority service account allows you to manipulate K8s cluster resources.


## Usage
```
cdk evaluate
```

## Output
![](https://static.cdxy.me/20201124180833_gXUojO_Screenshot.jpeg)