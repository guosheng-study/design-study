#代理模式

###代理模式定义
代理模式是为一个对象提供一个代用品或占位符，以便控制对它的访问。

###虚拟代理
把一些开销很大的对象，延迟到真正需要它的时候才去创建。         

###用虚拟代理实现图片懒加载
1.不用代理的情况，直接给图片设置src
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
