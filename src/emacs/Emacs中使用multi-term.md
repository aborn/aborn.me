# Emacs下使用multi-term

Emacs里的[multi-term](http://www.emacswiki.org/emacs/MultiTerm)相当于mac下的iterm，是emacs下非常好用的terminal。

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
2. 如果你使用的是mac系统，发现multi-term每行出出了4m，在shell里运行下：
```
tic -o ~/.terminfo /Applications/Emacs.app/Contents/Resources/etc/e/eterm-color.ti
```
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

## 解决几个烦人的问题

**1.** 默认目录
我开始设置的是zsh，我发现，当我用$cd$命令改变工作目录的时候，emacs里的default-directory这个变量没有改变，使得C-x C-f调用打开文件时目录不是当前工作目录？
[解决方法](http://stackoverflow.com/questions/367442/getting-emacs-ansi-term-and-zsh-to-play-nicely)将下列代码放到zsh的配置文件 $~.zshrc$里，使得emacs能跟踪路径的改变，[参考1](https://snarfed.org/why_i_run_shells_inside_emacs)，[参考2](http://emacs.stackexchange.com/questions/5589/automatically-update-default-directory-when-pwd-changes-in-shell-mode-and-term-m)。 [参考3](https://stackoverflow.com/questions/3508387/how-can-i-have-term-el-ansi-term-track-directories-if-using-anyhting-other-tha)
```shell
if [ -n "$INSIDE_EMACS" ]; then
    chpwd() { print -P "\033AnSiTc %d" }
    print -P "\033AnSiTu %n"
    print -P "\033AnSiTc %d"
fi
```

**2.** 光标位置处理 我希望当光标未处于最后一行时，"C-a"的作用是将光标移动到行首，当光标处于最后一行时，我希望"C-a"的作用是将光标移动到这行命令的开始处。解决方法：将下列 ab/move-beginning-of-line 绑定到快捷键"C-a"即可。

```elisp
;; 当处于最后一行时 "C-a" 将光标移动到 terminal开始处而不是这个行的头
(defun ab/is-at-end-line ()
   "判断是否在最后一行"
   (equal (line-number-at-pos) (count-lines (point-min)  (point-max))))
(defun ab/is-term-mode ()
   "判断是否为 term 模式"
    (string= major-mode "term-mode"))
(defun ab/move-beginning-of-line ()
   "move begin"
   (interactive)
   (if (not (ab/is-term-mode))
              (beginning-of-line)
         (if (not (ab/is-at-end-line))
                    (beginning-of-line)
                (term-send-raw))))
```

**3.** 有个烦人的问题，你发现使用了"C-b" (backword-char 函数)，你想在命令的中间插入新的字符，每次都插入到了这行的最后。解决方法：将下列ab/backword-char函数绑定到"C-b"
```elisp
;; 只后当是term-mode并且是最后一行时才采用 (term-send-left)
(defun ab/backword-char ()
   "Custom "
    (interactive)
    (if (not (ab/is-term-mode))
               (backward-char)
          (progn (if (not (ab/is-at-end-line))
                          (backward-char)
                        (progn (term-send-left)
                                   (message "term-send-left"))))))
```

**4.**修改快捷键的map,如果你发你定义自己的快捷键与该major-mode的冲突，可以直接修改它的key-map
```elisp
(define-key term-raw-map (kbd "M-n") 'ace-jump-mode)
```
更多见，[我的multi-term配置](https://github.com/aborn/emacs.d/blob/master/utils/multi-term-config.el)
