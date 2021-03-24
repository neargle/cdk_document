Source Code

```
├── cmd
│   └── cdk
│       └── cdk.go // binary main entry
├── conf
│   ├── evaluate_conf.go // for scripts in /evaluate
│   ├── exploit_conf.go // for scripts in /exploit
│   └── scanner_conf.go // for tool:probe
├── go.mod
├── go.sum
├── pkg
│   ├── evaluate // evaluate scripts
│   │   ├── ...
│   ├── exploit // exploit scripts
│   │   ├── ...
│   ├── kubectl // funcs for connecting K8s api-server
│   │   ├── common.go
│   │   └── kubectl.go // tool:kcurl
│   ├── lib
│   │   ├── banner.go // for --help
│   │   ├── parse.go // parse input args
│   │   └── plugin.go // plugin factory for module evaluate and exploit
│   ├── netcat // tool:nc
│   ├── network // tool:ifconfig
│   ├── probe // network probe for tool:probe
│   ├── ps // tool:ps
│   ├── util // common functions for reuse
│   └── vi // tool:vi
├── README.md
└── test // for go test

```