# Git的使用

出现如下问题，中文的文件名显示异常，如下图：
![效果图](git.png)

解决如下，[参考](https://stackoverflow.com/questions/34549040/git-not-displaying-unicode-file-names)

```shell
git config --global core.quotePath false
```
