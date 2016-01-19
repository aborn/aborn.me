# eeb博客平台介绍
eeb是elixir语言版本的一种静态博客生成器。对就的github项目为[https://github.com/aborn/eeb](https://github.com/aborn/eeb)。

## 安装
使用以下步骤
1. 安装elixir语言环境
2. git clone https://github.com/aborn/eeb.git
3. cd eeb
4. mix deps.get 
5. 写博客
6. mix eeb.blog

安装完elixir语言环境后，一键安装脚本:
```shell
git clone https://github.com/aborn/eeb.git
cd eeb
mix deps.get
mix eeb   # eeb相关命令介绍
```

## 部署
利用screen作为后台daemon
```
screen mix run --no-halt     #C-a d
# screen -ls
# screen -r id
```
