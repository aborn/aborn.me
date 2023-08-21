# mithril学习
[Mithril](http://mithril.js.org/)是一种客户端MVC框架。
它有以下三个特点：
1. 轻量级，代码只有7.8kB,上手快
2. 鲁棒性, 安全模板，结构化的MVC组件
3. 快，虚拟dom,智能的重页面渲染系统

## 一个例子
```javascript
//model
var Page = {
    list: function() {
        return m.request({method: "GET", url: "pages.json"});
    }
};

var Demo = {
    //controller
    controller: function() {
        var pages = Page.list();
        return {
            pages: pages,
            rotate: function() {
                pages().push(pages().shift());
            }
        }
    },

    //view
    view: function(ctrl) {
        return m("div", [
            ctrl.pages().map(function(page) {
                return m("a", {href: page.url}, page.title);
            }),
            m("button", {onclick: ctrl.rotate}, "Rotate links")
        ]);
    }
};
//initialize
m.mount(document.getElementById("example"), Demo);
```
