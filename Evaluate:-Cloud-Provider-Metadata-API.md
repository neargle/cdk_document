# Evaluate: Cloud Provider Metadata API

探测云厂商内置的metadata接口，从该接口可以获取到服务器VM的基础信息如OS版本、CPU及网络、DNS配置等，少数情况下可以发现用户在metadata中自定义的信息。

Discover available metadata APIs of different cloud service provider. Basic information of target VM like OS,CPU,DNS,CIDR can be found in these APIs. User-defined information can also be found if you're lucky.

## Usage 
```
cdk evaluate
```

## Example
![](https://static.cdxy.me/20201201113837_sBt38B_Screenshot.jpeg)

## Configuration

https://github.com/Xyntax/CDK/blob/main/conf/evaluate_conf.go

```
// Check cloud provider APIs in evaluate task
type cloudAPIS struct {
	CloudProvider string
	API           string
	ResponseMatch string
	DocURL        string
}

var CloudAPI = []cloudAPIS{
	{
		CloudProvider: "Alibaba Cloud",
		API:           "http://100.100.100.200/latest/meta-data/",
		ResponseMatch: "instance-id",
		DocURL:        "https://help.aliyun.com/knowledge_detail/49122.html",
	},
	{
		CloudProvider: "Azure",
		API:           "http://169.254.169.254/metadata/instance",
		ResponseMatch: "azEnvironment",
		DocURL:        "https://docs.microsoft.com/en-us/azure/virtual-machines/windows/instance-metadata-service",
	},
	{
		CloudProvider: "Google Cloud",
		API:           "http://metadata.google.internal/computeMetadata/v1/instance/disks/?recursive=true",
		ResponseMatch: "deviceName",
		DocURL:        "https://cloud.google.com/compute/docs/storing-retrieving-metadata",
	},
}
```