#命令模式
命令模式的应用场景：不知道请求接收者和发送者。

###命令模式例子
```html
<button id="button1">按钮一</button>
```

```js
var button1 = document.getElementById('button1');

//安装命令
var setCommand = function (button, command) {
    button.onclick = function () {
        command.execute();
    };
};

var menuBar = {
    refresh: function () {
        console.log('刷新菜单目录');
    },
    add : function(){
        console.log('增加');
    },
    del : function(){
        console.log('删除'); 
    }
};

var RefreshMenuBarCommand = function (receiver, funcName) {
    this.receiver = receiver;
    this.execute = function(){
        this.receiver[funcName]();
    };
};

var refreshMenuBarCommand1 = new RefreshMenuBarCommand(menuBar, 'refresh');
var refreshMenuBarCommand2 = new RefreshMenuBarCommand(menuBar, 'add');
var refreshMenuBarCommand3 = new RefreshMenuBarCommand(menuBar, 'del');

//安装命令
setCommand(button1, refreshMenuBarCommand2);
```

###javascript里的命令模式
```js
var button1 = document.getElementById('button1');

var bindClick = function (button, fnc) {
    button.onclick = fnc;
};

var menuBar = {
    refresh: function () {
        console.log('刷新菜单目录');
    },
    add : function(){
        console.log('增加');
    },
    del : function(){
        console.log('删除'); 
    }
};

bindClick(button1, menuBar.add);
```


