# 一些常用的经常忘记的命令

## 文件查找
find-file-in-project-by-selected
emacs配置见如下，需要设置变量 ffip-use-rust-fd 为t (C-v ffip-use-rust-fd)
https://github.com/sharkdp/fd
mac下需要先安装fd
brew install fd

## 通过ripgrep进行文本内容搜索
spacemacs/helm-project-do-ag M-m s a p

## 打开所以之前打开过的文件
aborn/open-all-recent-files

## 打开当前所在的url连接
spacemacs/avy-open-url M-m j U

## 最近打开过的文件列表
recentf-open-files

## magit相关
```magit-diff-unstaged```表示全项目的diff  
```magit-diff``` 默认表示当前文件的diff

---
magit-commit的命令失效，需要magit-commit-create替换

统计git的贡献情况：
https://stackoverflow.com/questions/42715785/how-do-i-show-statistics-for-authors-contributions-in-git
添加到path里
export PATH=$PATH:/usr/local/go/bin:/Users/aborn/github/CodeSnippet/git
在Mac下还需要安装(awk替换成gawk)
brew install gawk
命令如下：
git-user-stats.sh --since="1 week ago"
https://stackoverflow.com/questions/1828874/generating-statistics-from-git-repository?rq=1

打开所有历史文件
https://emacs.stackexchange.com/questions/52127/how-to-open-all-the-opened-files-from-a-previous-session
