
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


###订阅只感兴趣的信息
```js
var salesOffices = {
    clientList: [], //缓存列表
    listen: function (key, fn) {
        this.clientList[key] = this.clientList[key] || [];
        this.clientList[key].push(fn);
    },
    trigger: function () {
        var key = Array.prototype.shift.call(arguments),
            fns = this.clientList[key];

        if (fns) {
            for (var i = 0, fn; fn = fns[i++];) {
                fn.apply(this, arguments);
            }
        }
    }
};

//订阅88平米的信息
salesOffices.listen('squareMeter88', function (priec) {
    console.log('squareMeter88：' + priec);
});
//订阅110平米的信息
salesOffices.listen('squareMeter110', function (priec) {
    console.log('squareMeter110：' + priec);
});

//发布
salesOffices.trigger('squareMeter88', 20000);
salesOffices.trigger('squareMeter110', 30000);
```

###发布订阅查式的通用实现
```js
function Observer() {
    this.clientList = [];
}
Observer.prototype = {
    listen: function (key, fn) {
        this.clientList[key] = this.clientList[key] || [];
        this.clientList[key].push(fn);
    },
    trigger: function () {
        var key = Array.prototype.shift.call(arguments),
            fns = this.clientList[key];

        if (fns) {
            for (var i = 0, fn; fn = fns[i++];) {
                fn.apply(this, arguments);
            }
        }
    }
};

salesOffices = new Observer();


//订阅88平米的信息
salesOffices.listen('squareMeter88', function (priec) {
    console.log('squareMeter88：' + priec);
});
//订阅110平米的信息
salesOffices.listen('squareMeter110', function (priec) {
    console.log('squareMeter110：' + priec);
});

//发布
salesOffices.trigger('squareMeter88', 20000);
salesOffices.trigger('squareMeter110', 30000);
```


