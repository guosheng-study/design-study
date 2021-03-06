
#迭代器模式


###定义
迭代器模式是指提供一种方法，顺序访问一个集合中的各个对象，需又不需要暴露对象内部的表示。    
可以把迭代的过程从业务中分离出来。

###jQuery里的迭代器
```js
$.each([11, 22, 23], function (index, n) {
    console.log(index, n); // index为下标，n为值
});
```

###实现自己的迭代器
```js
var each = function (ary, callback) {
    for (var i = 0, l = ary.length; i < l; i++) {
        callback.call(ary, i, ary[i]);
    }
};

each([11, 22, 33], function (i, n) {
    console.log(i, n);
});
```

###判断两个数组是否一样
```js
function compare(arr1, arr2) {
    var flag = arr1.length === arr2.length ? true : false;

    if (flag) {
        each(arr1, function (index, val) {
            if (val !== arr2[index]) {
                flag = false;
            }
        });
    }

    return flag;
}

```


###外部迭代器
外部迭代器必须显式的请求迭代下一个元素
```js
var iterator = function (obj) {
    var current = 0;
    return {
        next: function () {
            current += 1;
        },
        isDone: function () {
            return current >= obj.length;
        },
        getCurrItem = function () {
            return obj[current];
        }
    };
};
```

###外部迭代器判断两个数组是否一样
```js
function compare(arr1, arr2) {
    var iterator1 = iterator(arr1),
        iterator2 = iterator(arr2),
        flag = true;

    while (!iterator1.isDone() && !iterator2.isDone()) {
        if (iterator1.getCurrItem() !== iterator2.getCurrItem()) {
            flag = false;
        }
        iterator1.next();
        iterator2.next();
    }
    return flag;
}
```

###迭代类数组对象和字面象对象
```js
var each = function (obj, callback) {
    var value,
        i = 0,
        length = obj.length;

    if (length) {
        for (; i < length; i++) {
            if (callback.call(obj, i, obj[i]) === !1) {
                break;
            }
        }
    } else {
        for (i in obj) {
            if (callback.call(obj, i, obj[i]) === !1) {
                break;
            }
        }
    }
};

each(['a', 'b', 'c', 'd'], function (index, value) {
    console.log(index, value);
});

each({
        a: '1',
        b: '2',
        c: '3'
    },
    function (index, value) {
        console.log(index, value);
    });

(function () {
    each(arguments, function (index, value) {
        console.log(index, value);
    });
}(1, 2, 3, 4, '5'));
```

###倒序迭代器
```js
var reverseEach = function (arr, cb) {
    for (var l = arr.length - 1; l >= 0; l--) {
        cb.call(arr, l, arr[l]);
    }
};

reverseEach(['a', 'b', 'c', 'd'], function (index, val) {
    console.log(index, val);
});
```

###终止迭代器
```js
var each = function (ary, callback) {
    for (var i = 0, l = ary.length; i < l; i++) {
        if (callback.call(ary, i, ary[i]) === false) {
            break;
        }
    }
};

each([1, 2, 3, 4, 5, 6, 7], function (index, n) {
    if (n > 3) {
        return false;
    }
    console.log(n);
});
```







