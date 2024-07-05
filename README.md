---
title: go库之Cobra
tags: golang
categories: golang
comments: true
---

# go库之Cobra

## 简介

Cobra是一个用来创建CLI应用程序的库，用于许多的Go项目，例如K8s、Hugo、和GitHub CLI等。

<!--more-->



Cobra建立在命令、参数和标志的结构之上，需要遵循的模式是

`APPNAME VERB NOUN --ADJECTIVE` 例如：`git clone URL --bare`

或者

`APPNAME COMMAND ARG --FLAG`  例如: `hugo server --port=1313`

## Command

命令

## Flag

标志是用来修改命令行为的一种方法

## 安装

使用go get 获取最新版本的库

`go get -u github.com/spf13/cobra@latest`

在程序中导入

`import "github.com/spf13/cobra"`

## 使用

### 项目基本结构

```go
  ▾ appName/
    ▾ cmd/
        root.go
      main.go
```



## 结构体

### Command

```go
type Command struct{
  // 具体的指令
  Use string 
  
  // 注释简短介绍
  Short string 
  //预期的参数 type PositionalArgs func(cmd *Command, args []string) error
  Args PositionalArgs
  // 长注释，通过-h --help来展示
  Long string 
  // 对命令的使用做一个简单的示例，在-h的时候会展示
  Example string
  // PreRun方法之前执行，并且子命令一样会执行
	PersistentPreRun func(cmd *Command, args []string)
 // PreRun方法之前执行，并且子命令一样会执行，返回一个error，如果该方法重写那么PersistentPreRun不会执行
	PersistentPreRunE func(cmd *Command, args []string) error
  // Run函数执行前执行
  PreRun func(cmd *Cmmand,args []string)  
  // Run函数执行前执行,返回一个error,如果重写了这个方法，PreRun不会执行
  PreRunE func(cmd *Command, args []string) error
  // 命令真正执行的函数，大多数命令都只会实现这个方法
  Run func(cmd *Command,args []string)
  // 命令执行的函数，但是会返回一个error，如果实现了这个方法那么Run方法不会再继续执行
  RunE func(cmd *Command,args []string)error
  // 该函数执行发生在 Run的方法之后
  PostRun func(cmd *Command,args []string)
  // 该函数执行发生在 Run的方法之后，返回一个error,如果重写了这个方法那么PostRun方法将不会执行
  PostRunE func(cmd *Command,args []string)error
  // 该函数执行发生在PostRun之后，并且子命令也会执行
  PersistentPostRun(cmd *Command,args []string)
	// 该函数执行发生在PostRun之后， 并且子命令也会执行，返回一个error，如果重写了这个方法，那么PersistentPostRun不会再执行
  PersistentPostRunE func(cmd *Command, args []string) error
  // 开启建议
  SuggestionsMinimumDistance int
  // 开启建议命令
  SuggestFor []string
}
```



<img src="https://img.ethanleo.top/uPic/image-20221201111016933.png" alt="image-20221201111016933" style="zoom:33%;" />

### 方法

#### func (c *Command) Flags() *flag.FlagSet

返回只给当前命令的flagSet

#### func (c *Command) PersistentFlags() *flag.FlagSet

返回一个flagSet,可以提供给当前命令和子命令使用

#### func (c *Command) MarkFlagRequired(name string) error

设置某个Flag为必填

#### func (f *FlagSet) StringVarP(p *string, name, shorthand string, value string, usage string)

绑定一个string类型的flag

* name: flag 名称
* shorthand:  flag简称
* value: flag 默认值
* usage：flag的使用说明





## 示例

https://gitee.com/Pea997/eagle

