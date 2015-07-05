# emacs 使用multi-term

emacs里的[multi-term](http://www.emacswiki.org/emacs/MultiTerm)相当于mac下的iterm，是emacs下非常好用的terminal。

## 载入multi-term.el文件
emacs使用multi-term作为terminal，首先要将[multi-term.el](http://www.emacswiki.org/emacs/download/multi-term.el)文件放到你emacs的load-path里。

## 配置
```
;; ------------------------------------------------------------
;; set multi-term
;; ------------------------------------------------------------
(require 'multi-term)
(setq multi-term-program "/bin/zsh")
;; Use Emacs terminfo, not system terminfo, mac系统出现了4m
(setq system-uses-terminfo nil)

```
下面是几点需要注意的点：
1. 我用的是zsh，如果你使用的是bash， 将"/bin/zsh"换成你的"/bin/bash"
2. 如果你使用的是mac系统，发现multi-term每行出出了4m，在shell里运行下：tic -o ~/.terminfo /Applications/Emacs.app/Contents/Resources/etc/e/eterm-color.ti
3. zsh在mac下可能会出现中文显示为????的情况，这时候创建一个文件：~/.zshenv,其内容如下：
```
export LANG='en_US.UTF-8'
export LC_ALL="en_US.UTF-8"
```

## 快捷键
打开multi-term的命令是**multi-term**，你可能发现在multi-term模式下会出现与自己的快捷键冲突的地方。如果想保留自己在其他mode下的快捷键，将快捷键添加到 *term-bind-key-alist*这个列表中，例如我想把"C-j"保留我其他mode一样，如下：
```
(add-to-list 'term-bind-key-alist '("C-j"))
```
