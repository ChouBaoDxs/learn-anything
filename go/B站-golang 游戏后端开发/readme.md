---
title: golang 游戏后端开发
date: 2022-08-29 00:00:00

tags: 
- Go

categories: 
- Go
---

:::tip
视频名称：golang 游戏后端开发  
视频地址：https://www.bilibili.com/video/BV1KB4y1i7yQ  
作者：[拉托mu](https://space.bilibili.com/93811554)  
讲师 github：https://github.com/phuhao00?tab=repositories  
课程 github：
- 练习：https://github.com/phuhao00/practical  
- 游戏框架：https://github.com/phuhao00/greatestworks  

我的评价：★★★☆☆  
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

## 第三讲 go 中的 interface 11:17
```go
package main

import "fmt"

type Animal interface {
	Eat(interface{})
	Name() string
}

type AnimalCategory int

const (
	AnimalCategory1 AnimalCategory = iota + 1
	AnimalCategory2
)

type Base struct {
	Nick string `json:"nick"`
	Age  int    `json:"age"`
}

func (b *Base) Eat(i interface{}) {
	fmt.Println(" not clear ")
}

func (b *Base) Name() string {
	return ""
}

type Undefined struct {
	Base
}

type Pig struct {
	Nick string `json:"nick"`
	Age  int    `json:"age"`
}

func (p *Pig) Eat(i interface{}) {
	fmt.Println("cookie")
}

func (p *Pig) Name() string {
	return "peiqi"
}

type Dog struct {
	Nick string `json:"nick"`
	Age  int    `json:"age"`
}

func (d *Dog) Eat(i interface{}) {
	fmt.Printf("nick is:%v \n", d.Nick)
}

func (d *Dog) Name() string {
	return d.Nick
}

func main() {
    p := &Dog{
		Nick: "tiaotiao",
		Age: 0,
	}
	fmt.Println(p)
	fmt.Println(p.Name())

	animal1 := getAnimal(AnimalCategory1)
	animal2 := getAnimal(AnimalCategory2)
	animal3 := getAnimal(AnimalCategory(7))
	UseAnimal(animal1)
	UseAnimal(animal2)
	UseAnimal(animal3)
}

func getAnimal(category AnimalCategory) Animal {
	switch category {
	case AnimalCategory1:
		return &Dog{
			Nick: "tiaotiao",
			Age:  0,
		}
	case AnimalCategory2:
		return &Pig{
			Nick: "tiaotiao",
			Age:  0,
		}
	default:
	}
	return &Undefined{Base{
		Nick: "Base",
		Age:  0,
	}}

}

func UseAnimal[T Animal](a T) {
	a.Eat("suibian")
	a.Name()
}
```

## 第四讲 go 中的 channel 19:24
分类：
- 有缓冲、无缓冲
- 只读、只写、可读可写

```go
package main

import (
	"fmt"
	"sync"
)

var w2 sync.WaitGroup

func main() {
	w2.Add(2)
	ChDemo()
	ChDemo2()
	w2.Wait()
	demo3()
}

func demo3() {
	var c1 = make(chan int)
	var c2 = make(chan int)
	//var c1 = make(chan int, 1)
	//var c2 = make(chan int, 1)
	go demo3SUb(c1, c2)
	c1 <- 1
	fmt.Println(<-c2)
	close(c1)
	close(c2)
}

//只写 channel
func demo3SUb(in <-chan int, out chan<- int) {
	for v := range in {
		out <- v + 1
	}
}

var ch2 = make(chan int)    //无缓冲
var ch3 = make(chan int, 5) //有缓冲
var w sync.WaitGroup

func ChDemo() {
	w.Add(1)
	c := ChDemoSub()
	go func() {
		w.Wait()
		fmt.Println(<-c)
		close(c)
		w2.Done()
	}()
}

func ChDemoSub() chan int {
	var ch1 chan int
	ch1 = make(chan int, 2) //有缓冲
	ch1 <- 9
	w.Done()
	return ch1
}

func ChDemo2() {
	ch3 <- 7
	go ChDemo2SUb()
}

func ChDemo2SUb() {
	val := <-ch3
	fmt.Println(val)
	close(ch3)
	w2.Done()
}
```


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