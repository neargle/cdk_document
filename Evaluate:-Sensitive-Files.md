进行全盘路径扫描，在路径中匹配敏感词来识别敏感文件，识别到的敏感文件如 docker.sock, .git, .kube 等将对后续渗透带来帮助。

Deploy a full-disk file path scan, matched sensitive files will do a great help to further pentest.

## Usage

```
cdk evaluate --full
```

## example
![](https://static.cdxy.me/20201124202409_gKW3AL_Screenshot.jpeg)

## Configuration

Edit this file and rebuild CDK.
https://github.com/Xyntax/CDK/blob/main/conf/evaluate_conf.go


```
var SensitiveFileConf = sensitiveFileRules{
	StartDir: "/",
	NameList: []string{
		`docker.sock`,
		`.kube/`,
		`.git/`,
		`.svn/`,
		`.pip/`,
		`/.bash_history`,
		`/.bash_profile`,
		`/.bashrc`,
		`/.ssh/`,
		`.token`,
		`/serviceaccount`,
		`.dockerenv`,
		`/config.json`,
	},
}

```