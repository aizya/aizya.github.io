---
layout: post
title: 浅析JS中的方法
date: 2017-08-22
categories: JavaScript
tags: [JavaScript]
description: 简明扼要的理解JS中的方法
published: true
---

### 对象中的方法

什么是对象?对象就是用来表示现实世界的一些实体,如用户/订单等等..

**给一个对象绑定一个函数,而这个函数则被称之为方法.**

下面就是一个简单的对象定义:

    let handsome_man = {
        name: "xiong",
        age: 23
    }; // 分号可加可不加

在现实世界中,handsome_man(就是本人)具有一些相应的行为表现,如,唱歌啊,跳舞啊,裸睡啊...哈

这些具体的动作表现在JS中,皆通过函数方式进行表示(类似与properties属性).

先不管三七二十一,像我这么帅的男人,怎能没有腹肌,虽然只有一块...那就练出来!哼哈~~

    handsome_man.doSomeYunDong = function(){
        alert("哟哟,啪啪啪~~~");
    };

    handsome_man.doSomeYunDong(); // output: 哟哟,啪啪啪~~~

上面这段代码很简单,创建一个函数Function并将其赋值给handsome_man.doSomeYunDong,因为JS是门动态语言,因此,这样做是没有错的.

这样,我们的handsome_man就学会了啪啪啪,噢,不不不,是做运动...

当然,除了上面这种方式的定义之外,还可以预先定义好函数,然后进行赋值,如:

    let handsome_man = {
        // nothing
    };
    
    function doSomeYunDong(){
        alert("哟哟,啪啪啪~~~");
    }

    handsome_man.doSomeYunDong = doSomeYunDong; // 注意这里不要写成了doSomeYunDong(), 这表示方法调用..

    handsome_man.doSomeYunDong(); // 同上输出....

上面这段代码的不同之处在于,定义好了一个函数,然后将这个函数赋值给一个对象,你知道的:

    function doSomeYunDong(){
        alert("哟哟,啪啪啪~~~");
    }
    
    //等价于

    let doSomeYunDong = function(){
        alert("哟哟,啪啪啪~~~");
    }; // 因此可以直接使用handsome_man.doSomeYunDong = doSomeYunDong; 进行赋值

Tip:
    
    JS中,这种使用对象来描述现实世界实体的方式被称之为面向对象编程(Object-Oriented-Programming).简称为OOP.Java就是一个典型的OOP语言.

此外,Method字面量表示方式更为简单:

    let handsome_man = {
        doSomeYunDong : function(){
            alert("哟哟,啪啪啪~~~");
        }
    }

或者再看看下面这种方式,与上面那种方式等价:

    let handsome_man = {
        doSomeYunDong(){
            alert("哟哟,啪啪啪~~~");
        }
    }

经过论证,可以直接省略function,直接写doSomeYunDong(). 今后,你会发现,后一种方式最长使用.

### 在方法中this的使用

一般而言,对象的方法中需要普遍使用到存储在对象中的其他数据作一些其他的工作.就拿上一个例子而言,在"做运动"的时候,我需要传入谁在做运动,这时候,就需要这个handsome_man的name.

To access the object, a method can use the this keyword.

比如:

    let handsome_man = {
        name: "xiong",
        age:23,
        doSomeYunDong: function(){
            alert(this.name + "  正在哟哟,啪啪啪~~~");
        }
    }

    handsome_man.doSomeYunDong(); // output: xiong  正在哟哟,啪啪啪~~~

**注意:**在执行方法调用的时候,this可以被理解成当前对象,也就是handsome_man. 

所以,也可以这样进行定义:

    let handsome_man = {
        name: "xiong",
        age:23,
        doSomeYunDong: function(){
            alert(handsome_man.name + "  正在哟哟,啪啪啪~~~");
        }
    }

虽然输出的结果都是一样,但是这样做却不是很可靠,比如我们将这个handsome_man赋值给其他变量,如me,然后调用就会出错:

    me = handsome_man;
    handsome_man = null; //置空 
    me.doSomeYunDong(); // 将会出错,因为handsome_man为空,在调用它的name属性,就会出现异常.

### this是不受约束的

在JS中,this这个关键与其他很多编程语言中的this都不一样,首先,它可以被使用在任何函数中,比如下面这种操作,也没有语法错误:

    function handsome_man(){
        alert(this.name);
    }

this的值在运行时才会进行判定,因此它可能是任何值.

