
#发布-订阅模式

###定义
发布-订阅模式又叫观察者模式，一对多的关系。当一个对象发生改变，所以依赖它的对象都得到通知。


###Dom事件
```js
document.body.addEventListener('click', function () {
    alert(2);
}, false);

document.body.click();
```

###增加订阅
```js
document.body.addEventListener('click', function () {
    alert(2);
}, false);

document.body.addEventListener('click', function () {
    alert(3);
}, false);

document.body.addEventListener('click', function () {
    alert(4);
}, false);

document.body.click();
```

###先订阅后发布
```js
var salesOffices = {
    clientList: [], //缓存列表
    listen: function (fn) {
        this.clientList.push(fn);
    },
    trigger: function () {
        for (var i = 0, fn; fn = this.clientList[i++];) {
            fn.apply(this, arguments);
        }
    }
};

//订阅
salesOffices.listen(function (priec, squareMeter) {
    console.log(priec, squareMeter);
});
salesOffices.listen(function (priec, squareMeter) {
    console.log(priec, squareMeter);
});

//发布
salesOffices.trigger(20000, 88);
salesOffices.trigger(30000, 100);
```
