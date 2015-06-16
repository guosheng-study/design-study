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

###使用策略模式重构代码
策略模式的程序由至少有两部分组成，策略组封装了具体的算法，环境类接受客户的请求，随后请求委托给某一个策略类。

```js
//定义策略组
var performanceS = function () {};
performanceS.prototype.calculate = function (salary) {
    return salary * 4;
};

var performanceA = function () {};
performanceA.prototype.calculate = function (salary) {
    return salary * 3;
};

var performanceB = function () {};
performanceB.prototype.calculate = function (salary) {
    return salary * 2;
};

//定义奖金类
var Bonus = function () {
    this.salary = null; //基本工资
    this.strategy = null; //绩效等级对应的策略对象
};

Bonus.prototype.setSalary = function (salary) { //设置基本工资
    this.salary = salary;
};

Bonus.prototype.setStrategy = function (strategy) { //设置员工绩效等级对应的策略对象
    this.strategy = strategy;
};

Bonus.prototype.getBonus = function () { //获取奖金数
    //将计算奖金的操作委托组策略类。
    return this.strategy.calculate(this.salary);
};

//计算奖金
var bonus = new Bonus();

bonus.setSalary(6000);
bonus.setStrategy(new performanceB());
bonus.getBonus(); //12000

bonus.setSalary(8000);
bonus.setStrategy(new performanceS());
bonus.getBonus(); //3200

//如果增加C等级的绩效就很简单了
var performanceC = function () {};
performanceC.prototype.calculate = function (salary) {
    return salary * 1.5;
};
```
上面的代码是对传统对象语言的模仿，接下去看一下javascript版的策略模式。

###javascript版的策略模式
```js
//定义策略组
var setStrategies = {
    S: function (salary) {
        return salary * 4;
    },
    A: function (salary) {
        return salary * 3;
    },
    B: function (salary) {
        return salary * 2;
    }
};

//定义奖金类
var calculateBonus = function (level, salary) {
    return setStrategies[level](salary);
};

calculateBonus('B', 6000); //12000
calculateBonus('S', 8000); //32000
```

