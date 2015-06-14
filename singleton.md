##单例模式的定义
保证一个类仅有一个实例，并提供一个访问它的全局访问点。

##实现单例模式
用一个变量标记当前是否已经为某个类创建过对象，如果是，则在下一次获取类的实例时，直接返回之前创建的对象。
```js
var Singleton = function (name) {
    this.name = name;
    this.instance = null; //标记是否已经实例化
};

Singleton.prototype.getName = function () {
    console.log(this.name);
};

Singleton.getInstance = function (name) {
    if (!this.instance) {
        //没有实例，实例化一下
        this.instance = new Singleton(name);
    }
    return this.instance; //返回实例
}

var a = Singleton.getInstance('sven1');
var b = Singleton.getInstance('sven2');

alert(a === b); //true
```
或者：
```js
var Singleton = function (name) {
    this.name = name;
};

Singleton.prototype.getName = function () {
    console.log(this.name);
};

Singleton.getInstance = (function () {
    var instance = null; //标记是否已经实例化
    return function (name) {
        if (!instance) {
            instance = new Singleton(name);
        }
        return instance;
    }
}());

var a = Singleton.getInstance('sven1');
var b = Singleton.getInstance('sven2');

alert(a === b); //true
```
这两种方法相对简单，但是有一个问题，增加了这个类的不透明性，Singleton类的使用者必须知道这个单例类。

##透明的单例模式
```js
var createDiv = (function () {
    var instance;

    var CreateDiv = function (html) {

        if (instance) {
            return instance;
        }

        this.html = html;
        this.init();

        return instance = this;
    };

    CreateDiv.prototype.init = function () {
        var div = document.createElement('div');
        div.innerHTML = this.html;
        document.body.appendChild(div);
    };

    return CreateDiv;
}());

var a = new createDiv('hello');
var b = new createDiv('kit');
```
虽然这样子完成了一个透明的单例类，但它同样有一些缺点。       
1.程序复杂，使用自执行的匿名函数和闭包，让这个匿名函数返回真正的单例类。      
2.实例单例的构造函数，负责了两件事：第一保证只有一个实例，创建对象和执行init()方法。    
