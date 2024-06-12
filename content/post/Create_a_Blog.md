+++
title = '使用Hugo创建博客'
date = 2024-01-06T16:47:48+08:00
draft = false
+++
#
## 1. 前提
电脑里有git环境

## 2. windows安装Hugo
[Hugo](https://gohugo.io/)有标准版和扩展版两个版本，推荐[下载](https://gohugo.io/installation)扩展版，区别在于扩展版提供了将Sass转译为CSS的 LibSass 转译器

###
#### Scoop安装Hugo扩展版
PowerShell先下载[scoop](https://scoop.sh/)，再执行以下命令安装hugo扩展版
>scoop install hugo-extended

hugo被下载到`./scoop/apps`里

下载好hugo后还需要配置环境变量Path，bin目录里面存放hugo.exe文件
>此电脑>>属性>>高级系统设置>>环境变量>>系统变量>>Path>>新建，加入D:\hugo\bin

## 3. 创建本地站点
>hugo new site myblog

从官网下载[主题](https://themes.gohugo.io/)，以本博客主题为例(myblog目录下执行)
>git clone https://github.com/vaga/hugo-theme-m10c.git themes/m10c

## 4. 本地启动个人博客

###
#### 4.1. 动态选择主题
>hugo server -t m10c --buildDrafts

#### 4.2. 主题放入配置文件config.toml
`themes/m10c/exampleSite`里的config.toml替换根目录下的config.toml，注意修改里面的`themesDir`路径，配置好后直接执行
>hugo server


## 5. 写一篇本地博客
`content/post`里新建一个md文件

或者在根目录下使用命令
>hugo new post/test.md

注意，博客文件必须放在content/post路径下

## 6. 个人博客部署到远端服务器
创建一个github仓库，命名为`用户名.io`

终端输入
>hugo --theme=m10c --baseURL="https://用户名.github.io/" --buildDrafts

会生成一个public文件

进入public目录，执行
```
git init
git add .
git commit -m "your comment"
git remote add origin sshURL
git push
```

## 7. 博客提交到远端
本地写好博客后在myblog目录下执行Hugo进行编译

进入public目录，执行
```
git add .
git commit -m "add comment"
git push
```
