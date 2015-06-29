#代理模式

###代理模式定义
代理模式是为一个对象提供一个代用品或占位符，以便控制对它的访问。

###虚拟代理
把一些开销很大的对象，延迟到真正需要它的时候才去创建。         

###用虚拟代理实现图片懒加载
1.不用代理的情况，直接给图片设置src，代理如下，如果图片很大，那么会有一段空白的时间。
```js
var myImage = (function () {
    var imgNode = document.createElement('img');
    document.body.appendChild(imgNode);

    return {
        setSrc: function (src) {
            imgNode.src = src;
        }
    }
}());

myImage.setSrc('https://www.baidu.com/img/bdlogo.png');
```

2.引入代理
```js
var myImage = (function () {
    var imgNode = document.createElement('img');
    document.body.appendChild(imgNode);

    return {
        setSrc: function (src) {
            imgNode.src = src;
        }
    };
}());

//增加了代理
var proxyImage = (function () {

    var img = new Image();

    img.onload = function () {
        myImage.setSrc(this.src);
    }

    return {
        setSrc: function (src) {
            myImage.setSrc('http://img.lanrentuku.com/img/allimg/1212/5-121204193R0.gif');
            img.src = src;
        }
    };
}());

//使用代理加载图片
proxyImage.setSrc('https://www.baidu.com/img/bdlogo.png');
```

3.代理和本体的接口要保持一致性。    
用户可以放心的使用代理，他只关心是否能得到想要的结果；    
在任何用本体的地方都可以替换成使用代理。   

###代理的意义
一个函数仅一个引起它变化的原因，myImage只负责把本体加上src,而proxyImage只负责loading图，完了之后交给本体。
注：单一职责原则。

###虚拟代理合并请求
选中一个checkbox的时候向服务器发送一个请求，我们可以用代理来减少请求。
```html
<label><input type="checkbox" value="1">1</label>
<label><input type="checkbox" value="2">2</label>
<label><input type="checkbox" value="3">3</label>
<label><input type="checkbox" value="4">4</label>
<label><input type="checkbox" value="5">5</label>
<label><input type="checkbox" value="6">6</label>
<label><input type="checkbox" value="7">7</label>
<label><input type="checkbox" value="8">8</label>
<label><input type="checkbox" value="9">9</label>
<label><input type="checkbox" value="10">10</label>
```

```js
//本体
var syncFiles = function (id) {
    console.log('开始同步文件：' + id);
};

//代理
var proxySyncFiles = (function () {
    var cache = [], //存ids的集合
        timer; //定时器

    return function (id) {
        cache.push(id);
        if (timer) {
            return
        }
        timer = setTimeout(function () {
            syncFiles(cache.join(','));
            timer = null;
            cache.length = 0;
        }, 2000)
    }
}());

//添加事件
var checkbox = document.getElementsByTagName('input');
for (var i = 0, c; c = checkbox[i++];) {
    c.onclick = function () {
        if (this.checked === true) {
            //syncFiles(this.value);
            proxySyncFiles(this.value);
        }
    }
};
```

###缓存代理
缓存代理可以为一些开销大的运算结果提供暂时的存储，在下次运算时，如果传递进来的参数跟之前一致，则可以直接返回前面存储的运算结果。

```js
//实现阶乘
var mult = function () {
    console.log('开始计算阶乘：' + Array.prototype.join.call(arguments, ','));
    var a = 1;
    for (var i = 0, l = arguments.length; i < l; i++) {
        a = a * arguments[i];
    }

    return a;
};

console.log(mult(2, 3));
console.log(mult(2, 3, 4));

//加入缓存代理
var protyMult = (function () {
    var cache = {};
    return function () {
        var args = Array.prototype.join.call(arguments, ',');
        if (args in cache) {
            return cache[args];
        }
        return cache[args] = mult.apply(this, arguments);
    }
}());

console.log(protyMult(2, 3)); //使用mult方法
console.log(protyMult(2, 3)); //调用缓存里的值
console.log(protyMult(2, 3)); //调用缓存里的值
```
