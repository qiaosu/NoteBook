Seajs
=======

哎，以后终归是要用seajs了，记点笔记吧。但是我推崇的define写法在seajs里并不推荐啊，啊啊啊~

####Api

<https://github.com/seajs/seajs/issues/266>

**1. seajs.config**

配置项。<https://github.com/seajs/seajs/issues/262>

<pre><code>seajs.config({
  alias: {	//别名简化
    'es5-safe': 'es5-safe/0.9.2/es5-safe',
    'json': 'json/1.0.1/json',
    'jquery': 'jquery/1.7.2/jquery'
  },
  preload: [	//提前加载
    Function.prototype.bind ? '' : 'es5-safe',
    this.JSON ? '' : 'json'
  ],
  debug: true,	//调试模式
  map: [	//隐射文件，也是用于调试
    ['http://example.com/js/app/', 'http://localhost/js/app/']
  ],
  base: 'http://example.com',	//根路径，一般不用
  charset: 'utf-8'	//编码
});</code></pre>

推荐把config和seajs放在一起加载：

<pre><code>&lt;script src="path/to/??sea.js,sea-config.js"&gt;&lt;/script&gt;</code></pre>

**2. seajs.use**

加载入口模块，通常只在 index.html 里，引入 sea.js 之后，通过 use 方法来加载模块：

<pre><code>seajs.use('./a');

seajs.use('./a', function(a) {
  a.doSomething();
});

seajs.use(['./a', './b'], function(a, b) {
  a.doSomething();
  b.doSomething();
});</code></pre>

**3. define**

<pre><code>define(function(require, exports, module) {
  // The module code goes here
});</code></pre>

**4. require**

<pre><code>define(function(require) {
  var a = require('./a');
  a.doSomething();
});		//推荐写法?</code></pre>

**5. require.async**

<pre><code>define(function(require, exports, module) {
  // load one module
  require.async('./b', function(b) {
    b.doSomething();
  });

  // load multiple modules
  require.async(['./c', './d'], function(c, d) {
    // do something
  });
});</code></pre>

**6. exports**

<pre><code>define(function(require, exports) {
  // snip...
  exports.foo = 'bar';
  exports.doSomething = function() {};
});</code></pre>

或者：

<pre><code>define(function(require, exports, module) {
  // snip...
  module.exports = {
    name: 'a',
    doSomething: function() {};
  };
});</code></pre>


####最佳实践
<br/>

`遵循规范`

遵循CMD模块规范：
<https://github.com/seajs/seajs/issues/242>

`模板`

使用文本插件管理Templates：
<pre><code>seajs.config({
  preload: ['seajs/plugin-text']    //激活插件
});</code></pre>

现在可以直接加载文本文件了。
<pre><code>define(function(require) {
  var tpl = require('./a.tpl');
  // do sth.
});</code></pre>

a.tpl：

<pre><code>&lt;div&gt;I am {{name}}.&lt;/div&gt;</code></pre>

`coffee`

用了seajs的一大好处是以后可以尝试coffeescript开发了。激活插件：

<pre><code>seajs.config({
  alias: {
    'coffee': 'coffee/1.3.3/coffee-script'
  },
  preload: ['seajs/plugin-coffee']
});</code></pre>

a.coffee：

<pre><code>define (require, exports) ->
  exports.foo = 'bar'
  return</code></pre>

注意：coffee 文件的最后要加一个 return 语句。