所以,即便是使用相同的函数,也可能得到其他不同的值.

    let user = {
        name: 'zhangyue'
    }

    let admin = {
        name: 'xiong'
    }

    function sayHi(){
        alert(this.name);
    }

    user.func = sayHi;
    admin.func = sayHi;

    user.func; // zhangyue
    admin.func; // xiong

    admin['func']; // xiong  这种调用方式与.的调用方式一致.

事实上,即便不使用任何对象,我们也可以调用一个函数:

    function sayHi(){
        alert(this);
    }

    sayHi(); // undefined

在严格模式下(use strict), 上面这个例子中的this为undefined,如果你要访问this.name,就会得到一个错误.

然而,在非严格模式下,换而言之,也就是你忘了使用use strict,那么这种情况下的this表示了全局对象window. 之所以使用严格模式,可以很好的解决这个历史遗留的行为.

**特别注意:** 在Function中抛开对象去使用this虽然并不常见,但是却不是一个错误.如果一个函数使用了this,那么就表示它需要调用上下文中的对象.

### 未绑定this的consequences(后果)

如果你曾经学习过其他的编程语言,你可能也使用过this绑定的一些想法,一个定义在对象中的方法,总是存在一个this去指向这个对象.

然而,在JS中,this是很随意的.它表示的值在运行时才会得到确认,它并不依赖于方法是在哪里进行定义的,它只关心.(dot,点号)之前的对象是什么.

这种在运行时evaluated(评估)this的方式pluses and minuses(得失参半),一方面,一个函数能够被多个对象所使用,强大的灵活性造成了更多错误的发生.

### 内部的: 引用类型

深入了解语言的一些特性: (下面会了解一些边缘情况)

关于函数中this的遗失问题:

    let user = {
        name: 'xiong',
        age: 23,
        sayHi(){
            alert(this.name);
        }
        bye(){
            alert('bye');
        }
    }
    
    user.sayHi(); // xiong
    
    (user.name=='xiong'? user.sayHi : user.bye)(); // 出现错误

注意看这段代码,最后一行使用了一个三元运算符,当用户名为xiong的时候,使用user.sayHi,否则,使用user.bye,然后,调用()园括号. 这种方式就会出错,因为此时的this为undefined.

如果直接在三元运算符中使用user.sayHi(),这就没有任何问题.

那么为什么使用user.sayHi就会出现问题呢?

让我们来看一看具体obj.method()调用的时候,发生了什么?

仔细看,可以发现,在obj.method()语句中,存在两个操作符,一个是.(dot),另外一个是()括号.

1. 首先,.点号操作符将会属性obj.method;
2. 然后,()括号操作符就会执行它.

那么,this相关的信息,是如何在第一部分传递到第二部分上?

下面,将两个操作符分来来进行使用,看一看具体发生了什么?

    let user = {
        name: 'xiong',
        sayHi(){
            alert(this.name);
        }
    }

    //将得到方法属性与调用方法两部分分开
    let sayHi = sayHi;

    sayHi();

为了能够使得user.sayHi()正常工作,JS使用了一个小戏法,点号.返回的并不是一个函数,而是一个引用类型.

引用类型由三部分组成:(base,name,strict);
1. base: 表示对象
2. name: 表示属性
3. strict: 表示是否是严格模式

因此,user.sayHi的返回类型其实并不是一个函数,而是一个引用类型:

    //引用类型值
    (user, sayHi, true);

当在引用类型上使用了括号(),则会得到这个函数的所有信息,包括对象本身以及对象的方法.

因此,其他一些操作符,如赋值,将sayHi = user.sayHi; 将会把引用类型作为一个整体给遗弃掉,因此,this会被遗失掉.

所以,若想要this拿到期待的值,必须直接调用obj.method()或者使用obj['method'];二者是完全等价的.

关于箭头函数,它是不存在this的.

箭头函数有点特殊,它本身并不包含this,如果我们在箭头函数中使用了this,那么需要在再包裹一个外层的函数.

    let user = {
        name: 'xiong',
        age: 23,
        sayHi(){
            let arrow = () => alert(this.name);
            arrow();
        }
    }

    //调用
    user.sayHi();

### 总结

1. 放置在对象中,作为它属性的函数称之为方法.

2. 对象的方法用于描述对象的行为,如obj.doSomething();

3. 方法中可以使用this来表示当前对象

4. this的具体值在运行时才会定下来

5. 当定义函数的时候,它可能会使用到this,但是只有在使用的时候,this
的具体值才会定下来, 否则它是没有值的.

6. 函数可以在对象与对象之间重复使用

7. 箭头函数arrow并没有this这个概念.

### 任务练习

1. 语法检查

下面这段代码的输出是什么?

    let user = {
        name: "John",
        go: function() { alert(this.name) }
    }

    (user.go)()

