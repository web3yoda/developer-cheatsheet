---
weight: 132
title: golang
---

# golang

## go mod

mod就是module的缩写，顾名思义管理go 的module

```shell

# 新项目创建 go.mod 
go mod init

# 根据代码里引用的包，自动更新或者移除 module
go mod tidy

# 自动下载module
go mod download

# 查看依赖图

go mod graph

```

## go build

```shell

# 新建个简单的go程序
mkdir mytool && cd mytool

cat <<EOF > main.go
package main

func main() {
  println("Hello Yoda!")
}
EOF

# 直接运行
go run main.go
Hello Yoda!

# 编译运行
go build main.go
./main
Hello Yoda!

# 指定output bin文件
go build -o mytool main.go
./mytool
Hello Yoda!

# 添加链接器参数, ld 指 linker，移除debug信息使得 binary更小
du -h mytool
1.1M	mytool

go build -o mybin -ldflags "-w -s" main.go
du -h mytool
844K	mytool

# 编译linux平台的binary
GOOS=linux go build -ldflags "-w -s" -o mytool main.go
file mytool
mytool: ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), statically linked, Go BuildID=FURGh_kizf7q2mSRhtEy/WzwE086pXTlDzXRm4tTS/s9VALqNxdEK6f6aWA4Hc/PEfftgeMk8zdvOg_KZhR, stripped

# 编译mac amd64 / arm64版本
GOARCH=amd64 go build -ldflags "-w -s" -o mytool main.go
file mytool
mytool: Mach-O 64-bit executable x86_64

GOARCH=arm64 go build -ldflags "-w -s" -o mytool main.go
mytool: Mach-O 64-bit executable arm64

```