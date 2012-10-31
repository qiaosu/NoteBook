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

###<div style="border-top: dotted 1px #ccc;margin-top:2em;">Backbone.Event</div>