# Run Test in Containers/K8s Pods

Most of CDK functions are designed to run inside containers/pods with different privileges.  
  
 https://github.com/Xyntax/CDK/tree/main/test 

This script helps to simulate a victim container in remote host, build CDK source and upload binary to test.

## Install Requirements

* Local requirement: Python3.6+, configured kubectl with cluster-admin privilege
* Remote Test Server: Linux with docker installed, K8s cluster

```
cd CDK-deploy-test
pip install -r requirements.txt
```

## Config
Edit lib/conf.py add your test server addr and local sourcecode dir.

```
class SERVER:  # your remote server for test
    HOST = '39.104.80.49'
    USER = 'root'
    KEY_PATH = '/Users/xy/.ssh/lezhen-cdk.pem'


class CDK:
    # local source-code dir to run `go build`
    BUILD_PATH = '/usr/local/go/bin/src/github.com/Xyntax/CDK/cmd/cdk'
    # build command
    BUILD_CMD = 'cd {} && CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build cdk.go'.format(BUILD_PATH)
    # binary after build
    BIN_PATH = '/usr/local/go/bin/src/github.com/Xyntax/CDK/cmd/cdk/cdk'

    # you can keep it unchanged
    REMOTE_HOST_PATH = '/root/cdk-fabric'
    REMOTE_CONTAINER_PATH = '/cdk-fabric'


class K8S:
    # upload cdk to target pod then check command output using kubectl
    TARGET_POD = 'myappnew'
    REMOTE_POD_PATH = '/cdk-fabric'

```

## Run Test
```
python test_main.py
```

codes:
```
/lib/conf.py : for configuration  
/lib/ssh_remote_action.py: control remote server to run CDK commands.  
/lib/k8s_remote_action.py: control remote k8s cluster to run CDK commands. 
/test_main.py : entry, with all test commands and excepted outputs.  
```
