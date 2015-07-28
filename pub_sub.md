
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
