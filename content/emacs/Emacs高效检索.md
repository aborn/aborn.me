有这样的场景，其中之一为我们想快速查找到想要的文件；其二为我们能根据关键字快速找到相关文档：

## 纯文本检索
https://tech.marksblogg.com/meilisearch-full-text-search.html
https://github.com/meilisearch/meilisearch 做纯文本检索

## ripgrep 按关键字检索
下面这个作为命令行
https://github.com/BurntSushi/ripgrep  本地搜索
https://gist.github.com/pesterhazy/fabd629fbb89a6cd3d3b92246ff29779  如何在emacs里进行设置
rg -i "bixin”  命令行里关键字搜索

mac下需要先安装：

```shell
brew install ripgrep
```


## fd-find 按文件名进行搜索
https://lib.rs/crates/fd-find

mac下需要先安装fd：

```shell
brew install fd
```

命令行下：
fd "优惠券"
