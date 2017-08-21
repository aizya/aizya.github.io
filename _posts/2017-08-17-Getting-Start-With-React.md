### React学习小记

React是个好东西,虚拟DOM让我们能够很快速的渲染出一个组件,并且尽可能的避免其他多余的开销.. 这些玩意都是从React的官网上看到的,具体性能怎么样,我一个前端小白哪里知道这么多..

相对而言,React的学习性价比还是比较高的,相对于Apache的Cordova,React现在是越来越火了,所以这就是慕课网,网易云那些货收费的原因吗?哼哼,是的,这么好的东西,凭什么给你免费,你算老几....

言归正传,JS我学得很烂,我刚开始学Java的时候,还真的看不起JS,现在,哈哈哈.. JS的语法真的有点绕,太灵活了...至于那些Vue, Angular...我都不会...

没事,不就是一个小小的V(View)框架,学完React就跟着学Flux, React Native... 不会差呢...

#### Components

一个页面可以由若干个不同的组件组成..组件在React的重要之处不言而喻..在Java中,可以通过new的方式创建一个Class,也可以通过反射,还可以通过序列化,克隆...与Class一样,Component的创建方式也有很多种,不仅可以通过ES6的Class形式,还可以通过JSX语法噢...反正有一点,尽量提高Component的可复用性...

每一个Component都有一个render函数,在这个函数中返回的内容将会被作为HTML显示在页面上,当然,也可以返回其他的Component.

Tip: 本篇入坑笔记的所有组件创建的前提都是在CodePen这个网站的前提下运行的:
    
    <div id="root">
        <!-- This div's content will be managed by React. -->
    </div>

不逼逼了,先撸一个HelloWorld(你问我为什么是HelloWorld, 因为,我只会这个...)

    ReactDOM.render(
        <h1>Hello World </h1>,
        document.getElementById('root')
    );

当然也可以把渲染部分提出来:

    const first =  <h1> Hello World! </h1>;
    ReactDOM.render(
        first,
        document.getElementById('root')
    );

#### JSX

上面这个HelloWorld,'麻雀虽小,五脏俱全'. 里面有个让人觉得很奇怪的用法,直接将HTML语法赋值给一个对象: 

    const first = <h1> Hello World </h1>;

这种形式的写法,即不是字符串,也不是HTML.在React中被称之为JSX(JavaScript + XML). 这种方式相对来说,渲染起来非常的快,而且非常的方便.

你还可以在JSX中嵌入任何的JS语法,如React官网上这个例子:

    //先定义一个用户对象User
    const user = {
        firstName: 'zhang',
        lastName: 'yue'
    }

    //然后在定义一个函数,格式化输出用户的名称
    function formatName(user){
         return user.firstName + '  ' + user.lastName;
    }

    //定义一个Component,通过大括号使用js函数
    const nameComponent = (
        <h1>Hello, {formatName(user)}</h1>
    );

    //进行渲染
    ReactDOM.render(
        nameComponent,
        document.getElementById('root')
    );

定义nameComponent的时候,使用了一个圆括号将多行数据扩了起来,这样可以有效避免JS自动为每行末尾添加分号的问题.(其实我也没有发现没写有什么错误..)

还可以在function中加入判断,以选择该输出什么JSX内容:

    //增加另外一个Component,内容经过判断决定
    const sayLove = (
      <h2> {greeting(user)} </h2>
    );

    //在函数中加入JSX
    function greeting(user){
        if(user){
            return <h1> woai {formatName(user)} </h1>
        }else{
            return <h1>  nenenen </h1>
        }
    }

    //进行多组件的渲染
    ReactDOM.render(
      <div>
        {nameComponent}
        {sayLove}
      </div>,                           
      document.getElementById('root')
    );

针对上面这段代码,有几个小tip:

1. 可以在大括号里面,添加任何JS的代码,包括函数调用,还可以用来嵌套组件.
2. 尤其嵌套组件(调用)的使用,我试了很多方法,都不行.
3. 可以在函数中使用JSX

#### 使用JSX指定属性

1. 方法一:

        const element = <div className="zhangyue"> baobeiyoushengqile </div>

2. 方法二:
        
        