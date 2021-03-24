检查挂载到当前容器内的目录和文件，一些挂载到容器内部的敏感目录如/etc,/root等可以提供逃逸机会，如将恶意代码写入/etc/crontab或/root/.ssh/authorized_keys.

Print directories and files which mounted to container. Some sensitive such as /etc,/root will offer you a chance to escape current container by modifying /etc/crontab or /root/.ssh/authorized_keys.

See also: https://github.com/Xyntax/CDK/wiki/Exploit:-mount-procfs

## Usage
```
cdk evaluate
```

## Output
![](https://static.cdxy.me/20201124175535_BY4KTU_Screenshot.jpeg)