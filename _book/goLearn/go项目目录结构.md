```shell
➜  goproject tree
.
├── classes  //逻辑层，处理事务
├── config  //基本项目配置
├── controller  //处理接受前端请求
├── go.mod  
├── go.sum
├── main.go
├── model  //模型层，数据库表
├── param  //请求参数封装验证
└── tool  //工具类

6 directories, 3 files

# 项目启动
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o build/main-linux main.go
```

