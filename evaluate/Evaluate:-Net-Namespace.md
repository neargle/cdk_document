# Evaluate: Net Namespace

判断容器是否与宿主机共享Net Namespace, 如果docker以`--net=host`启动且containerd-shim存在虚拟unix socket时，可通过CVE-2020-15257进行逃逸。

Check if container shares host's net namespace(e.g. `docker run --net=host`). Containers with host net namespace can be escaped by CVE-2020-15257 when containerd-shim abstract unix socket found.

## Usage
```
cdk evaluate
```

## Example
![png](https://static.cdxy.me/20201202095554_UBiXRZ_Screenshot.jpeg)