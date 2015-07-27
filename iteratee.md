
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




