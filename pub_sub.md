
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


###取消订阅
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
    },
    remove: function (key, fn) {
        var fns = this.clientList[key];

        //消息没有订阅，直接返回
        if (!fns) {
            return false;
        }


        if (!fn) {
            fns.length = 0;
        } else {
            for (var i = fns.length - 1; i >= 0; i--) {
                var _fn = fns[i];
                if (_fn = fn) {
                    fns.splice(1, 1);
                }
            };
        }

    }
};

salesOffices = new Observer();


//订阅88平米的信息
salesOffices.listen('squareMeter88', function (priec) {
    console.log('squareMeter88：' + priec);
});

//取消订阅
salesOffices.remove('squareMeter88');


//订阅110平米的信息
salesOffices.listen('squareMeter110', function (priec) {
    console.log('squareMeter110：' + priec);
});

//发布
salesOffices.trigger('squareMeter88', 20000);
salesOffices.trigger('squareMeter110', 30000);
```

###普通登陆后刷新
```js
var login = {
    succ: function (fn) {
        setTimeout(function () {
            var res = {
                id: 1,
                name: 'xiaoming'
            };

            fn.call(this, res);

        }, 10);
    }
}


var header = (function () {
    return {
        refresh: function (data) {
            console.log('header', data);
        }
    }
}());

var cart = (function () {
    return {
        refresh: function (data) {
            console.log('cart', data);
        }
    }
}());
var nav = (function () {
    return {
        refresh: function (data) {
            console.log('nave', data);
        }
    }
}());


login.succ(function (data) {
    header.refresh(data);
    cart.refresh(data);
    nav.refresh(data);
});
```


###用订阅发布者模式实现
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
    },
    remove: function (key, fn) {
        var fns = this.clientList[key];

        //消息没有订阅，直接返回
        if (!fns) {
            return false;
        }


        if (!fn) {
            fns.length = 0;
        } else {
            for (var i = fns.length - 1; i >= 0; i--) {
                var _fn = fns[i];
                if (_fn = fn) {
                    fns.splice(1, 1);
                }
            };
        }

    }
};


var login = new Observer();

login.listen('loginSucc', function (res) {
    console.log('header', res);
});

login.listen('loginSucc', function (res) {
    console.log('cart', res);
});

login.listen('loginSucc', function (res) {
    console.log('nav', res);
});

setTimeout(function () {
    var res = {
        id: 1,
        name: 'xiaoming'
    };
    login.trigger('loginSucc', res);
}, 10);
```

