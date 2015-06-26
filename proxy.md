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
