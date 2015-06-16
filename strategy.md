#策略模式

###策略模式定义
定义一系列的算法，把它们一个个封装起来，并且使它们可以相互替换。
比如：去北京旅游，可以选择坐飞，坐火车，坐汽车或是骑车。

###计算奖金
绩效为S的人，年奖奖有4倍工资     
绩效为A的人，年奖奖有3倍工资
绩效为B的人，年奖奖有2倍工资

最初代码实现
```js
var calculateBonus = function (performanceLevel, salary) {
    if (performanceLevel === 'S') {
        return salary * 4;
    }
    if (performanceLevel === 'A') {
        return salary * 3;
    }
    if (performanceLevel === 'B') {
        return salary * 2;
    }
};

calculateBonus('B', 6000); //12000
calculateBonus('S', 8000); //32000
```

需求已经实现，但是以有下缺点：
1.calculateBonus包含了多个if
2.calculateBonus如果要增加C,D评级，就得改函数体内代码
3.算法复用性差。

