# Tool: ps

查看进程信息，类似linux `ps`命令。如果在进程列表中观察到容器中包含宿主机进程（docker run --pid=host），可结合ptrace权限注入宿主机进程逃逸。

Show process information like linux `ps` command. If host process occurs in docker(docker run --pid=host), try escaping docker by process injection with linux ptrace capability.


## Usage
```
cdk ps
```

## Example

![png](https://static.cdxy.me/20201130094717_5jU6wK_Screenshot.jpeg)