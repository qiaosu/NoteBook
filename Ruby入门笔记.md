Ruby入门笔记
=======


###语法特性
<br/>

`小标题` 直接字面量

ruby中直接字面量有整数，浮点数，字符串，字符，true，false，nil
但是ruby是面向对象语言，所有值都是对象，没有基本类型与对象类型的区别。

例如需要求绝对值，在javascript中：<br>
<pre><code>var num = -1234;
num = Math.abs(num); //=>1234</code></pre>
而在ruby中：<br>
<pre><code>num = -1234.abs //=>1234</code></pre>

<br>

`小标题` 注释

\#后面是单行注释<br>
=begin 和 =end 之前的是多行注释

<br>

`小标题` 变量名与方法名约定

$ 全局变量<br>
@ 实例变量<br>
@@ 类变量，类变量在使用前必须初始化<br>
? 通常以?结尾的方法返回布尔值<br>
! 通常以!结尾的方法会改变调用他们的对象， 不包含!的方法则修改原始对象的一个拷贝并返回<br>
= 调用=号结尾的方法时可以省略此等号，通常置于操作符左侧。


<br>

`小标题` 方法的返回值

方法中最后一个表达式的值，就是方法的返回值。<br>
你也可以使用return语句，这样方法的返回值就是return语句的参数。

<br>

`小标题` 递增递减的算术表达式

ruby中没有++，--的赋值表达式，如javascript中的i++，--i等都是语法错误的。<br>
请使用+=，-=来代替。

<br>

`小标题` 并行赋值

<pre><code>x,y,z = 1,[2,3]				# x=1;y=[2,3];z=nil
x,(y,z) = 1,[2,3]			 # x=1;y=2;z=3
a,b,c,d = [1,[2,[3,4]]]		 # a=1;b=[2,[3,4]];c=d=nil
a,(b,(c,d)) = [1,[2,[3,4]]]  # a=1;b=2;c=3;d=4</code></pre>

<br>

`小标题` 区间

Ruby区间存在于任何地方，如：1到12月。Ruby用区间实现了3个不同的特性：序列，条件，间隔。<br>
".."：两个点号创建一个闭区间a..b——[a,b]<br>
"..."：而三个点号创建一个右开区间(即右边界不取值）a...b——[a,b)<br>
to_a方法可以把区间转换成列表

<br>

`小标题` Rake集成Rspec

创建一个名为"Rakefile"的文件。

<pre><code>require 'rspec/core/rake_task'
RSpec::Core::RakeTask.new(:spec)
task :default => :spec</code></pre>

创建测试文件"spec/thing_spec.rb"。

<pre><code>describe "something" do
  it "does something" do
    # pass
  end
end</code></pre>

运行rake。 > bundle exec rake


###OO与模块化
<br/>

`Module` The Most Important Kind of Object

<pre><code>module Dog
  def self.bark
    "bow wow"
  end
end</code></pre>

一个基本的Module例子，其中self.bark定义的是类方法。类方法也叫做静态方法，通过类名来进行调用。而实例方法，则需要new出来一个实例后才能使用。

__Module的定义是一段立即执行代码。__

__Module是一个命名空间。__

<br/>

`Class` Module + Inheritance

<pre><code>class Dog
  def self.bark
    "bow wow"
  end
end
class Poodle < Dog
end
puts Poodle.bark #=> bow wow</code></pre>

__一等公民__

与类方法，静态方法类似的是类变量与实例变量的组合。@@Variable表示类变量，无需new实例出来即可以使用，而@表示实例变量，需要配合实例使用。

<br/>

`Mixins`

Ruby中通过include一个模块来实现Mixin。

<pre><code>module Flyer
  def fly
    "wheeee!"
  end
end
class Bird
  include(Flyer)
end
eagle = Bird.new
puts eagle.fly #=> wheeee!</code></pre>

并且当一个模块发生改变时，这种改变通常会立刻影响到Mixin这个模块的所有对象。

<pre><code>module Flyer
  def fly
    "wheeee!"
  end
end
class Bird
  include(Flyer)
end
eagle = Bird.new
Kernel.puts(eagle.fly) #=> wheeee!
module Flyer
  def land
    "aaaaah"
  end
end
puts eagle.land #=> aaaaah</code></pre>

<br/>

`Singleton`

Singleton: 在某种情况下可能需要某个特定的类实例拥有一些方法，而同样继承自此类的其他实例则没有。

几种创建Singleton单例的方式，基本方式是Ruby中允许给实例单独添加方法。

<pre><code>class Dog
  def bark
    "bow wow"
  end
end
fido = Dog.new
rover = Dog.new
def fido.fetch
  "pant pant slobber slobber"
end</code></pre>

或者：

<pre><code>class << fido
  def fetch
    "pant pant slobber slobber"
  end
end</code></pre>

<pre><code>module Fetcher
  def fetch
    "pant pant slobber slobber"
  end
end
fido.extend(Fetcher)</code></pre>

