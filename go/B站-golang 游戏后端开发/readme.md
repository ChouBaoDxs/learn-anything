---
title: golang 游戏后端开发
date: 2022-08-29 00:00:00

tags: 
- Go

categories: 
- Go
---

:::warning
视频名称：golang 游戏后端开发  
视频地址：https://www.bilibili.com/video/BV1KB4y1i7yQ  
作者：[拉托mu](https://space.bilibili.com/93811554)  
讲师 github：https://github.com/phuhao00?tab=repositories  
课程 github：
- 练习：https://github.com/phuhao00/practical  
- 游戏框架：https://github.com/phuhao00/greatestworks  

我的评价：★★★★☆  
:::

<!-- more -->
[TOC]

## 第一讲 闲聊和 go 中的 slice 10:05
讲师已经做了快 5 年的游戏开发，现在在上海做 MMO，刚毕业是做 SLG，18 年开始工作。

包管理推荐 go mod，ide 选择 goland 或者 vscode。

go 中的切片 slice：
```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	runtime.GOMAXPROCS(3)
	SliceDemo()
}

func SliceDemo() {
	var s1 []int
	s2 := make([]int, 9)
	fmt.Println(s2)
	s1 = make([]int, 9, 9)
	print(s1)
	s1[0] = 0
	s1[1] = 1
	fmt.Println(s1[0:1])
	s1 = append(s1, 1) //
	println("---", s1[0])
	var m1 map[int]bool
	m1 = make(map[int]bool)
	m1[1] = true

	m1[5] = true
	m1[6] = true
	m1[2] = true
	m1[3] = true
	m1[4] = true

	for i, i2 := range s1 {
		fmt.Println(i, i2)
	}

	for i, b := range m1 {
		fmt.Println(i, b)
	}
}
```

## 第二讲 游戏后端开发逻辑 08:42
```go
package main

func main() {
	LoadConf()
	LoadDBData()
	StartGameServer()
	ResolveGameLogic()
	StopGameServer()
}

//StartGameServer  启动游戏服
func StartGameServer() {
	Log()

}

//LoadConf 加载配置
func LoadConf() {
	Log()

}

//LoadDBData 加载DB 数据
func LoadDBData() {
	Log()

}

//ResolveGameLogic 处理游戏逻辑
func ResolveGameLogic() {
	SaveGameData()
	Log()

}

//SaveGameData  存储游戏数据
func SaveGameData() {
	Log()

}

//StopGameServer 停止游戏服
func StopGameServer() {
	Log()

}

//Log 打印日志
func Log() {

}
```

## 第三讲 11:17
## 第四讲 19:24
## 第五讲 开始写游戏框架 18:59
## 第六讲 网络传输层逻辑 part 1 48:23
## 第七讲 网络传输层逻辑 part 2 16:05
## 第8讲 模拟客户端，proto 协议 玩法大致介绍一下自己知道的 part2 28:10
## 第8讲 模拟客户端，proto 协议 玩法大致介绍一下自己知道的 49:10
## 第9讲 模拟客户端 创角色，登录，添加，删除好友 ，发聊天消息 逻辑 part1 58:53
## 第10讲 sugar函数库，日志库spoor 引用介绍 40:39
## 第11讲 泛型使用例子讲解，以及mongo 开篇介绍下 56:09
## 第12讲 context 使用了解，golang 继承，框架组件抽象 28:01
## 第13讲 mongo 基础语法过一遍 40:39
## 第14讲 讲一下后续的讲解的大纲 18:55
## 第15讲 redis nsq 安装部署，简单熟悉下 29:42
## 第16讲 开启unity客户单学习 18:20
## 第17讲 timerassistant ,nsq封装 bug fix... 16:46