JavaScript
=======

`问题` X-UA-Compatible设置导致标准doctype未生效

模式、版本不同，不仅仅css解析不同，js的解析也有不同。
<pre><code>&lt;meta http-equiv="X-UA-Compatible" content="IE=EmulateIE7" /&gt;</code></pre>
设置X-UA-Compatible的做法会导致某些浏览器中不识别<!DOCTYPE html>

对样式及脚本都有影响，如对input[type=text]的样式不生效与iframe无法自适应高度等。

[参考链接](http://dancewithnet.com/2009/06/14/activating-browser-modes-with-doctype/)