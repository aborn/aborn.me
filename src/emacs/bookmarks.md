# Emacs的书签功能
emacs的书签用于记录你在文件中的阅读位置。它有点类似寄存器，跟寄存器一样，因为它也能记录位置位置。
但同寄存器有两点不一样：1. 它有比较长的名字; 2. 当emacs关闭的时候，它会自动持久化到
磁盘。

## 设置一个书签

当我们阅读一个很长的文档，没能一口气读完时。我们希望记住当前文档的最后阅读的位置，以便下次再用emacs
阅读的时候能快速地定位到。那么，我们设置一个书签，通过**bookmark-set** 对应快捷键为 **C-x r m**

## 列出保存的书签

**bookmark-bmenu-list** 对应快捷键为 **C-x r l** ，它将打开一个\*Bookmark List\*的buffer同时
列出所有保存的书签。

### 书签列表\*Bookmark List\*

在\*Bookmark List\*这个buffer里，有以下快捷键可以使用：

-   a 显示当前书签的标注信息;
-   A 在另一个buffer中显示所有书签的所有标注信息;
-   d 标记书签，以便用来删除 (x – 执行删除);
-   e 编辑当前书签的标注信息;
-   m 标记书签，以便用于进一步显示和其他操作 (v – 访问这个书签);
-   o 选中当前书签，并显示在另一个window中;
-   C-o 在另一个window中切换到当前这个书签;
-   r 重命名当前书签;
-   w 将当前书签的位置显示在minibuffer里。

## 跳转到一个书签

使用 **bookmark-jump** 函数，可以跳转到一个特定的书签，它绑定的快捷键为 **C-x r b** 。
如果你的emacs中安装了[helm](https://github.com/emacs-helm/helm) 这个插件，你也可以使用 **helm-bookmarks** 这个命令
来快速查找书签，并跳转到书签位置。

### helm-bookmarks

通过helm-bookmarks命令来查找并跳转书签如下图：

![bookmark查找与跳转](./images/bookmarks.webp)

### 修改默认排序

书签查找和跳转的时候，默认的书签排序是按字母排序的。如果想将最近访问的书签放在最前面，
将下面代码添加到你的emacs配置文件中。

```elisp
    (defadvice bookmark-jump (after bookmark-jump activate)
      (let ((latest (bookmark-get-bookmark bookmark)))
        (setq bookmark-alist (delq latest bookmark-alist))
        (add-to-list 'bookmark-alist latest)))
```

## 删除一个书签

删除一个书签对应的命令为 **bookmark-delete** 。

## 保存书签

最新版本emacs（老版本的书签保存在 **~/.emacs.bmk** ），
在退出的时候会自动保存书签。如果想手动保存书签的话，可以采用
 **bookmark-save** 这个函数命令。默认的情况，emacs会将书签保存在 **bookmark-default-file**
变量对应的文件中。在我的机器中，对应的文件如下：

```elisp
    ELISP> bookmark-default-file
    "/Users/aborn/.emacs.d/.cache/bookmarks"
    ELISP>
```

## 其他设置

有一个变量 **bookmark-save-flag** 。如果这个变量的值为一个数值，它表示修改（或新增）
多少次书签后，emacs会自动保存书签到磁盘。当这个变量的值被设置为1时，每次对bookmark的改
动，emacs就会自动保存内容到磁盘相应位置（这样可以防止emacs突然crash时bookmark的丢失）。
如果这个值设置为nil，表示emacs不会主动保存bookmark，除非用户手动调用
**M-x bookmark-save** 。

## bookmark+

[bookmark+](https://www.emacswiki.org/emacs/bookmark+.el) 是对bookmark的一个扩展的包。它有更多的功能：

1.  原始的bookmark只能对文件位置记录，bookmark+对孤立的buffer(没有关联文件的buffer)也能保存书签;
2.  支持对书签进行打tag;
3.  对文档的某个区域保存为书签，而不仅仅是某个位置;
4.  记录了每个书签的访问次数，及最后一次的访问时间，可以基于它们排序;
5.  多个书签可以有相同的名字;
6.  可以对函数、变量等加书签。

更多功能请参考: <https://www.emacswiki.org/emacs/BookmarkPlus#Bookmark%2b>
