JavaScript类库笔记
=======

###<div style="border-top: dotted 1px #ccc;margin-top:2em;">klass</div>
`class`

<https://github.com/ded/klass>

用途：Creating expressive classes in JavaScript.

关于类构造，我的基本要求：

1. 无所谓加不加new关键字，或者使用Object.create，仅要求固定并只支持固定一种格式；
2. 有extend方法提供类继承，正确的instanceof关系；
3. 能通过函数或对象字面量的方式初始化类；
4. 实例化的时候默认执行initialize方法；
5. 有spur的解决方案。

构造一个类的基本方法：

<pre><code>var Person = klass(function(name){
  this.name = name;
});
var john = new Person('john');
john instanceof Person //true</code></pre>

也可以通过对象字面量直接创建一个类：

<pre><code>var Foo = klass({
  foo: 0,
  initialize: function() {
    this.foo = 1
  },
  getFoo: function () {
    return this.foo
  },
  setFoo: function (x) {
    this.foo = x
    return this.getFoo()
  }
});</code></pre>

initialize方法会在实例创建时默认执行。

源代码：

<pre><code>function fn() {
  if (this.initialize) {
    this.initialize.apply(this, arguments)
  } else {
    //...
  }
}</code></pre>

extend继承：

<pre><code>var Foo = klass({
  initialize: function() {
    this.fly = 'fly';
  }
});

var Bar = Foo.extend({
  initialize: function() {
    this.supr();
    this.swim = 'swim';
  }
});

var bar = new Bar();
bar.fly(); //'fly'
bar.swim(); //'swim'
</code></pre>


以上基本满足我的需求。此外，klass提供了额外的特性：

1. 通过.methods方法添加类方法；
2. 通过.statics方法添加类变量；
3. 通过.implement方法实现mixin。

`todo` 尝试提供一个最小集。

###<div style="border-top: dotted 1px #ccc;margin-top:2em;">Arale.Event || Backbone.Event</div>
`event`

<https://github.com/aralejs/events>

用途：支持自定义事件。

基本要求：on, off, trigger。<br>
附加：可以针对特定的类或实例推送自定义事件。

用法1：直接实例化一个全局监听事件的对象：
<pre><code>var eventObj = new Events();
eventObj.on('expand', function() {
  alert('expanded');
});
eventObj.trigger('expand');</code></pre>

用法2：混入其他对象

<pre><code>var View = klass({  
  initialize: function(){
    this._id = 'View';
  },
  subscribe: function(){},
  bind: function(){}
}); //Create a class
Events.mixTo(View); //Event mixin</code></pre>

针对特定类或实例推送：

<pre><code>var ViewA = klass({
  letBFly: function(){
    window.vB.trigger('FLY');
  }
});
var ViewB = klass({
  initialize: function(){
    this.subscribe();
  },
  subscribe: function(){
    this.on('FLY', this.fly);
  },
  fly: function(){
    alert('fly');
  }	
}); //Create classes
Events.mixTo(ViewA);
Events.mixTo(ViewB); //Event mixin
var vA = new ViewA();
var vB = new ViewB();
vA.letBFly();
</code></pre>

额外的特性：

1. 支持通过all事件来监听所有事件；

	<pre><code>allObj.on("all", function(eventName){
	  console.log("event name is " + eventName);
	});</code></pre>

2. 同时触发多个事件;
	<pre><code>tObj.trigger('eventA eventB eventC');</code></pre>

3. off方法不传参即可卸载绑定在对象上的所有自定义事件。

此外：

如果自定义事件较多，官方推荐使用命名空间来管理，比如dance:tap。

个人感觉除了All这个不怎么用到，其他基本上就已经是最小集了。
	