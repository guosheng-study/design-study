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

##实现单例模式
